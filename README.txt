pkg-tools - modern package manager.

Commands:
pkg-add (/usr/local/bin/pkg-add) - installs package
pkg-create (/usr/local/bin/pkg-create) - creates package from current directory (to use it create in CWD dirs: usr,etc,opt etc), puts the pkg file to ..
pkg-info (/usr/local/bin/pkg-info) - displays information about package
pkg-delete (/usr/local/bin/pkg-delete) - removes a package

All commands can be used with --help flag

DOWNLOAD:
Arch package: https://github.com/glowiak/pkg-tools/releases/download/1.0/pkg-tools-1.0-1-any.pkg.tar.zst
Universal TXZ: https://github.com/glowiak/pkg-tools/releases/download/1.0/pkg-tools.1.0.txz

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
pkg-tools = no
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
