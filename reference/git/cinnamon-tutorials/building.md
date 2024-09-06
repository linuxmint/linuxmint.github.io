
[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials)

# Part II. Building Cinnamon

## Debian-based systems

### Add APT sources repositories

Open `/etc/apt/sources.list`. For each deb line, add the same line with deb replaced with deb-src. For instance, here's how it should look like in Linux Mint 22:

```bash
deb http://packages.linuxmint.com wilma main upstream import
deb-src http://packages.linuxmint.com wilma main upstream import

deb http://archive.ubuntu.com/ubuntu/ noble main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ noble main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ noble-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ noble-updates main restricted universe multiverse

deb http://extras.ubuntu.com/ubuntu noble main
deb-src http://extras.ubuntu.com/ubuntu noble main
```

### Get build dependencies
Then we install the packages needed to compile the Cinnamon stack. Run the following in a terminal (as root):
```bash
apt-get update
apt-get install dpkg-dev
sudo apt-get build-dep cinnamon cinnamon-control-center cinnamon-desktop cinnamon-menus cinnamon-screensaver cinnamon-session cinnamon-settings-daemon cinnamon-translations cjs muffin nemo
```

Now get the latest git code for everything. Run (not as root):

```bash
git clone git://github.com/linuxmint/cinnamon.git
git clone git://github.com/linuxmint/cinnamon-control-center.git
git clone git://github.com/linuxmint/cinnamon-desktop.git
git clone git://github.com/linuxmint/cinnamon-menus.git
git clone git://github.com/linuxmint/cinnamon-screensaver.git
git clone git://github.com/linuxmint/cinnamon-session.git
git clone git://github.com/linuxmint/cinnamon-settings-daemon.git
git clone git://github.com/linuxmint/cinnamon-translations.git
git clone git://github.com/linuxmint/cjs.git
git clone git://github.com/linuxmint/muffin.git
git clone git://github.com/linuxmint/nemo.git
```

### Compile and install the stack

Build and install in the following order. Some packages are required to be <span class="emphasis"><em>installed</em></span> prior to building later packages. See below for how to build and install.

```
cinnamon-translations
cinnamon-desktop
cinnamon-menus
**INSTALL PRECEDING PACKAGES**
cinnamon-session
cinnamon-settings-daemon
cinnamon-screensaver
cjs
**INSTALL PRECEDING PACKAGES**
cinnamon-control-center
muffin
**INSTALL PRECEDING PACKAGES**
cinnamon
nemo
**INSTALL PRECEDING PACKAGES**
```

To build a package:

```bash
cd *package-name*
dpkg-buildpackage
cd ..
```

To install a package:

```bash
sudo dpkg -i *packages produced*
```

### Optional: Compiling from stable branch

The instructions above compile the Cinnamon stack from their "master" branch, which isn't always stable. To compile from the stable branch, instead of the usual building procedure, you need to do the following (for all packages):

```bash
cd *package-name*
git checkout stable
dpkg-buildpackage
cd ..
```

## Other systems

### Get build dependencies
Install the following dependencies using your system's package manager. For build-time dependencies, you may need the development headers (usually found in packages suffixed `-dev` or `-devel`). If a dependency is not available for your system, you can build and install it.
* Build-time dependencies:
	* C/C++ compiler
	* `at-spi2-core`, or `at-spi2-atk` and `atk >= 2.5.3`
	* `cairo >= 1.10.0`
	* `dbus`
	* `fontconfig`
	* `fribidi >= 1.0.0`
	* `gdk-pixbuf >= 2.22.0`
	* `gettext`
	* `glib >= 2.66.0`
	* `gobject-introspection >= 1.66.0`
	* `graphene >= 1.9.3`
	* `gtk3 >= 3.19.8`
	* `intltool`
	* `iso-codes`
	* `json-glib >= 1.6`
	* `libcanberra >= 0.26`
	* `libffi`
	* `libgnomekbd >= 3.6.0`
	* `libgsf`
	* `libgudev >= 232`
	* `libice`
	* `libnotify >= 0.7.3`
	* `libsm`
	* `libx11`
	* `libxau`
	* `libxcb`
	* `libxcomposite >= 0.4`
	* `libxcursor`
	* `libxdamage`
	* `libxext >= 1.1`
	* `libxfixes >= 3`
	* `libxi >= 1.7.4`
	* `libxinerama`
	* `libxkbcommon >= 0.4.3`
	* `libxkbfile`
	* `libxklavier >= 5.1`
	* `libxml2`
	* `libxrandr >= 1.5.0`
	* `libxrender`
	* `libxtst`
	* `linux-pam` or `openpam`
	* `make`
	* `mesa` or `libglvnd`
	* `meson >= 0.56.0`
	* `pango >= 1.40.0`
	* `pkg-config` or `pkgconf`
	* `polkit >= 0.103`
	* `pulseaudio >= 12.99.3`
	* `spidermonkey = 115`
	* `upower >= 0.99.8`
	* `xapp >= 2.6.0`
	* `xdotool`
	* `xkeyboard-config`
	* `xorgproto`
