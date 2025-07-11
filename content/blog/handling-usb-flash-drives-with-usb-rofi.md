+++
title = "Handling USB flash drives with usb-rofi"
date = "2025-7-18"
+++

### Brief context

Desktop environments (DE) like `KDE`, `Gnome` and `XFCE` are popular choices among `Linux` users for setting up their own workspaces. These environments offer a rich ecosystem of user-facing applications — such as file managers, application launchers, text editors, and terminal emulators — along with system-level applications (which are typically not launched directly by users), like window managers, system monitors, and more. In short, DEs provide a wide range of conveniences for `Linux` users, especially for those who prefer not to deal with the complexity of the operating system and simply want things to "just work" with minimal effort.

However, there's life outside the world of desktop environments.

For users who prefer fine-grained control over every installed package and configuration made on their system  — like myself — a tiling window manager combined with a few essential applications and configuration files is a good and pretty common choice. In such minimalist environments, one common issue I faced (and I guess it's a pretty common one) was the lack of a mechanism to handle USB events (such as device attachment/removal). To addresss this, **usb-rofi** was born.

### usb-rofi

[usb-rofi](https://github.com/andrelcmoreira/usb-rofi) is a simple tool designed to simplify the management of USB flash drives. It leverages the power of the `rofi` app launcher and `udev`. Currently at version `0.1.1`, the tool consists of an `udev` rule — which triggers the `usb-rofi` script whenever a new USB device is attached to the system — and the script itself, which prompts the user to choose an action (mount or ignore the device).

> **NOTE**: The tool has been tested primarily on Void Linux — my daily driver — and Debian. You're welcome to test it on other distributions and report any issues via email or the GitHub repository.

### Setting things up

After cloning the repository, simply run:

```bash
$ make usb-rofi
$ sudo make install
```

### Roadmap

Support for `MTP` (Media Transfer Protocol) devices is planned for upcoming releases.
