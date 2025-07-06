+++
title = "Handling USB flash drives with usb-rofi"
date = "2025-7-18"
+++

### Introduction

Desktop environments (DE) like `KDE`, `Gnome` and `XFCE` are popular choices for `Linux` users to set their own workspaces, due to the rich ecosystem provided by them, that are used to be composed by user applications — like file managers, application launchers, text editors and terminal emulators —, system applications (which are not launched directly by the user) — like window managers, system monitoring tools, and so on. In short: they brings a lot of facilities to the distro users, mainly for those who are not interested to deal with the complexity of the operating system to do some tasks and just wants everything working fine with the minimal effort to achieve it.

However there's life outside the DE world. For the users who prefer to keep a fine grainded control about every package installed and every single configuration made on the system (me, for example), a tiling window manager with some applications and configuration files is a good and pretty common choice. For this scenario, a problem I used to face (and I guess it's a pretty common issue) was the lack of a mechanism to handle USB events (device attaching/dettaching) on my system and to solve this problem, the `usb-rofi` has born.

### usb-rofi

[usb-rofi](https://github.com/andrelcmoreira/usb-rofi) is a simple tool to help the management of USB flash drives using the `rofi` app launcher and `udev`. It's composed by the udev rule `usb-rofi.rules` and the script `usb-rofi`. The rule is responsible for trigger the script when a new device is attached to the system, which in turn will prompt the user for the action to be performed. The main usage for this tool is for tiling window managers such as i3, dmw, bspwm, etc.

### Hands on
