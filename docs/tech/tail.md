---
title: alternative to tee with tail
date: Wed, 26 Jun 2019 19:21:58 +0530
comments: true
---

The tail command in linux is something that you can use to observe a file in real
time. Use the `-f` option for this to happen.

Example, to track the output from this command:

```sh
while true; do date >> /tmp/observation-file; sleep 1; done
```

You can do..

```sh
$ tail -f /tmp/observation-file
Wed Jun 26 16:47:06 IST 2019
Wed Jun 26 16:47:08 IST 2019
Wed Jun 26 16:47:09 IST 2019
Wed Jun 26 16:47:10 IST 2019
Wed Jun 26 16:47:11 IST 2019
Wed Jun 26 16:47:12 IST 2019
Wed Jun 26 16:47:13 IST 2019
Wed Jun 26 16:47:14 IST 2019
Wed Jun 26 16:47:15 IST 2019
Wed Jun 26 16:47:16 IST 2019
Wed Jun 26 16:47:17 IST 2019
Wed Jun 26 16:47:18 IST 2019
Wed Jun 26 16:47:19 IST 2019
Wed Jun 26 16:47:20 IST 2019
Wed Jun 26 16:47:21 IST 2019
Wed Jun 26 16:47:22 IST 2019
Wed Jun 26 16:47:23 IST 2019
Wed Jun 26 16:47:24 IST 2019
Wed Jun 26 16:47:25 IST 2019
Wed Jun 26 16:47:26 IST 2019
Wed Jun 26 16:47:27 IST 2019
Wed Jun 26 16:47:28 IST 2019
Wed Jun 26 16:47:29 IST 2019
Wed Jun 26 16:47:30 IST 2019
Wed Jun 26 16:47:31 IST 2019
```

If you don't know about tee.
