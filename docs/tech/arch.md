---
title: archlinux
comments: true
---
## /etc/wpa_supplicant/wpa_supplicant.conf
```
ctrl_interface=/run/wpa_supplicant
# update_config=1 # Let wpa_cli modify this file
update_config=0 # Don't let wpa_cli modify this file

network={
	ssid="Vectorspace"
	psk="thePassword"
	mesh_fwding=1
	priority=3
}

network={
	ssid="NokiaG21"
	psk="thePassword"
	mesh_fwding=1
	priority=4
}

network={
	ssid="home wifi"
	psk="thePassword"
	mesh_fwding=1
	priority=3
}

network={
	ssid="Sreema Filaments - Jio"
	psk="thePassword"
	mesh_fwding=1
	priority=3
}
```

## sxiv
```
#!/usr/bin/bash

shopt -s extglob
shopt -s globstar
# Example for $XDG_CONFIG_HOME/sxiv/exec/key-handler
# Called by sxiv(1) after the external prefix key (C-x by default) is pressed.
# The next key combo is passed as its first argument. Passed via stdin are the
# images to act upon, one path per line: all marked images, if in thumbnail
# mode and at least one image has been marked, otherwise the current image.
# sxiv(1) blocks until this script terminates. It then checks which images
# have been modified and reloads them.

# The key combo argument has the following form: "[C-][M-][S-]KEY",
# where C/M/S indicate Ctrl/Meta(Alt)/Shift modifier states and KEY is the X
# keysym as listed in /usr/include/X11/keysymdef.h without the "XK_" prefix.

rotate() {
	degree="$1"
	tr '\n' '\0' | xargs -0 realpath | sort | uniq | while read file; do
		case "$(file -b -i "$file")" in
		image/jpeg*) jpegtran -rotate "$degree" -copy all -outfile "$file" "$file" ;;
		*)           mogrify  -rotate "$degree" "$file" ;;
		esac
	done
}

case "$1" in
"C-x")      xclip -in -filter | tr '\n' ' ' | xclip -in -selection clipboard ;;
"C-c")      while read file; do xclip -selection clipboard -target image/png "$file"; done ;;
"C-e")      while read file; do urxvt -bg "#444" -fg "#eee" -sl 0 -title "$file" -e sh -c "exiv2 pr -q -pa '$file' | less" & done ;;
"C-g")      tr '\n' '\0' | xargs -0 gimp & ;;
"C-r")      while read file; do rawtherapee "$file" & done ;;
"C-g")      while read file; do gimp "$file" & done ;;
"C-d")      while read file; do mv -vf "$file" /tmp/ ; FPATH="${file%/*}"; FNAME="${file##*/}"; if [[ -e "${FPATH}/.${FNAME}.txt" ]]; then mv -vf "${FPATH}/.${FNAME}.txt" /tmp/ ; fi done ;;
"C-m")      while read file; do cp -v "$file" /home/vector/Pictures/Marked/ ; done ;;
"C-m")        while read file; do mv -vf -- "$file" /home/vector/Pictures/Marked/Me/ ; done ;;
"C-t")        while read file; do FILEPATH="$(realpath "$file")"; mv -vf -- "$file" "${FILEPATH%/*}/trash/" ; done ;;
"C-p")      while read file; do realpath "$file" | xclip -i -selection clipboard & done ;;
"C-o")      while read file; do FNAME="${file%%-*}"; FNAME="${FNAME}.pdf"; PAGEN="${file##*-}"; PAGEN="${PAGEN%.*}"; PAGEN="${PAGEN##+(0)}"; zathura -P "${PAGEN}" "$FNAME" & done ;;
"comma")  rotate 270 ;;
"period") rotate  90 ;;
"slash")  rotate 180 ;;
esac
```

