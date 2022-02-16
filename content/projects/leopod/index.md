+++
author = "Christopher Leggett"
title = "Leopod"
date = 2022-02-16T04:06:38Z
description = "Podcast Organizer for Elementary OS"
tags = ["podcasts", "vala", "linux", "elementary"]
categories = ["Portfolio"]
github = "https://github.com/leggettc18/leopod"
image = "leopod.png"
portfolio_score = 900
+++

Leopod is a podcast subscription organizer and player written from the ground up for Elementary OS, though
it should be usable on any Linux Desktop thanks to Flatpak. Currently you can add feeds and it will check for updates
while the app is open, and you can download and play episodes. It will also remember the last played position of an
episode you didn't finish. The current version is 1.0.1 and a 1.1 version with some smoother UI interactions and the
ability to speed up playback is on the way.

<!--more-->

Eventually, I want the app to be able to sync with GPodder, export and import opml files, and do more automatic
library management tasks (automatically deleting played podcast episodes, refreshing feeds and downloading episodes
while the app is closed).

This app is a demonstration of my ability to quickly adapt to new technologies. This was my first app written in Vala,
my first time using the meson and ninja build tools, and my first time packaging an app with Flatpak. It is also my
first Desktop app ever (everything else I've worked on has been web apps). These are all new technologies for me,
and I was able to put them all together to develop, build, and publish a working application. You can check it out
on the Elementary AppCenter, or at the [AppCenter website](https://appcenter.elementary.io/com.github.leggettc18.leopod/).
