---
title: archlinux
comments: true
---

## Programs I use

programs i use:

* [qxmledit](qxmledit.md)
* [picom](picom.md)
* [wpa_supplicant](wpa_supplicant.md)
* [sxiv](sxiv.md)
* [mpv](mpv.md)
* [xpdf](xpdfrc.md)
* [xmodmap](xmodmap.md)
* [xbindkeys](xbindkeys.md).

## Tips

## makepkg and PKGBUILD
See [makepkg](makepkg.md), [PKGBUILD](pkgbuild.md).

## Set default apps
```
xdg-mime default zathura.desktop application/pdf
xdg-mime default sxiv.desktop image/png
```
### Skip annoying gpg checks
```
makepkg -sri --skippgpcheck
```

## PKGBUILD if your workplace has crappy internet
I) Replace git source with zip source
```
# source=("${pkgname}"::"git+https://github.com/neovim/neovim")
source=("${pkgname}.zip"::"https://github.com/neovim/neovim/archive/refs/heads/master.zip")
```

II) Configure makepkg to use aria2c

source: [https://wiki.archlinux.org/title/Aria2#Using_aria2_with_makepkg](https://wiki.archlinux.org/title/Aria2#Using_aria2_with_makepkg)
```
#########################################################################
# SOURCE ACQUISITION
#########################################################################
#
#-- The download utilities that makepkg should use to acquire sources
#  Format: 'protocol::agent'
# DLAGENTS=('file::/usr/bin/curl -qgC - -o %o %u'
#           'ftp::/usr/bin/curl -qgfC - --ftp-pasv --retry 3 --retry-delay 3 -o %o %u'
#           'http::/usr/bin/curl -qgb "" -fLC - --retry 3 --retry-delay 3 -o %o %u'
#           'https::/usr/bin/curl -qgb "" -fLC - --retry 3 --retry-delay 3 -o %o %u'
#           'rsync::/usr/bin/rsync --no-motd -z %u %o'
#           'scp::/usr/bin/scp -C %u %o')

DLAGENTS=('ftp::/usr/bin/aria2c -UWget -s10 -x10 %u -o %o --follow-metalink=mem'
          'http::/usr/bin/aria2c -UWget -s10 -x10 %u -o %o --follow-metalink=mem'
          'https::/usr/bin/aria2c -UWget -s10 -x10 %u -o %o --follow-metalink=mem'
          'rsync::/usr/bin/rsync --no-motd -z %u %o'
          'scp::/usr/bin/scp -C %u %o')

```

source: [https://github.com/neovim/neovim/blob/master/BUILD.md](https://github.com/neovim/neovim/blob/master/BUILD.md)
```
# Maintainer: VectorXYZ <resonyze@gmail.com>
pkgname=nvim-vector
# pkgver is found in CMakeLists.txt NVIM_VERSION_MAJOR, NVIM_VERSION_MINOR ..
pkgver=0.11.0_dev
pkgrel=1
pkgdesc="Neovim"
arch=(any)
url="https://github.com/neovim/neovim"
license=('MIT')
# depends = 
conflicts=('neovim')
makedepends=('git' 'base-devel' 'cmake' 'unzip' 'ninja' 'curl')
# source=("${pkgname}"::"git+https://github.com/neovim/neovim")
source=("${pkgname}.zip"::"https://github.com/neovim/neovim/archive/refs/heads/master.zip")
sha256sums=('SKIP')

build() {
  cd "$srcdir/neovim-master"
  make CMAKE_BUILD_TYPE=Release
}

package() {
  cd "$srcdir/neovim-master"
  make CMAKE_INSTALL_PREFIX=$pkgdir/usr install
}
```

## inputrc

`~/.inputrc`

```
set completion-ignore-case On
# set show-all-if-ambiguous on
set editing-mode vi
# #"\C-l": nop
"\C-l": clear-screen
```

## Alt-O Scratchpad
This is probably one of the first things I reach for once I've booted a fresh arch install and installed X, dwm.
Once you get used to this floating terminal which you can summon and dismiss with a simple binding, you can't go
back 😀.

```bash
ID=`xdotool search --class dwmalto`
if ! [[ -z $ID ]];
then
        if xdotool search --onlyvisible --class dwmalto;
        then
                xdotool windowunmap $ID
        else
                xdotool windowmap $ID
        fi
else
        tabbed -c -n dwmalto st -w
fi
```

## Volume scripts

At present i've created two separate scripts bound to separate bindings, Alt-] and Alt-[.

### Increase volume

```bash
#!/bin/bash

function ProgressBar {
# Process data
    let _progress=(${1}*100/${2}*100)/100
    let _done=(${_progress}*2)/10
    let _left=20-$_done
# Build progressbar string lengths
    _fill=$(printf "%${_done}s")
    _empty=$(printf "%${_left}s")

# 1.2 Build progressbar strings and print the ProgressBar line
# 1.2.1 Output example:
# 1.2.1.1 Progress : [########################################] 100%
printf "[${_fill// /#}${_empty// /-}] ${_progress}%%"

}


pactl -- set-sink-volume 0 +5%

VOL=$(pamixer --get-volume)

notify-send -t 1000 -h string:x-canonical-private-synchronous:volume-notification "VOL: $(ProgressBar $VOL 100)"
```

### Decrease volume

```bash
#!/bin/bash

function ProgressBar {
# Process data
    let _progress=(${1}*100/${2}*100)/100
    let _done=(${_progress}*2)/10
    let _left=20-$_done
# Build progressbar string lengths
    _fill=$(printf "%${_done}s")
    _empty=$(printf "%${_left}s")

# 1.2 Build progressbar strings and print the ProgressBar line
# 1.2.1 Output example:
# 1.2.1.1 Progress : [########################################] 100%
printf "[${_fill// /#}${_empty// /-}] ${_progress}%%"

}

pactl -- set-sink-volume 0 -5%

VOL=$(pamixer --get-volume)

notify-send -t 1000 -h string:x-canonical-private-synchronous:volume-notification "VOL: $(ProgressBar $VOL 100)"
```

## Steps to add a custom repository in archlinux

### Create the repository root in /home owned by the current user

```
# install -d /home/custom-testing-pkgs -o $USER
```

### Create a signed repository database

- Note that the name I specified in pacman.conf was `custom-testing`.
- The database file pacman expects in the root directory is `custom-testing.db`.
- When adding the database through `repo-add -s`, I specifiy the same `custom-testing.db`
  but in compressed form `custom-testing.db.tar.gz`.

```
$ repo-add -s /home/custom-testing-pkgs/custom-testing.db.tar.gz
```

### Edit /etc/pacman.conf to add a new entry for our repository

`# sudo vim /etc/pacman.conf`

```
[custom-testing]
SigLevel = Optional TrustAll
Server = file:///home/custom-testing-pkgs
```