## mpv
```
ctrl+r cycle_values video-rotate "90" "180" "270" "0"

# mpv keybindings
#
# Location of user-defined bindings: ~/.config/mpv/input.conf
#
# Lines starting with # are comments. Use SHARP to assign the # key.
# Copy this file and uncomment and edit the bindings you want to change.
#
# List of commands and further details: DOCS/man/input.rst
# List of special keys: --input-keylist
# Keybindings testing mode: mpv --input-test --force-window --idle
#
# Use 'ignore' to unbind a key fully (e.g. 'ctrl+a ignore').
#
# Strings need to be quoted and escaped:
#   KEY show-text "This is a single backslash: \\ and a quote: \" !"
#
# You can use modifier-key combinations like Shift+Left or Ctrl+Alt+x with
# the modifiers Shift, Ctrl, Alt and Meta (may not work on the terminal).
#
# The default keybindings are hardcoded into the mpv binary.
# You can disable them completely with: --no-input-default-bindings

# Developer note:
# On compilation, this file is baked into the mpv binary, and all lines are
# uncommented (unless '#' is followed by a space) - thus this file defines the
# default key bindings.

# If this is enabled, treat all the following bindings as default.
#default-bindings start

#MBTN_LEFT     ignore              # don't do anything
#MBTN_LEFT_DBL cycle fullscreen    # toggle fullscreen on/off
#MBTN_RIGHT    cycle pause         # toggle pause on/off
#MBTN_BACK     playlist-prev
#MBTN_FORWARD  playlist-next

# Mouse wheels, touchpad or other input devices that have axes
# if the input devices supports precise scrolling it will also scale the
# numeric value accordingly
#WHEEL_UP      seek 10
#WHEEL_DOWN    seek -10
#WHEEL_LEFT    add volume -2
#WHEEL_RIGHT   add volume 2

## Seek units are in seconds, but note that these are limited by keyframes
#RIGHT seek  5
#LEFT  seek -5
#UP    seek  60
#DOWN  seek -60
# Do smaller, always exact (non-keyframe-limited), seeks with shift.
# Don't show them on the OSD (no-osd).
Shift+RIGHT no-osd seek  1 exact
Shift+LEFT  no-osd seek -1 exact
#Shift+UP    no-osd seek  5 exact
#Shift+DOWN  no-osd seek -5 exact
# Skip to previous/next subtitle (subject to some restrictions; see manpage)
#Ctrl+LEFT   no-osd sub-seek -1
#Ctrl+RIGHT  no-osd sub-seek  1
# Adjust timing to previous/next subtitle
#Ctrl+Shift+LEFT sub-step -1
#Ctrl+Shift+RIGHT sub-step 1
# Move video rectangle
#Alt+left  add video-pan-x  0.1
#Alt+right add video-pan-x -0.1
#Alt+up    add video-pan-y  0.1
#Alt+down  add video-pan-y -0.1
# Zoom/unzoom video
#Alt++     add video-zoom   0.1
#Alt+-     add video-zoom  -0.1
# Reset video zoom/pan settings
#Alt+BS set video-zoom 0 ; set video-pan-x 0 ; set video-pan-y 0
#PGUP add chapter 1                     # skip to next chapter
#PGDWN add chapter -1                   # skip to previous chapter
#Shift+PGUP seek 600
#Shift+PGDWN seek -600
#[ multiply speed 1/1.1                 # scale playback speed
#] multiply speed 1.1
#{ multiply speed 0.5
#} multiply speed 2.0
#BS set speed 1.0                       # reset speed to normal
#Shift+BS revert-seek                   # undo previous (or marked) seek
#Shift+Ctrl+BS revert-seek mark         # mark position for revert-seek
#q quit
#Q quit-watch-later
#q {encode} quit 4
#ESC set fullscreen no
#ESC {encode} quit 4
#p cycle pause                          # toggle pause/playback mode
#. frame-step                           # advance one frame and pause
#, frame-back-step                      # go back by one frame and pause
SPACE cycle pause
n playlist-next                        # skip to next file
#ENTER playlist-next                    # skip to next file
p playlist-prev                        # skip to previous file
#O no-osd cycle-values osd-level 3 1    # cycle through OSD mode
#o show-progress
#P show-progress
#i script-binding stats/display-stats
#I script-binding stats/display-stats-toggle
#` script-binding console/enable
#z add sub-delay -0.1                   # subtract 100 ms delay from subs
#Z add sub-delay +0.1                   # add
#x add sub-delay +0.1                   # same as previous binding (discouraged)
#ctrl++ add audio-delay 0.100           # this changes audio/video sync
#ctrl+- add audio-delay -0.100
#9 add volume -2
#/ add volume -2
#0 add volume 2
#* add volume 2
= add volume 5
- add volume -5
#m cycle mute
#1 add contrast -1
#2 add contrast 1
#3 add brightness -1
#4 add brightness 1
#5 add gamma -1
#6 add gamma 1
#7 add saturation -1
#8 add saturation 1
#Alt+0 set window-scale 0.5
#Alt+1 set window-scale 1.0
#Alt+2 set window-scale 2.0
# toggle deinterlacer (automatically inserts or removes required filter)
#d cycle deinterlace
#r add sub-pos -1                       # move subtitles up
#R add sub-pos +1                       #                down
#t add sub-pos +1                       # same as previous binding (discouraged)
#v cycle sub-visibility
# stretch SSA/ASS subtitles with anamorphic videos to match historical
#V cycle sub-ass-vsfilter-aspect-compat
# switch between applying no style overrides to SSA/ASS subtitles, and
# overriding them almost completely with the normal subtitle style
#u cycle-values sub-ass-override "force" "no"
#j cycle sub                            # cycle through subtitles
#J cycle sub down                       # ...backwards
#SHARP cycle audio                      # switch audio streams
#_ cycle video
#T cycle ontop                          # toggle video window ontop of other windows
#f cycle fullscreen                     # toggle fullscreen
#s screenshot                           # take a screenshot
#S screenshot video                     # ...without subtitles
#Ctrl+s screenshot window               # ...with subtitles and OSD, and scaled
#Alt+s screenshot each-frame            # automatically screenshot every frame
#w add panscan -0.1                     # zoom out with -panscan 0 -fs
#W add panscan +0.1                     #      in
#e add panscan +0.1                     # same as previous binding (discouraged)
# cycle video aspect ratios; "-1" is the container aspect
#A cycle-values video-aspect-override "16:9" "4:3" "2.35:1" "-1"
#POWER quit
#PLAY cycle pause
#PAUSE cycle pause
#PLAYPAUSE cycle pause
#PLAYONLY set pause no
#PAUSEONLY set pause yes
#STOP quit
#FORWARD seek 60
#REWIND seek -60
#NEXT playlist-next
#PREV playlist-prev
#VOLUME_UP add volume 2
#VOLUME_DOWN add volume -2
#MUTE cycle mute
#CLOSE_WIN quit
#CLOSE_WIN {encode} quit 4
#ctrl+w quit
#E cycle edition                        # next edition
#l ab-loop                              # Set/clear A-B loop points
l cycle-values loop-playlist "inf" "no"    # toggle infinite looping
L cycle-values loop-file "inf" "no"    # toggle infinite looping
#ctrl+c quit 4
#DEL script-binding osc/visibility      # cycle OSC display
#ctrl+h cycle-values hwdec "auto" "no"  # cycle hardware decoding
#F8 show_text ${playlist}               # show playlist
#F9 show_text ${track-list}             # show list of audio/sub streams

