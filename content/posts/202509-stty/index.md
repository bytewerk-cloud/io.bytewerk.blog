+++ 
draft = false
date = 2025-09-02T11:36:31+00:00
title = "Interactive shell sessions on QNAP"
description = "How to configure your QNAP NAS to allow for interactive SSH sessions"
slug = "qnap-stty"
authors = ["Christian Lohr"]
tags = ["homelab", "personal"]
categories = ["homelab", "personal"]
externalLink = ""
series = []
+++

## The problem 

I run a bunch of stuff on my QNAP NAS to avoid having to purchase an actual server, most of them are containerized self-hosted alternative solutions to popular cloud products, like [Immich](https://immich.app/), [Paperless NGX](https://github.com/paperless-ngx/paperless-ngx), [Glance](https://github.com/glanceapp/glance), and a bunch of others, all behind a convenient reverse proxy.

With great power, however, comes great responsibility. And figuring out a reliable [3-2-1 backup strategy](https://en.wikipedia.org/wiki/Backup#3-2-1_Backup_Rule) often means I have to SSH into my machines to work on configuration files, cronjobs, etc...

Allowing your QNAP NAS to accept incoming SSH connections is easy enough, simply enable the feature in your Web UI settings, specify a port, and boom. I did, however, encounter a slight problem.

Right after connecting to the machine did I realize that there was zero feedback on keyboard input. This is usually caused by a misconfigured `stty` or a missing `$TERM` file. A quick `which stty` shed some (sad) light on this: `which: no stty in ...`. I'm not entirely sure **why** QNAP has settled on omitting this core tool from their images (if you do, please let me know. I'm genuinely interested), but I **do know** that getting it to run was much more troublesome than I had expected.

## The path to enlightenment

To begin with, there is no way to install small core packages like `core-utils` using the web UI. There's also no clear indicating which package manager to use instead. Some internet findings report `qpkg` (which was not present on my machine). I did however encounter `qpkg_cli`, which did allow me to install the same packages that the web UI had access to, so it's clearly their under-the-hood backend for this.

Luckily, there is a community of QNAP enthusiasts which provide open source packages for QNAP machines: [Entware-NG](https://bin.tranducanh.com/). Documentation around it is scarce, but following some discussions on Reddit, I was able to compile it down to this simple list:

- Add `https://www.myqnap.org/repo.xml` to your QNAP App Center repositories
- Update your package lists
- Install the `Entware-std` package via the App Center

This will give you access to a large pool of open source packages, as well as the `opkg` cli tool to install them from the terminal. You may install the core-utils like this:

```sh
sudo opkg update
sudo opkg install core-utils
```

Unfortunately, it appears that `stty` is not included in the `core-utils` package. There is also no `utils-linux` packages present. 

## The (hacky) solution

Alas, not all is lost, since `opkg` does provide us with a `busybox`:

```sh
sudo opkg install busybox

# Create a symlink that makes the applet callable as “stty”
sudo ln -sf /opt/bin/busybox /opt/bin/stty

# Add /opt/bin to PATH if it isn’t already
export PATH=$PATH:/opt/bin
echo 'export PATH=$PATH:/opt/bin' >> ~/.profile   # persist for future sessions

# Test
stty -a

```

After making these changes and reconnecting to my machine, I was finally able to actually see what kind of abominable mistypings I continuously sent over the wire. Time for some typing practice I presume ...
