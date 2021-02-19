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
