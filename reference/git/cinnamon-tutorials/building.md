[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials)

# Part II. Building Cinnamon

## Debian-based systems

### Add APT sources repositories

Open `/etc/apt/sources.list`. For each deb line, add the same line with deb replaced with deb-src. For instance, here's how it should look like in Linux Mint 13:

```bash
deb http://packages.linuxmint.com maya main upstream import
deb-src http://packages.linuxmint.com maya main upstream import

deb http://archive.ubuntu.com/ubuntu/ precise main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ precise-updates main restricted universe multiverse

deb http://extras.ubuntu.com/ubuntu precise main
deb-src http://extras.ubuntu.com/ubuntu precise main
```

### Get build dependencies
Then we install the packages needed to compile the cinnamon stack. Run the following in a terminal (as root):
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

The instructions above compile the Cinnamon stack from their "master" branch, which isn't always stable. To compile from the stable branch, instead of the usual building procedure, you need to to the following (for all packages):

```bash
cd *package-name*
git checkout stable
dpkg-buildpackage
cd ..
```