* Optional build-time dependencies:
	* logind implementation (`systemd` or `elogind`)
	* udev implementation `>= 228` (`systemd`, `eudev`, `libudev-devd`, or `libudev-zero`)
	* `colord >= 0.1.27`
	* `cups >= 1.4`
	* `egl-wayland`
	* `exempi >= 2.2.0`
	* `gstreamer`
	* `gtk-doc`
	* `libdrm`
	* `libexif >= 0.6.20`
	* `libinput >= 1.7`
	* `librsvg >= 2.36.2`
	* `libwacom >= 0.13`
	* `libxcvt`
	* `libxtrans`
	* `little-cms >= 2.0`
	* `modemmanager >= 0.7`
	* `networkmanager >= 1.2.0`
	* `nss >= 3.11.2`
	* `pipewire >= 0.3.0`
	* `selinux >= 2.0`
	* `startup-notification >= 0.7`
	* `sysprof3` and/or `sysprof4`
	* `tinysparql` or `tracker`
	* `wayland >= 1.13.0`
	* `wayland-protocols >= 1.19`
	* `xwayland`
* Run-time dependencies (these can be installed before or after compiling Cinnamon):
	* NTPD implementation, `systemd`, or `openrc-settingsd`
	* X11 server
	* `accountsservice`
	* `adwaita-icon-theme`
	* `bash`
	* `caribou`
	* `evolution-data-server`
	* `gnome-icon-theme` or `adwaita-icon-theme-legacy`
	* `gsound`
	* `keybinder3`
	* `libical`
	* `libtimezonemap`
	* `touchegg`

Python has to be installed, along with some Python modules. Install the following modules by using your system's package manager, from PyPI by using `pip`, or by building and installing them:
* `dbus-python`
* `pexpect`
* `pillow`
* `psutil`
* `py-setproctitle` (`setproctitle` on PyPI)
* `pycairo`
* `pygobject`
* `python-pam`
* `python3-xapp` (not on PyPI)
* `pytz`
* `requests`
* `tinycss2`
* `xlrd`

Now get the latest git code for everything. Run (not as root):

```bash
git clone git://github.com/linuxmint/cinnamon.git
git clone git://github.com/linuxmint/cinnamon-control-center.git
git clone git://github.com/linuxmint/cinnamon-desktop.git
git clone git://github.com/linuxmint/cinnamon-menus.git
git clone git://github.com/linuxmint/cinnamon-screensaver.git
git clone git://github.com/linuxmint/cinnamon-session.git
git clone git://github.com/linuxmint/cinnamon-settings-daemon.git
git clone git://github.com/linuxmint/cinnamon-translations.git
git clone git://github.com/linuxmint/cjs.git
git clone git://github.com/linuxmint/muffin.git
git clone git://github.com/linuxmint/nemo.git
```

### Compile and install the stack

Build and install in the following order. See below for how to build and install.

```
cinnamon-translations
cinnamon-desktop
cinnamon-menus
cinnamon-session
cinnamon-settings-daemon
cinnamon-screensaver
cjs
cinnamon-control-center
muffin
cinnamon
nemo
```

To build and install a package:

```bash
cd *package-name*
meson setup builddir
meson compile -C builddir
meson install -C builddir
cd ..
```

The procedure for building and installing `cinnamon-translations` is different:

```bash
cd cinnamon-translations
make
sudo cp -r usr /
cd ..
```

### Optional: Compiling from stable branch

The instructions above compile the Cinnamon stack from their "master" branch, which isn't always stable. To compile from the stable branch, instead of the usual building procedure, you need to do the following (for all packages except `cinnamon-translations`):

```bash
cd *package-name*
git checkout stable
meson setup builddir
meson compile -C builddir
meson install -C builddir
cd ..
```

For `cinnamon-translations`:

```bash
cd cinnamon-translations
git checkout stable
make
sudo cp -r usr /
cd ..
```