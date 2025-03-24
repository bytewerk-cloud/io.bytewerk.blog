+++ 
draft = false
date = 2025-03-22T16:52:31+01:00
title = "The journey to join the green team"
description = "How to squeeze your hugo blog into less than 100kb."
slug = "512kb"
authors = ["Christian Lohr"]
tags = ["internet", "personal"]
categories = ["internet", "personal"]
externalLink = ""
series = []
+++

Recently, I stumbled across the [512kb.club](https://512kb.club), a collection of performance based web pages from all over the internet. Here's a quote straight from their mission statement:

> But we can make a difference - all it takes is some optimisation. Do you really need that extra piece of JavaScript? Does your WordPress site need a theme that adds lots of functionality you’re never going to use? Are those huge custom fonts really needed? Are your images optimised for the web?

This felt like someone looking into my very soul. The current entry with the smallest footprint is a meager `2.16 kb`, and it [looks better](https://rmooreblog.netlify.app/) than most of the soulless  wordpress pages.

Naturally, I felt tempted to join. My setup is fairly simple and already doesn't pull in anything like tracking, UI frameworks, `jquery` and so on. The requirements to join the club are simple:

> 1. It must be an actual site that contains a reasonable amount of information, not just a couple of links on a page
> 2. Your total UNCOMPRESSED web resources must not exceed 512KB.

Given my stoic ignorance with regards to rule number 1, I set out to focus on rule number 2. The first thing I did was to check the current size of my blog. The result actually not so bad, sitting comfortably at `266.38 kb` of uncompressed resources.

That was already enough to join the club, but I wanted to go further and see whether I would be able to join the green team with less than `100 kb` of uncompressed data transmitted. Checking the network tab of our trusty web inspector, I was able to track down the following culprits:

a) the picture of my pretty face: 66kB (oops!)
b) the [formawesome](https://forkaweso.me/Fork-Awesome/) icons, a fork of the popular [fontawesome](https://fontawesome.com/) icon collection that has since been deprecated, but `coder` still uses: >100kB

Changing my profile picture was fairly straight forward. It's basically my Firefox profile picture, and I just loaded it straight from their servers, which hosted it in a not very web friendly `png` format. I have no excuse for this erratic behaviour, I was probably just lazy. Swiftly converting it to `jpg` and hosting it myself cut down the size to `6.65 kb`, 10% of the original size.

Sadly, the icons were a different story. I essentially had to end up forking the `coder` theme, painstakingly removing all mentions of the `fa-` prefixed rules in the CSS rules and hacking in some replacements where needed, mostly falling back on using simple emojis since they're part of Unicode and easily accessible. The result isn't perfect, but it gets the job done.

With the icon fonts gone and my likeness served in the appropriate format, the uncompressed landing page size was now at `57.84 kB`, which is good enough for me now. Adding the green team banner to the footer to raise some awareness to this project, I'm now officially part of the club, although it raised the size to `58.29 kB`. A price worth paying though.

That being said, I believe there is still plenty of room for improvement. I do not use most of the features provided by `hugo` and `coder` and have started using it mostly for convenience ([although starting to regret it]({{<ref "202501-hosting-this-blog/index.md">}})). Maybe I will try writing my own, custom blog hosting solution someday in the future. It sounds like one of the most common side projects that still sound kind of fun to me. If I do, you'll read about it here first!
