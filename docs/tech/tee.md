---
title: tee
date: Wed, 26 Jun 2019 19:23:25 +0530
comments: true
---

# See command output and log at the same time

```
date | tee -a mylog.og
```

Remember to use the -a flag here to append to file. So the usage presented here is a
simple:

```sh
tee -a someFile
```
