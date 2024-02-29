---
title: xdg-settings
comments: true
---

# Browser
%date Fri, 14 Jun 2019 11:14:40 +0530

```sh
xdg-settings set default-web-browser chromium-browser.desktop
xdg-mime default chromium-browser.desktop x-scheme-handler/http
xdg-mime default chromium-browser.desktop x-scheme-handler/https
```
# Image
%date Fri, 14 Jun 2019 11:14:40 +0530

xdg-mime default mupdf.desktop application/epub+zip

Create a file called mupdf.desktop (if it doesn't alredy exist) in /usr/share/applications with the 
following conttents

```
[Desktop Entry]
Name=mupdf
GenericName=MuPDF
Exec=mupdf-gl %f
TryExec=mupdf-gl
Terminal=false
Type=Application
MimeType=application/pdf;application/x-pdf;application/x-cbz;application/oxps;application/vnd.ms-xpsdocument;application/epub+zip;image/png;image/jpeg;image/pjpeg;image/gif;image/bmp;image/jpx;image/jp2;image/vnd.ms-photo;image/jxr;image/x-portable-bitmap;image/x-portable-greymap;image/x-portable-pixmap;image/x-portable-arbitrarymap;image/png;
Categories=Office;Viewer;Graphics
Actions=View
Version=1.0
[Desktop Action View]
Name=View with mupdf
Exec=mupdf-gl %f
```
# To find application type
%date Fri, 14 Jun 2019 11:14:40 +0530

```sh
xdg-mime query filetype how-to-prove.pdf
```
# Image
%date Fri, 14 Jun 2019 11:14:40 +0530

```sh
xdg-mime default sxiv.desktop image/png 
xdg-mime default sxiv.desktop image/jpeg
```
# Set default application
%date Fri, 14 Jun 2019 11:14:40 +0530

```sh
vector@ubuntu:res$ xdg-mime query filetype isPhoneNumber.py
text/x-python

vector@ubuntu:res$ xdg-mime query default text/x-python
org.gnome.gedit.desktop
```

Set the default:

```sh
xdg-mime default feh.desktop image/png 
```
