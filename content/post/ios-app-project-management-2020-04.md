+++
title = "Structuring an iOS App Project in 2020"
date = 2019-04-12T12:18:28+09:00
draft = true 
tags = ["ios", "app", "automation"]
categories = []
+++

Kondo Marie is famous for her talks on organizing the home for a more fulfilling life. One of her axioms is keeping only what "sparks joy". Well, it's 2020 and you should have more joy in life. In this post, I'll talk about the keys to iOS app development happiness.

## Themes and Goals

What is joy? For team-based iOS app development, I define joy as the following things:

1. Reproducable builds on any machine
2. Ease of deployment and onboarding
3. Low maintenance cost

<..details..>

## Initial Settings

First, the bare essentials. These are fairly self-explanatory, so I'll go over them as a list.

- `.gitignore` 
  - from the (standard GitHub template)[link]
  - You are using version control, right? Even if you are a beginner, taking the time to learn a common version control tool like git will make your life easier.
- `README.md` 
  - This should include basic instructions to get setup for development and a snazzy logo
  - For open source, a friendly and detailed README is key, so starting out with (a common template)[] is a good idea 
  - Also, for open source, you'll want a guide to contributing (`CONTRIBUTING.md`) and a few more documents
- `.github/ISSUE_TEMPLATE.md`
  - <links, explanation>
- `.github/PR_TEMPLATE.md`
  - <links, explanation>

## The Project File

Here is perhaps the most controversial part of my post. If you plan on developing an iOS app with multiple people in 2020, I recommend using a tool like [XcodeGen]() or [Tuist]() to generate your Xcode project files, then adding your `.pbxproj` file to your gitignore. Why? I think the tools above solve some significant shortcomings with Xcode's pbxproj files that far outweigh potential problems and risks.

### Problems XcodeGen and Tuist solve

1. `.pbxproj` files are inherently prone to merge conflicts.
  - Yes, I know using union merge strategy or a tool like mergepbx for pbxproj files will help a lot, but these are never 100%, and having to checkout the branch then solve the conflict locally then push to GitHub is a major time sink that could be avoided altogether.
2. Keeping a `.pbxproj` around means that you are willing to put up with a certain number of implicit project settings that are very difficult to track or notice when looking at file diffs
  - Putting build settings in `.xcconfig` files definitely helps, but there will always be settings that live only in the `.pbxproj` file
3. Making sure files are always organized and that file references always exactly match the file system structure is extremely difficult. 
  - A missing file reference could lead to cryptic errors that show up at just the wrong time

### Problems XcodeGen and Tuist cause

- new versions of xcode
- dependency on new tool
- generation times

## Dependencies

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

The point of this one is that a). the project is broken up in different modules based on main features and b). each screen has its own folder in which the ViewController, ViewModel (or presenter or whatever), storyboard (if you use 'em), and such reside. In my experience, this method scales far better that making separate folders for ViewControllers, ViewModels, and whatnot. 

As for modules, whenever the app reaches a certain size, it really pays off to have a project split up to keep incremental build times down and enforce separation of concerns. Another benefit of multi-modules is naming. ViewController names can get very long in a monolithic app, but when the project is split into modules, avoiding name collisions is as simple as prefacing the object name with the module name: `Search.ProductListViewController`.

## CI/CD and Automation

## Conclusion
