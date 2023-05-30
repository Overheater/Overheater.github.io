---
author: "Ian Pougher"
date: 2023-05-01
title: Spotistore
categories: ["Software"]
dropCap: false
displayInMenu: false
displayInList: true
draft: false
resources:
- name: featuredImage
  src: "Spotistore.png"
  params:
    description: "A screenshot of Spotistore and it's CSV Output"
---



## **Spotistore: a minimalist cross-platform playlist metadata archiver.**

After not having a working PC other than my work laptop for most of the pandemic, I finally built a replacement for my college desktop late last year.  As a test project to set up my development environment, I decided to build this tiny archiver for Spotify.

I started to build this in earnest the week following the [API revocations on twitter]( https://arstechnica.com/tech-policy/2023/01/twitter-says-third-party-apps-broke-long-standing-api-rules-wont-name-rules/), which single handedly broke hundreds of different apps, utilities, clients and projects the community had built over the years. I decided that I should probably have a backup of all the playlist and music info that I had listened to in the 14 years I've had a spotify account, and that I should build it relatively quickly since many large company's APIs were getting more and more restrictive.


To build the application, I decided to check back in on [Avalonia UI](https://avaloniaui.net/), a .NET UI framework that targets cross-platform operability. I had tried the early beta of Avalonia back in 2019, and had bad experiences with instability, as well as errors when trying to use the framework on linux. It's been four years full of updates since my last attempt with Avalonia however, and there's not many (if any) other frameworks for cross platform UI.

The initial version of the application is very basic. It takes the user's inputted playlist ID, and outputs a CSV file with the metadata of each track within the playlist, as well as a link to Discogs for the Physical release of the track album/single.

Further improvements that are targeted include an executable release, customizable CSV columns, and a Spotify profile navigator/batch archiver.

Repository: [*Github Link*](https://github.com/Overheater/SpotiStore)

Language: **C#**

Frameworks: **Avalonia**