#
# Legacy bindings (may or may not be removed in the future)
#
#! add chapter -1                       # skip to previous chapter
#@ add chapter 1                        #         next

#
# Not assigned by default
# (not an exhaustive list of unbound commands)
#

# ? add sub-scale +0.1                  # increase subtitle font size
# ? add sub-scale -0.1                  # decrease subtitle font size
# ? cycle angle                         # switch DVD/Bluray angle
# ? cycle sub-forced-only               # toggle DVD forced subs
# ? cycle program                       # cycle transport stream programs
# ? stop                                # stop playback (quit or enter idle mode)
```


## .xbindkeysrc
```
# For the benefit of emacs users: -*- shell-script -*-
###########################
# xbindkeys configuration #
###########################
#
# Version: 1.8.7
#
# If you edit this file, do not forget to uncomment any lines
# that you change.
# The pound(#) symbol may be used anywhere for comments.
#
# To specify a key, you can use 'xbindkeys --key' or
# 'xbindkeys --multikey' and put one of the two lines in this file.
#
# The format of a command line is:
#    "command to start"
#       associated key
#
#
# A list of keys is in /usr/include/X11/keysym.h and in
# /usr/include/X11/keysymdef.h
# The XK_ is not needed.
#
# List of modifier:
#   Release, Control, Shift, Mod1 (Alt), Mod2 (NumLock),
#   Mod3 (CapsLock), Mod4, Mod5 (Scroll).
#

