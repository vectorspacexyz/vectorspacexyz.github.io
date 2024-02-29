---
title: Shell Scripting
date: 2024-02-21
---
## cut
cut -d*delimiter* -f*index* is pretty useful in shell scripting.

```bash
for i in *;
do
    VAL=$(du -sh "${i}" | cut -d'   ' -f1)
    if [[ $VAL ==  0 ]]
    then
        rm "$i"
        echo "$i removed"
    fi
done
```

To paste the delimiter in the terminal I used Ctrl-v. Ctrl-v + Tab to insert
tab character. Ctrl-v is used in vim in terminal to insert characters by their
unicode.
