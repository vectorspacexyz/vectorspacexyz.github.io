---
title: systemd
date: Sat, 18 Jan 2020 13:21:44 +0530
comments: true
---

# systemd timer
date Sat, 18 Jan 2020 13:22:59 +0530

Need to things:

1. foo.service
2. foo.timer
3. some script foo.service is going to run


I placed the script in `~/bin`

script:
```sh
vector@SINGAPORE-WS:~/.config/systemd$ cat delete-tmp-img.sh
#!/bin/bash

find /home/vector/website/html/ri/ -mmin +5 -type f -exec rm -fv {} \;
```

I placed the .service in `~/.config/systemd/user/`

service:

```sh
[Unit]
Description=Remove the temp images

[Service]
Type=simple
ExecStart=/home/vector/bin/delete-tmp-img.sh

[Install]
WantedBy=multi-user.target
```

I placed the .timer in the same place as .service file, in:

```
$HOME/.config/systemd/user/
```

timer:

```sh
[Unit]
Description=Execute delete-tmp-img.service every 5 minutes

[Timer]
OnBootSec=0min
OnCalendar=*:0/15

[Install]
WantedBy=timers.target
```

I checked that everything works with:

```
systemctl --user list-unit-files
```

To make sure the unit files were deteted. And if they weren't:

```
systemctl --user daemon-reload
```

And then individually testing each unit file:

```
systemctl start --user delete-tmp-img.service
```

```
systemctl --user start delete-tmp-img.timer
```

After ensuring the above two commands worked with problems, I enable the module:

```
systemctl --user enable systemd-tmpfiles-clean.timer
```

# Disable PAM for systemd timer
date Sat, 18 Jan 2020 14:13:31 +0530

In `/etc/ssh/ssh_config`

```
UsePAM yes
```

This should be set.