# The release modifier is not a standard X modifier, but you can
# use it if you want to catch release events instead of press events

# By defaults, xbindkeys does not pay attention with the modifiers
# NumLock, CapsLock and ScrollLock.
# Uncomment the lines above if you want to pay attention to them.

#keystate_numlock = enable
#keystate_capslock = enable
#keystate_scrolllock= enable

# Examples of commands:

#"xdotool mousemove_relative 30 0"
#    m:0x0 + c:114
#    Right

#"xdotool mousemove_relative 0 30"
#    m:0x0 + c:116
#    Down
#
#"xdotool mousemove_relative 0 -30"
#    m:0x0 + c:111
#    Up

#"xdotool mousemove_relative -- -30 0"
#    m:0x0 + c:113
#    Left

"~/bin/l.sh"
    m:0x8 + c:25
    Alt + w

"xbindkeys_show" 
  control+shift + q

"~/bin/scratchpad.sh"
    m:0x8 + c:32
    Alt + o

"~/bin/cmus.sh"
    m:0x40 + c:58
    Mod4 + m

"~/bin/screenshot.sh"
    m:0x0 + c:107
    Print

"~/bin/record-screen.sh"
    m:0x8 + c:27
    Alt + r

"~/bin/annotateShot.sh"
    m:0x0 + c:78
    Scroll_Lock

"~/bin/inc-volume.sh"
    m:0x8 + c:35
    Alt + bracketright

"~/bin/dec-volume.sh"
    m:0x8 + c:34
    Alt + bracketleft

"~/bin/t"
    m:0x8 + c:31
        Alt + i

"cmus-remote -r && ~/bin/cmus-song-info.sh"
    m:0x40 + c:34
    Mod4 + bracketleft

"cmus-remote -n && ~/bin/cmus-song-info.sh"
    m:0x40 + c:35
    Mod4 + bracketright

"cmus-remote -u"
    m:0x40 + c:65
    Mod4 + space

"~/bin/cmus-song-info.sh"
    m:0x40 + c:51
    Mod4 + backslash

"cmus-remote -k +5 && ~/bin/progressbar.sh"
    m:0x40 + c:60
    Mod4 + period

"cmus-remote -k -5 && ~/bin/progressbar.sh"
    m:0x40 + c:59
    Mod4 + comma

"~/bin/cmus.sh"
    m:0x8 + c:58
    Alt + m

"~/bin/shownote.sh"
    m:0x8 + c:57
    Alt + n

#"~/bin/book.sh"
#    m:0x40 + c:44
#    Mod4 + j

"~/bin/showNoteShots.sh"
    m:0x8 + c:24
    Alt + q

#"xfce4-session-logout"
#    m:0x9 + c:26
#        Shift+Alt + e

# set directly keycode (here control + f with my keyboard)
#"xterm"
  c:41 + m:0x4

# specify a mouse button
#"xterm"
#  control + b:2

#"xterm -geom 50x20+20+20"
#   Shift+Mod2+alt + s
#
## set directly keycode (here control+alt+mod2 + f with my keyboard)
#"xterm"
#  alt + c:0x29 + m:4 + mod2
#
## Control+Shift+a  release event starts rxvt
#"rxvt"
#  release+control+shift + a
#
## Control + mouse button 2 release event starts rxvt
#"rxvt"
#  Control + b:2 + Release

##################################
# End of xbindkeys configuration #
##################################
```

## .bashrc
```bash
#  _               _
# | |__   __ _ ___| |__  _ __ ___
# | '_ \ / _` / __| '_ \| '__/ __|
# | |_) | (_| \__ \ | | | | | (__
# |_.__/ \__,_|___/_| |_|_|  \___|

