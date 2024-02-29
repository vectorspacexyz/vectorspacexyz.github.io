---
title: printf
date: Thu, 11 Jul 2019 22:23:12 +0530
---

```
printf "\\n<item>\\n<title>%s</title>\\n<guid>%s</guid>\\n<link>%s</link>\\n<pubDate>%s</pubDate>\\n<description><![CDATA[\\n%s\\n]]></description>\\n</item>\\n\\n" "$title" "$guid"   "$guid" "$date" "$body" > /tmp/article-body
```

Notice that there's a string within double quotes as the first argument. The rest of
the string that follows.

Just another silly example:

```
$ printf "%s here. My name is %s\n" Hello Vector
Hello here. My name is Vector
```
