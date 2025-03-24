+++ 
draft = false
date = 2025-02-13T20:52:31+01:00
title = "The Horrors of hosting this blog"
description = "Why one click deployments tend to suck."
slug = "hosting-this-blog"
authors = ["Christian Lohr"]
tags = ["internet", "personal"]
categories = ["internet", "personal"]
externalLink = ""
series = []
+++

As you can probably tell by my posting history, I do not update this page too frequently, and often take long breaks from writing. Whenever I do come back to maintaining this web presence, I'm starting to sweat a little. But let's start at the beginning.

A long time ago, I wrote a small web platform using [MeteorJS](https://www.meteor.com/), on which I spent roughly half a year of productive work on my free days. It was a proof-of-concept for a digital marketplace revolving around the idea of reward tokens & loyalty programs. As is often the case, some real life stuff happened, and my focus was shifted elsewhere as the world kept turning.

Two years later, I decided to revisit the project. So I checked out the code and ran `npm install && npm run start` (which was the style at the time, before `yarn`, `npx` and `bun` came around). 

![simpsons memes have feelings too](images/202501_01.jpg)

As you can  probably imagine, absolutely nothing worked. Node had moved on a couple of versions in the meantime, and I got breaking dependencies left and right and abandoned libraries in favour of new, more exciting stuff. It took me over an hour to simply get the compilation to run, with a lot of components still breaking during runtime.

Now, to be clear, this post is not a rant about Meteor or NodeJS, both technologies are good at what they're trying to accomplish (although personally, I wonder whether running JS on the backend was really needed). But this experience really sharpened the understanding that a fast lived ecosystem like Node greatly benefits from a constant exposure to it. Stepping away for a while and coming back to it is not always easy.

Part of the reason I enjoy technologies like `Go` and `Rust` is that the language evolution itself is highly stable and backwards compatible. I can pick up a project I wrote 5 years ago and it will still compile and run, with the same performance characteristics as before, despite having a much more modern version of the compiler. This paradigm transpires into the community, with most packages being maintained in a similar way, leading to mostly painless upgrade paths.

When am I getting to the point? Well, when I picked up framework for hosting this blog, I went with [hugo](https://gohugo.io/), for two reasons: I am able to write content in markdown files, and I end up with a static HTML page I can host myself. What I didn't realise back then is that there is an entire ecosystem which surrounds hugo. You can add plugins, use custom themes, automate the build process with CI and continuously deploy the site to a hosting provider of your choice.

All of that is great - but comes again with a cost. You have to familiarise yourself with how all of these technologies interact with each other, and how to debug them when things go wrong. And things will go wrong, trust me. Pipelines break, you may want to customise a thing or two about the theme you're using and you will come across a cryptic error message every so often.

This blog had been laying dormant for almost a year before I decided to pick it up again, and when I did, my experience was very similar to NodeJS. If you don't work regularly work with these tools, you will need to re-familiarise yourself with them, and fix any breakages that may have occurred in the meantime. Customising my theme [to reduce the page size]({{<ref "202501-512kb/index.md">}}) turned out to be an even bigger nightmare, as you have to really dig into the stack and understand how everything comes together under the hood. 

In general, I'm not a fan of this amount of abstraction. I get that there is a large market for solutions like `hugo` for situations where this amount of control is not required, and you just want to have a straight-forward pipeline. But for me, it's just another thing that can break. I've written my own solutions for accounting and a bunch of static websites (using `Go` and `hmtx`), and all of these projects compile fine to this day. I can step away from them for years, only to find myself right at home when I get back to them. 

The reason for this is that I've written most of the code myself, in a language I am familiar with, with a minimal subset of stable, slow evolving technologies (only HTML, plain CSS, plain JS where needed), no exotic framework or dependencies. With very few building blocks and explicit, functional code, there is no guesswork about what happens under the hood, or what black magic is being used to achieve dependency injection or hot reloading.

And I understand this approach is not for everyone. You might be looking for much more elaborate features on your platform than me, and you might be willing to pay the price in complexity for that. But for me and my needs, the very fast and tidy `hugo` already feels like a bit too much.