# [[ $- == *i* ]] && source /usr/share/blesh/ble.sh --noattach

# If not running interactively, don't do anything
# [[ $- != *i* ]] && return

#export PATH="$(getconf PATH):/usr/local/bin:$HOME/bin"
export PATH="/usr/bin:/usr/local/bin:$HOME/bin"
[ -r /usr/share/bash-completion/bash_completion   ] && . /usr/share/bash-completion/bash_completion
source "/usr/share/doc/pkgfile/command-not-found.bash"
#source "/etc/profile.d/autojump.bash"
shopt -s autocd
shopt -s globstar
shopt -s extglob
# shopt -s failglob
shopt -s cmdhist
shopt -s lithist
shopt -s histappend
export HISTCONTROL=ignoredups:erasedups
export HISTTIMEFORMAT='%F %T '

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
export HISTSIZE=50000000
export HISTFILESIZE=100000


# BASH PS1
## colors
red=$(tput setaf 1)
brightred=$(tput setaf 9)
green=$(tput setaf 2)
brightgreen=$(tput setaf 10)
yellow=$(tput setaf 3)
brightyellow=$(tput setaf 11)
blue=$(tput setaf 4)
brightblue=$(tput setaf 12)
magenta=$(tput setaf 5)
brightmagenta=$(tput setaf 13)
cyan=$(tput setaf 6)
brightcyan=$(tput setaf 14)
black=$(tput setaf 0)
bright_black=$(tput setaf 8)
white=$(tput setaf 7)
brightwhite=$(tput setaf 15)
bold=$(tput bold)
normal=$(tput sgr0)
dim=$(tput dim)
reset=$(tput sgr0)

git-status-checker () {
    RESULT="$(git rev-parse --show-toplevel 2>/dev/null)"
    if ! [[ -z "$RESULT" ]];
    then
      BRANCH="$(git rev-parse --abbrev-ref HEAD 2>/dev/null)"
      if [[ "$RESULT" == "$HOME" ]];
      then
	RESULT="~"
      else
	RESULT="$(basename $RESULT)"
      fi
      if [[ -z `git status --porcelain` ]];
      then
        echo -e " $green($RESULT : $BRANCH)$reset"
      else
        echo -e " $red($RESULT : $BRANCH)$reset"
      fi
    fi
}

SSH_TTY=${SSH_TTY:-`tty 2>/dev/null||readlink /proc/$$/fd/0 2>/dev/null`}

# PROMPT_COMMAND="history -a ; history -n ;$PROMPT_COMMAND"
export PS1="\[$white\]\d\[$yellow\] \W\[$reset\]\[$bold\]\$(git-status-checker)\[$reset\] [\[$blue\]\[$bold\]\u@\h\[$reset\]:${SSH_TTY/\/dev\/}:\j]\n\[$reset\]\[$brightblack\]\t\[$blue\] \$ \[$reset\]"

alias vim='nvim'

#alias tmux='env TERM=xterm-256color tmux'

# Launch TMUX

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options

alias ls='ls --color=auto'

#export FZF_DEFAULT_OPS="--extended"
[ -f ~/.fzf.bash ] && source ~/.fzf.bash

# FZF Stuff
EDITOR=nvim
export EDITOR

export FZF_DEFAULT_COMMAND='find -type f'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_DEFAULT_OPTS='-e --multi --bind ctrl-a:select-all,ctrl-d:deselect-all,ctrl-t:toggle,Home:preview-page-up,End:preview-page-down --preview-window=wrap'

# alias fzf_pyside_helper="fzf --preview 'py_helper.sh {}' --preview-window=right,70%"
alias fzf_pyside_helper="fzf --preview 'py_helper.sh {}'"


genpasswd () {
	echo -n @ && cat /dev/urandom | env LC_CTYPE=C tr -dc '[:alnum:]' | head -c 15 && echo
}

convert-transcript () {
  egrep -v '<|>' "$1" | egrep -v '^$' | tail --lines +5 | egrep [a-zA-Z0-9] | sed -n 'p;n' | xclip -i && echo 'Copied to clipboard!'
}

