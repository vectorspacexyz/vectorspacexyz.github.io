---
title: sed
comments: true
---

# sed luke smith
%date Mon, 03 Jun 2019 18:40:34 +0530

The kind of stuff sed is used for:

1. replacing stuff
2. printing stuff
3. deleting stuff

# Replace color in base16
%date Mon, 24 Jun 2019 00:54:06 +0530

```sh
sed "s/color00=.*/color00=\"33\/33\/33\"/" base16-eighties.sh
```

# Sed replace words
%date Mon, 03 Jun 2019 18:58:30 +0530

```
sed -i "s@/usr/local@$out@" config.mk
```

This was the sed command that we needed to compile dwm using nix. We had to substitute
/usr/local with /nix/somehash-package-name.

Look at it this way:

Whatever is between @something@ gets replaced by whatever is between @somethingelse@. The
@@ could be thought of the delimiters.


# Commenting lines in sed
%date Mon, 24 Jun 2019 00:54:02 +0530

```sh
sed -i '78,97 s/^/#/' base16-3024.sh
```
