+++
title = "Handling USB flash drives with usb-rofi"
date = "2025-7-18"
+++

### Introduction

Desktop environments (DE) like `KDE`, `Gnome` and `XFCE` are popular choices for `Linux` users to set their own workspaces, due to the rich ecosystem provided by them, composed by user applications — like file managers, text editors and terminal emulators — and system applications (which are not launched directly by the user) — like window managers and system monitoring tools. In short: they brings a lot of facilities to the distro users, mainly for those which are not interested to deal with the complexity of the operating system to do some tasks.

However there's life outside the DE world. For those who prefer to keep the track about every package installed and every single configuration made on the system (me, for example), a tiling window manager with a bunch of configuration files is the way to go. A problem I used to face (and I guess it's pretty common issue) was the lack of a mechanism to handle USB events (attach/dettach) on my system, and the answer was: `usb-rofi`.

### usb-rofi

With ```usb-rofi```

**usb-rofi** is a simple tool to help the management of USB flash drives using
rofi app launcher and udev. It's composed by the udev rule `usb-rofi.rules`
and the script `usb-rofi`. The rule is responsible for trigger the script when
a new device is attached to the system, which in turn will prompt the user for
the action to be performed. The main usage for this tool is for tiling window
managers such as i3, dmw, bspwm, etc.
