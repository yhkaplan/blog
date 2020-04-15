+++
title = "Structuring an iOS App Project in 2020"
date = 2020-04-15T11:00:00+09:00
draft = false 
tags = ["ios", "app", "automation"]
categories = []
+++

Kondo Marie is famous for her talks on organizing the home for a more fulfilling life. One of her axioms is keeping only what "sparks joy". Well, it's 2020 and you should have more joy in life. In this post, I'll talk about the keys to iOS app development happiness.

## Themes and Goals

What is joy? For team-based iOS app development, I define joy as the following:

1. Reproducable builds on any machine
2. Ease of deployment and onboarding
3. Low maintenance cost

## Initial Settings

First, the bare essentials. These are fairly self-explanatory, so I'll go over them as a list.

- `.gitignore` 
  - from the (standard GitHub template)[link]
  - You are using version control, right? Even if you are a beginner, taking the time to learn a common version control tool like git will make your life easier in the long run.
- `README.md` 
  - This should include basic instructions to get setup for development and a snazzy logo
  - For open source projects, a friendly and detailed README is key, so starting out with (a common template)[] is a good idea 
  - Also, for open source, you'll want a guide to contributing (`CONTRIBUTING.md`) and a few more documents
- `.github/ISSUE_TEMPLATE.md`
  - <links, explanation>
- `.github/PR_TEMPLATE.md`
  - <links, explanation>

## The Project File

Here is perhaps the most controversial part of my post. If you plan on developing an iOS app with multiple people, I recommend using a tool like [XcodeGen]() or [Tuist]() to generate your Xcode project files, then adding your `.pbxproj` file to your gitignore. Why? I think the tools above solve some significant shortcomings with Xcode's pbxproj files that far outweigh potential problems and risks. 

Among the problems XcodeGen and Tuist solve:

1. `.pbxproj` files are inherently prone to merge conflicts.
  - Yes, I know using union merge strategy or a tool like mergepbx for pbxproj files will help a lot, but these are never 100%, and having to checkout the branch then solve the conflict locally then push to GitHub is a major time sink that could be avoided altogether.
2. Keeping a `.pbxproj` around means that you are willing to put up with a certain number of implicit project settings that are very difficult to track or notice when looking at file diffs
  - Putting build settings in `.xcconfig` files definitely helps, but there will always be settings that live only in the `.pbxproj` file
3. Making sure files are always organized and that file references always exactly match the file system structure is extremely difficult. 
  - A missing file reference could lead to cryptic errors that show up at just the wrong time

Of course, generating your project has disadvantages:

- Dealing with new versions of Xcode requires more work
- Dependency on new tool that each developer needs to install or that could break with a new Xcode version
- Generation times
    - This is mostly a non-issue because a) XcodeGen and Tuist using caching and b) are designed to parse and output very quickly
    
Managing XcodeGen/Tuist and other developer tool version is also very important, so I recommend using (Mint)[].

## Dependencies

This is where things get complicated. The main package managers used for iOS are: Swift Package Manager, Carthage, and Cocoapods. This could really be an article in its own right, but I personally prefer avoiding Cocoapods if possible because it changes the project (adding an xcworkspace) and can make fresh builds longer. Swift Package Manager is very nice in that it uses static libraries, leaving a minimal impact on your app's launch time (unlike dynamic libraries). However, Swift Package Manager does not currently work with close-sourced binaries and static assets. Swift 5.3 will apparently add these features in, so it should be more viable then. 

