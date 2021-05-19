pkg-tools - modern package manager.

If you're looking for news about pkg-tools, devblog is located at bottom of the page

Commands:
pkg-add (/usr/local/bin/pkg-add) - installs package
pkg-create (/usr/local/bin/pkg-create) - creates package from current directory (to use it create in CWD dirs: usr,etc,opt etc), puts the pkg file to ..
pkg-info (/usr/local/bin/pkg-info) - displays information about package
pkg-delete (/usr/local/bin/pkg-delete) - removes a package

All commands can be used with --help flag

This software is wroten is SH(ell) and it's distributed under 4-clause BSD license ( https://raw.githubusercontent.com/glowiak/pkg-tools/master/BSD )


DOWNLOAD:
Arch package: https://github.com/glowiak/pkg-tools/releases/download/1.0/pkg-tools-1.5-1-any.pkg.tar
Universal TXZ: https://github.com/glowiak/pkg-tools/releases/download/1.0/pkg-tools.1.5.txz

Installation:
ArchPKG:
root# pacman -U pkg-tools*.pkg.tar.*
Universal TXZ (preffered way):
root# tar xJvf pkg-tools*.txz -C / | tee > /etc/pkg-tools-MTREE.mtree

Removing pkg-tools:
ArchPKG:
root# pacman -R pkg-tools
Universal TXZ:
root# CWD=$(pwd)
root# cd /
root# rm $(cat /etc/pkg-tools-MTREE.mtree)
root# cd $CWD

Dependiences:
-sh
-xz-utils
-tar

Usage:
pkg-create:
user$ pkg-create <name of the package> <version> <creator> <architecture> <postinst script>
pkg-add:
root# pkg-add /path/to/package.txz
pkg-delete:
root# pkg-delete <name of the package>
pkg-info:
user$ pkg-info <name of the package>

Postinst script is the script that is executed after installation of the package.
Example of postinst.sh is located at /etc/pkg-tools/examples/postinst.sh-example


What is .MTREE?
.MTREE is filesystem image that is used for pkg-delete to know what files should be deleted

What if my program don't need postinst?
If you don't want to execute any postinst scripts, just use '/etc/pkg-tools/examples/postinst.sh-example' as postinst path when building package.

How I can make my package?
That's easy. Just follow the steps (and may watch example package).
Step 1: create directory with hierarchy of dirs (/usr, /usr/local etc).
Step 2: run 'pkg-create <name of package> <version> <creator> <architecture> <postinst, you can use example for no script>' as normal user in build dir.
Done.

As example you can test Minecraft Launcher package, that was built
with pkg-create: https://github.com/glowiak/pkg-tools/releases/download/repodb/minecraft-launcher-1.0_x86_64.txz

