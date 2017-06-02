---
title: Projects
permalink: projects.html
---
# Cinnamon

Cinnamon is composed a lot of smaller interconnected projects:

## <a href="https://github.com/linuxmint/Cinnamon">Cinnamon</a>
Cinnamon, forked from GNOME Shell, is the "shell" of Cinnamon. It provides the user interface such as panels, hot corners, menus etc. The ui is written in JavaScript, while its core libraries are written in C.

## <a href="http://github.com/linuxmint/cinnamon-screensaver">Cinnamon Screensaver</a>
Cinnamon Screensaver, forked from GNOME Screensaver, is the screen locker you see when you leave the session idle for the a long time. It currently supports loading xscreensaver hacks as well as webkit screensaver. You can also write your own Cinnamon Screensaver plugin without tying to either xscreensaver or webkit.

## <a href="https://github.com/linuxmint/cinnamon-desktop">Cinnamon Desktop</a>
Cinnamon Desktop, fork of GNOME desktop, provides certain useful resources for Cinnamon. Most importantly, it contains the schemas for most Cinnamon components, eg. `org.cinnamon.desktop.screensaver`. It also has a small library that provides certain functions used in, say, Cinnamon Screensaver.

## <a href="https://github.com/linuxmint/cinnamon-menus">Cinnamon Menus</a>
Cinnamon Menus, fork of GNOME menus, contains the `libcinnamon-menu` library, the layout configuration files for the Cinnamon menu, as well as a simple menu editor.

The `libcinnamon-menu` library implements the "Desktop Menu Specification" from freedesktop.org:

- <a href="http://freedesktop.org/wiki/Specifications/menu-spec">http://freedesktop.org/wiki/Specifications/menu-spec</a>
- <a href="http://specifications.freedesktop.org/menu-spec/menu-spec-latest.html">http://specifications.freedesktop.org/menu-spec/menu-spec-latest.html</a>

## <a href="https://github.com/linuxmint/cinnamon-settings-daemon">Cinnamon Settings Daemon</a>
Cinnamon Settings Daemon is a fork of GNOME Settings Daemon. 
It provides many session-wide services and functions that require a long-running process. Among the services implemented by cinnamon-settings-daemon are an XSettings manager, which provides theming, font and other settings to GTK+ applications, and a clipboard manager, which preserves clipboard contents when an application exits. Many user interface elements of cinnamon and cinnamon-settings rely on cinnamon-settings-daemon for their functionality.

The internal architecture of cinnamon-settings-daemon consists of a number of plugins, which provide functionality such as printer notifications, software update monitoring, background changing, etc.  For debugging purposes, these plugins can be individually disabled by changing the gsettings key `org.cinnamon.settings-daemon.plugins.plugin-name.active`, where `plugin-name` is the name of the plugin. To see a list of all plugins, use the command `gsettings list-children org.cinnamon.settings-daemon.plugins`.

Cinnamon Settings Daemon takes the name `org.cinnamon.SettingsDaemon` on the session bus to ensure that only one instance is running. Some plugins export objects under this name to make their functionality available to other applications. The interfaces of these objects should generally be considered private and unstable.

Cinnamon Settings Daemon is a required component of the Cinnamon desktop, i.e. it is listed in the RequiredComponents field of `/usr/share/cinnamon-session/sessions/cinnamon.session`. It is started in the initialization phase of the session, and cinnamon-session will restart it if it crashes.

## <a href="https://github.com/linuxmint/cinnamon-control-center">Cinnamon Control Center</a>
Cinnamon Control Center is a fork of GNOME Control Center. The official control center of Cinnamon in Cinnamon Settings, which is written in python and part of Cinnamon itself. However, some of the modules are still not yet ported to python, and we have to rely on the C modules, which are found here.

## <a href="https://github.com/linuxmint/cinnamon-session">Cinnamon Session</a>
This is responsible for starting the Cinnamon session. This is typically executed by the login manager (either mdm, xdm, or from your X startup scripts). It will load either your saved session, or it will provide a default session for the user as defined by the system administrator (or the default GNOME installation on your system).

## <a href="https://github.com/linuxmint/cinnamon-translations">Cinnamon Translations</a>
Cinnamon translations is a package that contains the translations used in Cinnamon

## <a href="https://github.com/linuxmint/cjs">Cjs</a>
Cjs, fork of Gjs, is the "interpreter" of Cinnamon's javascript code. It is not an actual interpreter - the interpretation is done by SpiderMonkey. Instead, the role of Cjs is to provide bindings to GNOME libraries through GObject Introspection.

## Linux Mint

Other Linux Mint projects include

## <a href="https://github.com/linuxmint/blueberry">Blueberry</a>
Blueberry is a bluetooth configuration tool that replaces the old Cinnamon Bluetooth. It depends on GNOME Bluetooth.

## <a href="https://github.com/linuxmint/mdm">MDM</a>
MDM, forked from GDM, is a display manager, not necessarily tied to Cinnamon. MDM officially stands for MDM display manager.

## <a href="https://github.com/linuxmint/nemo">Nemo</a>
Nemo, forked from Nautilus, is the file manager of Cinnamon.

## <a href="https://github.com/linuxmint/muffin">Muffin</a>
Muffin, forked from Mutter, which is in turn forked from Metacity, is the window manager of Cinnamon. Cinnamon is implemented as a plugin of Muffin.

### <a href="https://github.com/linuxmint/mintupload">Mint Upload</a>
### <a href="https://github.com/linuxmint/mintinstall">Mint Install</a>
### <a href="https://github.com/linuxmint/mintnanny">Mint Nanny</a>
### <a href="https://github.com/linuxmint/mintbackup">Mint Backup</a>
### <a href="https://github.com/linuxmint/mint-x-icons">Mint X Icons</a>
### <a href="https://github.com/linuxmint/mint-themes">Mint Themes</a>
### <a href="https://github.com/linuxmint/mint-themes-gtk3">Mint Themes Gtk3</a>
### <a href="https://github.com/linuxmint/mintupdate">Mint Update</a>
### <a href="https://github.com/linuxmint/mintstick">Mint Stick</a>
### <a href="https://github.com/linuxmint/live-installer">Live Installer</a>
### Mint Translations

Package with localization files for Linux Mint applications and services

GitHub: [/linuxmint/mint-translations](https://github.com/linuxmint/mint-translations)

### Mint Sources

Configure the sources for installable software and updates, manage PPAs and authentication keys

GitHub: [/linuxmint/mintsources](https://github.com/linuxmint/mintsources)

### Mint Menu

One of the most advanced menus under linux. Mint Menu supports filtering, favorites, easy uninstallation, autosession, and many other features.

GitHub: [/linuxmint/mintmenu](https://github.com/linuxmint/mintmenu)

### Mint Welcome

Linux Mint - welcome dialog. Shows important information about the release/edition of Linux Mint.

[Read more...](/projects/mintwelcome.html)

GitHub: [/linuxmint/mintwelcome](https://github.com/linuxmint/mintwelcome)

### Mint Desktop

GitHub: [/linuxmint/mintdesktop](https://github.com/linuxmint/mintdesktop)

### <a href="https://github.com/linuxmint/mintdrivers">Mint Drivers</a>
### <a href="https://github.com/linuxmint/mintlocale">Mint Locale</a>