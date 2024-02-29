---
title: ufw
---

Do it in the following order:

# Setting up default policies
```sh
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
```

# Allowing ssh connections
```
$ sudo ufw allow ssh
```

OR

```
sudo ufw allow 22
```

# Allow connections for website
```sh
$ sudo ufw allow 80
$ sudo ufw allow 443
```

# Allow connections for website
```sh
$ sudo ufw allow 80
$ sudo ufw allow 443
```

# Allow connections for Outline VPN
```sh
$ sudo ufw allow 65162/tcp
$ sudo ufw allow 23221/tcp
$ sudo ufw allow 23221/udp
```

# Enabling UFW
```sh
$ sudo ufw enable
```
