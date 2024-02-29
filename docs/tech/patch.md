---
title: patch
date: Mon, 17 Jun 2019 00:58:00 +0530
comments: true
---

# Patch
```sh
patch -p1 <st-scrollback-0.8.2.diff
```

# Reverse Patch
```sh
patch -p1 -R <st-scrollback-0.8.2.diff
```

# Create Patch
```sh
diff -Naur target.rb.original target.rb > my_awesome_change.patch
```
