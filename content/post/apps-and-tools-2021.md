+++
title = "Apps and Tools I Use"
date = 2021-02-09T11:00:00+09:00
draft = false 
tags = ["tooling", "app", "productivity", "CLI"]
categories = []
+++

I use a variety of apps and tools to develop for iOS, learn backend development, and do general computing. 

## Command Line

Details can be found in my [dotfiles](https://github.com/yhkaplan/dotfiles), but some of the top command line tools I use (besides the standard ones) are:

### CLI

- ansible: Very much a work in progress on my part, but used to automate setting up new environments quickly.
- autojump: Use `j directory-name` to quickly `cd` into the chosen directory. Better yet, the full dirname isn't needed so you could type `j desk` to jump to the Desktop for example.
- docker: The popular container runtime.
- fd: Much faster and easier-to-use version of `find` written in Rust.
- fzf: A "fuzzy-finder" used with a veriety of scripts to quickly open files, switch branches, and more.
- gh: GitHub's official CLI. I use it to open PRs mostly.
- git: version-control.
- homebrew: top macOS package-manager.
- jq: formatting and filtering JSON.
- mosh: similar to SSH, but works better with spotty networks connections when connecting to a raspberry pi or from an iPad for example.
- neovim (nightly): vim, but with built-in LSP support that works like a charm.
- ripgrep: much faster version of grep, built with Rust.
- [scaffold](https://github.com/yhkaplan/scaffold): the template-generation tool I built.
- tmux: Terminal multiplexing.
- trash: The much less scary way of deleting files than `rmdir` and `rm`. Looking for a Linux version.
- zsh: The more modern, yet POSIX-compliant shell. Better wildcard-handling than Bash and better plugin support.

### Zsh plugins

I use the following Zsh plugins to make my shell feel like Fish, but fully POSIX-compliant. Admittedly, Fish is faster to start up and has better performance sometimes, but a) learning Fish syntax feels like a waste of time for some obscure knowledge and b) POSIX compatibility means I can copy-paste and easily adapt example scripts and commands in an instant.

- denysdovhan/spaceship-prompt
- momo-lab/zsh-abbrev-alias
- zdharma/fast-syntax-highlighting
- zsh-users/zsh-autosuggestions
- zsh-users/zsh-completions
- zsh-users/zsh-history-substring-search

## macOS

- Alacritty & iTerm 3: Alacritty is quick but iTerm fuller macOS compatibility.
- Bear: Favorite notes app.
- Chrome & Safari: Chrome for websites that work better on it, but I expect to switch fully to Safari when I get an Apple Silicon Mac.
- Fantastical: Way better calendar app than the default one.
- Proxyman: The best network-proxy app I've tried. Great for debugging network requests.
- Sherlock: Similar to Reveal. Debug and change UIViews in real time.
- Slack: Chat.
- Things: Way better Todo app than Reminders. I don't like weird web-apps like Todoist.
- VSCode: Text editor for when neovim syntax highlighting can't keep-up with large markdown documents.
- Xcode: iOS development.

## iOS

- 1Password: I don't use the macOS version that much because I don't trust automatic completion inside web-browsers. I use Handoff to copy-paste between iOS and macOS.
- Apollo: Best Reddit client.
- Bear: Favorite notes app.
- Default Weather, Podcast, Mail, Music, and Safari apps.
- Facebook Messenger: Because it's the only way to keep in touch with all my college friends.
- Fantastical: Fastest app to input a new event.
- Google Maps: Best mapping data.
- Japanese: Simple Japanese <-> English dictionary.
- LINE: The popular messeging app in Japan.
- Reeder: Simple but beautiful RSS reader.
- Skype: VOIP app my parents understand. 
- Slack: Chat.
- Things: Better Todo app than Reminders.
- Twitter: First-party Twitter-client.
- Yahoo News (Yahoo Japan app): Convenient way to browse Japanese-language news. Most other apps for this are garbage.
- YouTube: I subscribe to Premium. No ads.

## iPadOS

In addition to the iOS apps above, some iPad-only or iPad-centric apps I use are:

- Buffer and Textastic: Text editors. Buffer has vim keybindings 😻 but can be slow.
- GoodNotes 5: Great Apple Pencil note-taking.
- Kindle and iBooks: E-Books.
- LumaFusion: Video-editing. Plan on posting more videos of my dog.
- Pixelmator Photo: Great photo-editing app with AI features.
- Procreate: Best drawing app. I use it for diagramming.
- Swift Playgrounds: Prototyping SwiftUI components (dreams of Xcode for iPad...).
- Working Copy: Best git client for iPadOS/iOS.

## Conclusion

That's mostly it! As for general themes, I enjoy trying out new apps and tools all the time and am constantly seeking out the best ones in each category. Great apps and tools are worth paying for and contributing to, but they also must be maintained, high-performance, and stable for me to choose them. Hit me up on Twitter if you have any recommendations!