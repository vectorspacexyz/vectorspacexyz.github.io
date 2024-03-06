---
title: Mkdocs material
comments: true
---

## systemd service
`~/bin/mkdocs-server.sh`:
```
#!/bin/bash

cd /home/vector/vectorspacexyz.github.io
mkdocs serve
```

Service file `~/.config/systemd/user/mkdocs-server.service`:

```
[Unit]
Description=Start the mkdocs server

[Service]
ExecStart='/home/vector/bin/mkdocs-server.sh'

[Install]
WantedBy=default.target
```
Finally:

```
systemctl --user daemon-reload

systemctl --user start mkdocs.service

systemctl --user enable mkdocs.servicek
```


## Extensions
Add the following to extension:

https://squidfunk.github.io/mkdocs-material/reference/code-blocks/

```yml
markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.superfences
```