alias youtube-dl-proxy='youtube-dl --proxy socks5://localhost:1080'
alias youtube-dl-proxy2='youtube-dl --proxy socks5://localhost:2080'
alias curl-proxy='curl -x socks5h://localhost:5050'
alias tarot='find /home/vector/Pictures/Tarot -iname "*.jpg" | sort -R | xargs -r -d "\n" sxiv'

# Load fzf colors
if [ -f ~/.fzf.conf ]; then
  . ~/.fzf.conf
fi

# # Base16 shell
# if [ -f ~/.base16_theme ]; then
#   script_name=$(basename "$(realpath ~/.base16_theme)" .sh)
#   export BASE16_THEME=${script_name#*-}
#   . ~/.base16_theme
# fi
# 
# if [ -e "$HOME/base16/profile_helper.sh" ];
# then
#     eval "$($HOME/base16/profile_helper.sh)"
# fi

alias xclipco='xclip -o -selection clipboard'
alias npvim='nvim -c "set ft=python"'

# if command -v tmux >/dev/null 2>&1 && [ "${DISPLAY}" ] && [ -z "$TMUX" ]; then
#     # if not inside a tmux session, and if no session is started, start a new session
#     tmux;
# fi

alias nvdocsearch='grep -riIHn "" ~/.nvim/**/doc | fzfpreview'
alias pydocsearch='grep -riI "" ~/arch-pyside/src/build/sources/pyside6/doc/**/*.html | fzf'
alias pydocsearch-html='grep -riI "" ~/arch-pyside/src/build/sources/pyside6/doc/**/*.html | fzf'

alias lsp='printf "%s\n"'

alias fzfp="fzf --delimiter : --preview 'bat --style=full --color=always -H {2} {1}' --preview-window '~3,+{2}+3/2'"
alias home="ssh root@192.168.220.1 'wpa_cli -i wlan0 select_network 0'"
alias ginkgo="ssh root@192.168.220.1 'wpa_cli -i wlan0 select_network 5'"
alias list_networks="ssh root@192.168.220.1 'wpa_cli -i wlan0 list_networks'"
alias pypro='python <(cat <<\EOF'

pkgbuild-template () {
	cat <<-'EOF'
		# Maintainer: Rebel@Vectorspace <resonyze@gmail.com>
		pkgname=restview
		pkgver=v2.2.r5.gbf3480d
		pkgrel=1
		pkgdesc="A viewer for ReStructuredText documents that renders them on the fly."
		arch=('x86_64')
		url="https://github.com/mgedmin/restview"
		license=('GPL3')
		depends=('python-docutils' 'python-pygments' 'python-readme-renderer' 'python-setuptools')
		makedepends=('git')
		#replaces=('libxfce4menu')
		#conflicts=('exo')
		source=("${pkgname}"::"git+https://github.com/mgedmin/restview.git")
		# source=("${pkgname}.deb"::"https://repository-origin.spotify.com/pool/non-free/s/spotify-client/spotify-client_1.1.84.716.gc5f8b819_amd64.deb")
		# source=("${pkgname}.deb")
		# noextract=("${pkgname}.deb")
		sha256sums=('SKIP')

		source=("${pkgname}"::"git+https://github.com/thermitegod/spacefm.git"
        "git+https://github.com/thermitegod/ztd.git"
        "git+https://github.com/thermitegod/alphanumeric.git")
		sha256sums=('SKIP'
            'SKIP'
            'SKIP')

		prepare () {
		    cd "${pkgname}"
		    # This command reads from .gitmodules in the current directory
		    git submodule init
		    # third_party/ztd refers to one the entries in .gitmodules file
		    # third_party/ztd.url refers to its url property
		    git config submodule.third_party/ztd.url "$srcdir/ztd"
		    git config submodule.third_party/alphanumeric.url "$srcdir/alphanumeric"
		    git submodule update
		    mkdir -p build
		}

		
		build() {
		  cd "${srcdir}/${pkgname}"
		  python setup.py build
		}

		pkgver() {
		  cd "$pkgname"
  		  git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
  		  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
  		  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
		}
		
		package() {
		  cd "${srcdir}/${pkgname}"
		  # python setup.py install --skip-build --root="${pkgdir}"/
		  # nvim -es --cmd ":helptags doc" --cmd ":q"
		  # python setup.py install --optimize=1 --root="$pkgdir/" --prefix=/usr --skip-build
		  # tar -xf "data.tar.gz" -C "$pkgdir"
		  # install -Dm755 ../HiFile.AppImage "${pkgdir}/usr/bin/hifile"
		  # install -Dm644 ../PKGBUILD "${pkgdir}/opt/HiFile/PKGBUILD"
		}
	EOF
}

