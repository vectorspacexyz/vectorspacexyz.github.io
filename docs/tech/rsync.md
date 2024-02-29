---
title: rsync
date: Mon, 17 Jun 2019 23:08:48 +0530
comments: true
---

# Rsync through SSH
-e flag to specify the protocol

```sh
rsync --progress -e ssh root@vectorspace.xyz:/root/something.apk  ./
```

# Get files from remote
After first creating the website directory, I did:

```sh
rsync -chavzP --stats vector@resonyze.xyz:~/website/ website/
```

# Rsync backup files from list
```sh
backup ()
{
  cd ~
  rsync --info=progress2 -r -z --delete --files-from=sync-files . vector@resonyze.xyz:~/backup/
}
```

Example of text file file: [s](vfile:sync-file.md)

```
bin/
.xinitrc
.xbindkeysrc
.Xmodmap
.vimrc
.bashrc
.inputrc
.bash_history
sync-files
notes/
.config/compton/compton.conf
.tmux.conf
books/
```
