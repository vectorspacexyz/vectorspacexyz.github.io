---
title: nginx
---

# Password protect a directory in nginx
date Sun, 23 Jun 2019 22:47:08 +0530
```sh
sudo apt-get install apache2-utils
sudo htpasswd -c /etc/nginx/.htpasswd vector
sudo vim /etc/nginx/sites-available/resoaxes.xyz
* create a new location block underneath the original to serve your protect directory
. . .
location /test {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/.htpasswd;
}
. . .
```

# Error checking in nginx
date Sun, 23 Jun 2019 22:46:22 +0530

```
sudo cat /var/log/nginx/error.log
```
