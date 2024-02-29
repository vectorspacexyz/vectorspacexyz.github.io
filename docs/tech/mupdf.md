---
title: mupdf
comments: true
---

# Downloading MuPDF
The latest development source is available directly from the git repository:
```
git clone --recursive git://git.ghostscript.com/mupdf.git
```
In the mupdf directory, update the third party libraries:
```
cd mupdf
git submodule update --init
```

```
make
sudo make prefix=/usr/local install
```

If something is missing, use your package manager find which package provides that file:

Ubuntu:
```
apt-file search <missing-thing>
```
ArchLinux:
```
pacman -Fs <missing-thing>
```
NixOS:
```
nix-locate <missing-thing>
```

# Desktop Entry
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
