+++
title = "Handling USB flash drives with usb-rofi"
date = "2025-7-18"
+++

### Brief context

Desktop environments (DE) like `KDE`, `Gnome` and `XFCE` are popular choices for `Linux` users to set their own workspaces, due to the rich ecosystem provided by them, that are used to be composed by user applications — like file managers, application launchers, text editors and terminal emulators —, system applications (which are not launched directly by the user) — like window managers, system monitoring tools, and so on. In short: they brings a lot of facilities to the distro users, mainly for those who are not interested to deal with the complexity of the operating system to do some tasks and just wants everything working fine with the minimal effort to achieve it.

However there's life outside the DE world. For the users who prefer to keep a fine grainded control about every package installed and every single configuration made on the system (me, for example), a tiling window manager with some applications and configuration files is a good and pretty common choice. For this scenario, a problem I used to face (and I guess it's a pretty common issue) was the lack of a mechanism to handle USB events (device attaching/dettaching) on my system and to solve this problem, the `usb-rofi` has born.

### usb-rofi

[usb-rofi](https://github.com/andrelcmoreira/usb-rofi) is a simple tool to help the management of USB flash drives based on `rofi` app launcher and `udev`. Currently on version `0.1.1`, it's composed by an udev rule, which is responsible to trigger the `usb-rofi` script when a new device is attached to the system, and the script itself, which will prompt the user for the action to be taken over the new attached device (mount or ignore).

> **NOTE**: So far, the tool was tested only on Void Linux — which as my daily driver, was my main target of testing — and Debian. Feel free to test the tool by yourself and report issues by e-mail or GitHub repository.

### Setting the things up

After clone the repository, just run:

```bash
$ make usb-rofi
$ sudo make install
```

### Roadmap

For the next releases, support for MTP devices will be added.