Distro compatibility
#########################################
Recommended distros to use pkg-tools are: Archlinux (86_64) and Slackware64-CURRENT
but pkg-tools can be used on any UNIX-like os with sh, tar and xz.
repodb ( http://github.com/glowiak/pkg-tools/releases/tag/repodb ) is repository of packages built for Arch (x86_64) and Slackware64-CURRENT



Why txz and not tgz, tbz or tlz?
idk I just like txz


Comparison of my package managers:
#######################################
Package format:
sysconf   = cpio+xz+tgz
gpk       = ar+tgz
pkg-tools = txz+txz
#######################################
Dependiences support:
sysconf   = no
gpk       = no
pkg-tools = yes
#######################################
Package creation (difficulty):
sysconf   = very hard, need to manually setup files, pack and upload it
gpk       = hard, need to manually setup files, need to download build script, which may print error (lololol)
pkg-tools = very easy, no need to manually setup any config files or download build scripts. Just prepare file tree and run pkg-create with arguments
#######################################
Package distribution
sysconf   = packages can be installed only from online repositories (no chance to install from local fs)
gpk       = supports only installing from local fs, but igpk package provides slackpkg-like interface to install from repos
pkg-tools = supports only installing from local fs, but I'm working on apt-like tool for managing repos

Sorry for not making 32bit packages, but I don't have 32bit pc



pkg-get
##################################
So, I made apt-like tool to managing pkg-tools packages.
Download:
for x64: https://github.com/glowiak/pkg-tools/releases/download/repodb/pkg-get-1.3_x86_64.txz
for i386: https://github.com/glowiak/pkg-tools/releases/download/repodb/pkg-get-1.3_i386.txz

btw
####################################
pkg-get is too much apt-like, btw it the NEW  orginal, customizable tool for managing pkg-tools packages.
How to get it?
Install 'btw' package with pkg-get or download this:
https://github.com/glowiak/pkg-tools/releases/download/repodb/btw-1.9_noarch.txz
file, and install it with pkg-add

Q: How I can configure btw?
A: Just edit /etc/btw.conf

btw 1.1 UPDATE RELEASED!!!!!!!
News: now btw uses wget to fetch files instead of curl; added groups

groups
#####################################
groups are like dependiences, you can use it, in btw only, but I'm planning port in into pkg-get.
groups usage (btw):
root# btw gdd <group name>

Q: Why latest btw is 1.2, but not 1.1?
A: btw 1.1 exists in repositories, but this version is broken and it isn't recommended to use it.


GOOD NEWS! 1.1 UPDATE RELEASED!!!!
What new?
-added /etc/pkg-tools/pkg-tools.conf, the config file of the pm
-fixed bug, that will broke pm, when filename is empty

######### CONFIGURING pkg-tools.conf ###########
pkg-tools.conf, that is located in /etc/pkg-tools/ is the config file

WHAT TO EDIT?
-You can change the 'PKGROOT' variable, to ser where the packages are installed, for example to '/mnt', works only with absolute paths
-Finnaly, you can change compression type!!! default is 'J' (xz), but you can change it for example to 'j' (bzip2) or 'Z' (ncompress). you can do it via changing 'PKGTYPE' variable
-You also can change pkg extension!!! default is 'txz', but you can change is as you like! for example to 'pkg' to get 'package.pkg'! if you want to troll windows users change extension to msi. you can to it via changing 'PKGEXT' variable


i'm moving pkg-tools to sourceforge


# btw 1.3 update
-added 'mpm' action. mpm stands for Multipackaging Mode, and allows you to install many packages with one command.

How to use btw-mp?
1. Type 'btw mpm' in shell;
2. Type packages that you want to install or 'exit' to quit btw


# pkg-tools 1.2 released!
What new in 1.2?
-ported pkg-tools to bourne shell (orginal /bin/sh, still used in FreeBSD and NetBSD)

pkg-get is now unsupported, please use btw instead

# btw 1.4 update
-added dependiences support. To have compatibility with older packages, 'add' option will install packages without
resolving dependiences, while 'ndd' option will install packages, but install their dependiences first.

# Adding dependiences to packages
[NOTE: btw doesn't support installing dependiences of dependiences. For example instead of 'apache-apr-util' use 'apache-apr apache-apr-util'.]
//First create pkg-create0 directory:
mkdir /tmp/pkg-create0
echo "<dependency 1> <dependency 2> <dependency 3>" > /tmp/pkg-create0/.DEPS_PKG //replace '<dependency X>' with depenciendes of your package
//Now normally create the package, and upload it to a repo. This package supports dependiences. I'm working on installing local files with dependiences.
pkg-create <name> <version> <creator> <architecture> <postinst script path>


Q: ok, but gpk also supports dependiences, so pkg-tools is using distro's package manager? if yes, why there's only ArchPKG and universal pkg?
A: gpk was pretty bad. pkg-tools is installing dependiences from its own repodb, using 'dps' btw function

Q: sooo, what is 'dps' btw function, how it works and how to use it?
A: dps means 'dependiences', and is used for installing dependiences. dps is using DEPS variable defined before starting btw, to know which packages should be installed. in one word, dps is like multipackaging mode, but can be used in a scripts, for example to install apache-apr{,-util} use 'DEPS="apache-apr apache-apr-util" btw dps'

# btw 1.5 update
-added support for installing local packages with dependiences. use 'lnd' btw option to do it. Usage: 'btw lnd package-version_arch.txz'.

I'm planning to compile X packages and pack it into pkg-tools packages.

Today or tomorrow I'll add search feature to btw.

I indexed repodb's packages, so now time to work on search feature :)

Ok, I finished the search feature, that's named 'srh'. I'll just fix some bugs and upload btw 1.6 :D

# btw 1.6 update
-added 'srh' function that's used to search for packages.
-added 'upr' function that's used to update local software index stored on your pc, needed for 'srh' function to work
-fixed many bugs
-now if you want to remove/get info about package, but this package doesn't exists, btw tells you that
-now if you want to install local package with dependiences, but file doesn't exists, btw tells you that


# Indexing packages
repository's package index is just a text file named 'repo.db' containing 'pkg_name     description'. Just place it in the same place
that repo's packages are stored in

# btw 1.7 update
1.7 is minor bugfix update for btw

# pkg-tools 1.3 update
1.3 is minor bugfix update for pkg-tools

# pkg-tools 1.4 update
1.4 is major update that adds 'noarch' package architecture, that allows you to package,
for example bash/kornshell/sh scripts as packages, for any architecture or steam

btw 1.8 will be released today :D
i'm working on graphical tool 'gbtw' or something :)

# btw 1.8 update
1.8 is major update for btw
What new?
-now packages are stored in /etc/btw/packages instead of tmp dir that is removed after installing
-added 'ccc' option to clean packages stored on disk
-now btw uses 'noarch' architecture, so you can't use it with pkg-tools 1.3 or older
-fixed many bugs


# Xorg
xorg is a big problem, it's large program with a lot of dependiences,
i don't have enough time to build all xorg-related packages, so i ported xorg from Arch's repos
as 'xorg-dev-bin' package. this package also contains open-source GPU drivers.

i'm planning to make pkg-tools-based distro. now i don't have enough time to compile
all those software on my pc, so this base stuff like gcc, xorg and others will be grabed from Arch repos
as <package name>-dev-bin, version of it will be 'arch-dev'.

# Base Utilities
I ported from Arch 2 more packages: 'base' as 'base-dev-bin' and 'base-devel' as 'basedevel-dev-bin'. Pacman is removed from those packages.
I'll port kernel 4.19 and GRUB2

# pkg-tools 1.5 update
1.5 is minor bugfix update for pkg-tools, that makes possible installing packages in chroot,
it will be useful, when pkg-tools-based distro will be ready.

# pkg-tools-based distro is NEAR
I added some more dev-bin packages for daily use:
-kernel54-dev-bin: Linux 5.4 Kernel and Headers
-kernelfw-dev-bin: Linux Kernel Firmware
-xorgapps-dev-bin: Xorg apps like xterm or twm
-grub2-dev-bin: GRUB2 Bootloader
Any suggestions of name of new distro?

# Installing Xorg with pkg-tools
Install 'xorg-dev-bin' and 'xorgapps-dev-bin' packages with btw. Optionally you can download and install it manually,
but this isn't recommended.


# pkg-tools-based OS development status
I was testing OS v0.1 codename "Tsetse", all is ok, but it has problems with initramfs, so no chance to booting from GRUB at the moment :/

Sorry, no updates today and tomorrow, because I'm working on desktopinstaller ( http://sourceforge.net/p/desktopinstaller/code/ci/master/tree )

pkg-tools yet don't have full bourne shell support, I'll add it today or tomorrow, but for now, you can symlink kornshell (ksh93) to /bin/sh

I decied to remove Arch pkg-tools package. Instead of it you will can download .tar.xz file, and use make to install/remove it.
I'll stop developing codename tsetse for a while, because I can't find any way to build initramfs,
instead of it I will release more pkg-tools and btw updates, for better bourne shell compatibility, and bug fixes
Codename tsetse will be back, but not now, sorry.

Wait, I have and idea: turn codename tsetse into bedrock-like metadistro installable on existing distro.
ok, I'll do it

Q: Will it delete my files?
A: No, It will delete only your-os-related things like pacman, zypper or makepkg

Q: When pkg-tools will have full bourne shell support
A: I don't know when, this may take a while

Q: So, which shell should I link to /bin/sh?
A: The best option is to link kornshell to /bin/sh, kornshell is faster than bash, I tested on it pkg-tools, but it also should work with bash, BUT NOT WORK WITH DASH, THAT IS DEFAULT LINKED IN UBUNTU, so if you're using ubuntu, install kornshell and link ksh93 (ksh is always symlink to ksh93, you cannot make link to link) to /bin/sh

I started working on tsetse, work in progress at tsetse-init - init management tool, that uses core-system's init
to manage services.

Q: So, tsetse can be installed on a systemd-distro only?
A: No, tsetse-init will detect your core-init. You'll can use one the same command on other init systems.

Two new packages added:
-tsetse-system, that contains Tsetse system
-tsetse-init, that contains Tsetse init and scripts

Tsetse will support following inits:
-systemd
-runit
-OpenRC
-SysVinit
I maybe add s6 support later

Q: What is "tsetse scripts"?
A: tsetse-init is modular init tool, everyone can add own modules to it. Modules are just ksh scripts, that
are placed in /usr/local/etc/tsetse/init/scripts directory

I'm making new source-based package manager - cps (Concurrent Port System) :)

I'm back. Time for updates.

btw 1.9 update RELEASED.
1.9 is major update to btw, that adds 'upg' function that upgrades a package.


Q: What is this 'upg' function?
A: it's like 'upgrade' slackpkg command. Upgrades a package, but cannot install a package that isn't installed

Today, will be another pkg-tools update

I'll stop developing tsetse for a while

### Self-updating btw
will be in next update


I'l compiling X11 packages :))))))))