pkgbuild-template2 () {
	cat <<-'EOF'
		# Maintainer: Hexchain Tong <i at hexchain dot org>
		pkgname=neovim-remote
		pkgver=2.5.0
		pkgrel=1
		pkgdesc="Support --remote and friends for Neovim"
		arch=(any)
		url="https://github.com/mhinz/neovim-remote"
		license=('MIT')
		depends=('python-pynvim' 'python-psutil')
		makedepends=('git' 'python-setuptools')
		source=("$pkgname-$pkgver.tar.gz::https://github.com/mhinz/neovim-remote/archive/v$pkgver.tar.gz")
		sha256sums=('3ad1c060688e102b2580d01c0360918113021788b0fcc3ac08bb7fc71b4f7658')
		
		build() {
		  cd "$srcdir/$pkgname-$pkgver"
		  python setup.py build
		}
		
		package() {
		  cd "$srcdir/$pkgname-$pkgver"
		  python setup.py install --optimize=1 --root="$pkgdir/" --prefix=/usr --skip-build
		  install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname"
		  install -Dm644 contrib/completion.bash "$pkgdir/usr/share/bash-completion/completions/neovim-remote"
		  install -Dm644 README.md -t "$pkgdir/usr/share/doc/$pkgname/"
		}
	EOF
}

alias search='grep -riI "" * | fzf'
export QT_QPA_PLATFORMTHEME=qt5ct
```

## Xmodmap

`~/.Xmodmap`

```
clear lock
clear control
add control = Caps_Lock Control_L Control_R
keycode 66 = Control_L NoSymbol NoSymbol NoSymbol

remove mod1 = Alt_L
remove mod4 = Super_L
add mod1 = Super_L
add mod4 = Alt_L
remove mod1 = Alt_R
remove mod4 = Super_R
add mod1 = Super_R
add mod4 = Alt_R
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

## xpdfrc

```
# Initial Conditions
initialZoom width
initialToolbarState yes
initialDisplayMode continuous
initialSidebarState no

# Everthing j
unbind j any
bind ctrl-j any scrollDown(30)
bind ] any "run(xdotool mousemove_relative 0 60)"
bind j any "run(xdotool mousemove_relative 0 10)"
# bind j any scrollDown(30)

# Everything k
unbind k any
bind ctrl-k any scrollUp(30)
bind [ any "run(xdotool mousemove_relative 0 -60)"
bind k any "run(xdotool mousemove_relative 0 -10)"
# bind k any scrollUp(30)

# Everything l
unbind l any
bind l any "run(xdotool mousemove_relative 10 0)"
unbind ctrl-l any
bind ctrl-l any "run(xdotool mousemove_relative 60 0)"

# Everything h
unbind h any
bind h any "run(xdotool mousemove_relative -- -10 0)"
unbind ctrl-h any
bind ctrl-h any "run(xdotool mousemove_relative -- -60 0)"

# Scrolling
bind space any scrollDown(30)
bind b any "run(xdotool mousemove_relative -- -30 0)"
bind ctrl-d any nextPage
bind ctrl-u any prevPage

# Go to page
# g to go to page

# e sideways faster
bind e any "run(xdotool mousemove_relative 30 0)"

# Selecting
unbind v any
bind v any "run(xdotool mousedown 1)"
unbind c any
bind c any "run(xdotool mouseup 1)"

#Set focus right
bind ctrl-i any focusToDocWin

# o to open file
bind o any open

# usual stuff
#bind gg any gotoPage(0)
```
