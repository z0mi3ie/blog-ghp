---
title: "Multi monitor setup w/ xrandr"
date: 2023-05-23
---

## Monitors goal

Last week I picked up a new Yoga ThinkPad to setup as a linux dev station. I
used to use [Arch Linux](https://archlinux.org) throughout university but drifted
away from it since all my career systems have been OSX. I missed the customization
and really always tried to make my `iTerm2`/`vim` configs feel like I was
still in a tiling window manager.

So anyways -- I picked up a ThinkPad and installed Arch + `dwm`. My setup
for all of that will be its own blog post at some point.

I have two external displays and wanted to stand the Yoga up as a "third' display.

Booting up with everything connected to a docker -- unsurprisingly -- things
didn't work out of the box.

## `xrandr` and problems

Enter `xrandr`. My main issue was that all my monitors were listed with `xrandr --listmonitors`:

```
Monitors: 3
 0: +*eDP-1 1920/293x1080/165+0+1080  eDP-1
 1: +DP-3-1 2560/597x1440/336+1920+1080  DP-3-1
 2: +DP-3-2 1920/597x1080/336+0+0  DP-3-2
```

But the monitor `DP-3-1` was displaying black, with "no source" from the connected HDMI.

Everything pointed to it *should* be working. The mouse could even enter the second
blacked out monitor.

Thinking through it, I tried to manually set the `--rate 144` for that output.

And yes! The monitor immediately finds the source and comes on. From there it was
just setting up the orientation through `xrandr` and inverting the Yoga display
since I have it opened like a book.


Here is my final script for initializing it all together:

```
#!/bin/sh

# Setup desk monitors.

MNTR_ASUS="DP-3-2"
MNTR_PRED="DP-3-1"
MNTR_YOGA="eDP-1"

#
# 1. Turn on ASUS 1920x1080 (144hz) monitor by setting the correct refresh rate
# 2. Flip the ThinkPad Yoga's display
# 3. Set monitor's relative positions
# 4. Restart feh
#
xrandr \
    --output ${MNTR_ASUS} --mode 1920x1080 --rate 144 \
    --output ${MNTR_YOGA} --rotate inverted \
    --output ${MNTR_ASUS} --above ${MNTR_YOGA} \
    --output ${MNTR_YOGA} --left-of ${MNTR_PRED}

${HOME}/.fehbg &
```

Simple as that :)

Thanks for reading!