I like Carthage as it doesn't mess with your project and it's easy to cache for the CI (by commiting built products with git LFS or using a tool like (Rome)[] for example. Swift Package Manager does not have any caching tools at the moment, which I consider a serious deficiency that could significantly slow down your CI builds.

## File Structure

Programmers love arguing about file structure, but I think a common one for apps of any size is as below:

```
$ tree
- SampleApp/
  - Sources/
    - AppDelegate.swift
    - SceneDelegate.swift
- Core/
  - Sources/
    - <lots of swift files>
  - Tests/
- Cart/
  - Sources/
    - Component/
      - CartProductCardView/
        - ProductCardView.swift
        - ProductCardView.xib
    - ScreenA/
      - ScreenAViewController.swift
      - ScreenAViewModel.swift
      - ScreenACoordinator.swift
      - ScreenAViewController.storyboard
  - Tests/
- Search/
  - Sources/
  - Tests/
- Component/
  - Sources/
  - Tests/
- Extensions/
  - Sources/
  - Tests/
```

The point of this one is that a). the project is broken up in different modules based on main features and b). each screen has its own folder in which the ViewController, ViewModel (or presenter or whatever), storyboard (if you use 'em), and such reside. In my experience, this method scales far better than making separate folders for ViewControllers, ViewModels, and what-not. 

As for modules, whenever the app reaches a certain size, it really pays off to have a project split up to keep incremental build times down and enforce separation of concerns. Another benefit of multi-modules is naming. ViewController names can get very long in a monolithic app, but when the project is split into modules, avoiding name collisions is as simple as prefacing the object name with the module name: `Search.ProductListViewController`.

## CI/CD and Automation

The most important tool for automation is (fastlane)[https://fastlane.tools], which can handle:

- Creating the App Store page and managing metadata
- Building and testing your app on CI
- Deploying beta versions to TestFlight
- Renewing and keeping in sync certificates and provisioning profiles
- Taking screenshots in each supported language
- Deploying new versions to the app store
- Much more

Without fastlane, iOS app releases and code-signing management are downright painful and error-prone, meaning the earlier you integrate fastlane, the easier your life will be. The one downside of fastlane is that it is key to manage its version using a Gemfile, and therefore also manage your local version of Ruby, which is work you might not normally need. To manage Ruby versions, I prefer using rbenv as it is very similar to swiftenv and is commonly used. To learn more about fastlane, take a look at its excellent documentation.

This is not strictly necessary, but code generation can be very useful for your project. I personally use [Sourcery](https://github.com/krzysztofzablocki/Sourcery) to generate mocks using the default template and (SwiftGen)[https://github.com/SwiftGen/SwiftGen] to generate type-safe references to assets.

To elimate discussions about style, I recommend using Swiftformat and/or `SwiftLint --format` to automatically format your code and keep it consistent. The setup that has benefited me the most is using a pre-commit git hook to automatically format the files that have changes. Code-formatting might not sound exciting, but it makes code reviews a lot more exciting by removing tedious style corrections. I also recommend using SwiftLint's regular linter functionality to cover areas that cannot be automatically corrected.

Timely and detailed information about crashes is **essential** and Apple's default crash info dashboard is unfortunately too slow to be useful. I recommend Firebase Crashlytics as it is free (though not the easiest to use) and has GCP Function triggers which can, for example, send you Slack alert when a new major crash is found. Also, this pertains to crashes, but I cannot recommend using `phased releases` enough. Phased releases are when, intead of releasing your app to all users at once, you instead release it in a staggered approach. This allows you to find issues and crashes before a new version reaches all users, pausing the release to make a hotfix.

A small, but extremely helpful tool I recommend for every project is (LicensePlist)[https://github.com/mono0926/LicensePlist], which will automatically fetch licenses from Carthage/Cocoapods/Swift Package Manager packages your introduce and handle keeping them up-to-date.

CI (continuous integration) is essential if you're working as a team and even useful if you are working by yourself. For iOS app development, a macOS environment is required to build and test your app. Because of this, mantaining a physical CI machine is very painful, time-consuming, and not recommended unless you have a dedicated person or team to do so. For the vast majority of teams, I think that a CI service is the right choice, with popular options being: Bitrise, CircleCI, GitHub Actions (not quite a CI service per say) and Travis. As for what you should do with your CI, I recommend building and testing each PR at the bare minimum. Other uses include running (Danger)[] to automate parts of your code review and deploying beta and release versions of your app to TestFlight/App Store.

## Conclusion

I went over a lot, because, well, there really is a lot in setting up an iOS project in 2020, be it UIKit or a SwiftUI one. That said, I am probably missing some things, so please feel free to contact me on Twitter with any suggestions. I hope my suggestions spark some joy.
