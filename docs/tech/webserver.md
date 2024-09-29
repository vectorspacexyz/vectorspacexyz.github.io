---
title: webserver
comment: true
---
Date: Sun, 23 Jun 2019 22:03:48 +0530

# Add a non root user
First thing i want to setup is to disallow root login and setup a new user

```sh
useradd -m -G sudo -s /bin/bash vector
```

# Set appropriate permissions for ssh folder
Next i want to copy the ~/.ssh folder to /home/vector/.ssh and set the
appropriate permissions for the ssh folder.

```sh
cp -R ~/.ssh /home/vector/.ssh
chown -R vector:vector .ssh
chmod 700 .ssh
chmod 600 .ssh/authorized_keys
```

# Disallow root logging in /etc/sshd_config
Edit /etc/ssh/sshd_config and disallow root login. Before closing root shell
make sure you are able to login with the user vector and that you are able to
use sudo.

Now you are done with the security part. good job!

# Certbot
```sh
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx certonly
```

Now we have the certificate and key in /etc/letsencrypt/live/resoaxes.com/;

Next, tell nginx to turn on ssl, and point it to the ssl certificate and ssl certificate key

Add the following block to the server section:

```sh
ssl on;
ssl_certificate /etc/letsencrypt/live/resoaxes.xyz/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/resoaxes.xyz/privkey.pem;
```

Since ssl connections listen on port 443 and not 80, edit 80 and replace it with 440 

```sh
listen 443 default_server;
listen ![pic](::):443 default_server;
```


Create a new server block that listens on port 80, rewrite links to https

```sh
server {
        listen 0.0.0.0:80;
         server_name resoaxes.xyz www.resoaxes.xyz;
         rewrite ^ https://$host$request_uri? permanent;
}
```

Now everything works. Redirecting to the website with https!

The next thing to setup is the https protected directories. These will be my
private notes.

# Password protect a directory in nginx
```
sudo apt-get install apache2-utils
sudo htpasswd -c /etc/nginx/.htpasswd vector
sudo vim /etc/nginx/sites-available/resoaxes.xyz
```

Create a new location block underneath the original to server your protect directory

```
location /test {
	auth_basic "Restricted Access";
	auth_basic_user_file /etc/nginx/.htpasswd;
}
```

# Enable HTTP/2
```sh
server {
   listen 443 http2 default_server;
   listen ![pic](::):443 http2 default_server;
   #... all other content
}
```

# Enable gzip compression
```sh
server {
   #...previous content
   gzip on;
   gzip_types application/javascript image/* text/css;
   gunzip on;
}
```

I haven't done this because of this vulnerability: https://en.wikipedia.org/wiki/CRIME

# Outline VPN
Install docker and make sure its running
```
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

Installing outline VPN. *Make sure you have maximized your terminal at this*
*point, we will need the output from this command.*

```
sudo su
wget -qO- https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh | bash
```

Paste the output of the command somewhere. Its important.

... After doing all this, I'd say the recommended way to install outline would
be to download Outline App Manager and following  the 2 line instruction there.

## Neovim config

`~/.config/nvim/init.lua`:

```lua
-- Remap Space to Leader in normal and visual mode
vim.keymap.set({ 'n', 'v' }, '<Space>', '<Nop>', { silent = true })
vim.g.mapleader = " "

-- H,L to switch tabs in normal mode
vim.keymap.set('n', 'H', function()
        vim.cmd("tabp")
end, { desc = 'Switch to previous tab' })

vim.keymap.set('n', 'L', function()
        vim.cmd("tabn")
end, { desc = 'Switch to next tab' })

-- Copy the directory containing file
vim.keymap.set('n', '<leader>cd', function()
  local cwd = vim.fn.expand('%:p:h')
  vim.fn.setreg('+', cwd)
  print("Copied " .. cwd .. " to + register.")
end, { desc = "Copy the directory containing file" })

-- Copy the file path
vim.keymap.set('n', '<leader>cp', function()
  local fp = vim.fn.expand('%:p')
  vim.fn.setreg('+', fp)
  print("Copied " .. fp .. " to + register.")
end, { desc = "Copy the file path" })

```
