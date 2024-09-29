---
title: mkdocs-material
isso: true
---
## creating a home page
* [creating a home page](https://github.com/squidfunk/mkdocs-material/discussions/6894#discussioncomment-8739517)
* [background svg](https://dev.to/kiranrajvjd/the-ultimate-css-background-pattern-resource-20m8)
* [site I used for background svg](https://heropatterns.com/)
* [setting up the footer](https://squidfunk.github.io/mkdocs-material/setup/setting-up-the-footer/)

## basics
[writing your docs](https://www.mkdocs.org/user-guide/writing-your-docs/)

## raw html
You can paste raw html into the site like this:

```html
<figure class="video_container">
  <video controls="true" allowfullscreen="true">
    <source src="../res/out.mp4" type="video/mp4">
  </video>
</figure>
```

Made use of this in [ffmpeg](ffmpeg.md).

source: [https://github.com/squidfunk/mkdocs-material/discussions/3984#discussioncomment-6374978](https://github.com/squidfunk/mkdocs-material/discussions/3984#discussioncomment-6374978)

## Add isso to mkdocs-material:
For more info check out [isso](isso.md)

```sh
vector@resonyze ~/vectorspacexyz.github.io (git)-[main] % cat docs/overrides/partials/comments.html
{% if page and page.meta and page.meta.isso is boolean %}
  {% set isso = page.meta.isso %}
{% else %}
  {% set isso = true %}
{% endif %}

{% if not page.is_homepage and isso %}
  <!-- replace disqus with isso -->
  <br>
  <h2 id="__comments">{{ lang.t("meta.comments") }}</h2>
  <div id="disqus_thread"></div>
  <script
    data-isso="https://isso.vectorspace.xyz/"
    data-isso-css="true"
    data-isso-require-author="true"
    data-isso-require-email="true"
    data-isso-reply-to-self="false"
    src="https://isso.vectorspace.xyz/js/embed.min.js">
  </script>
{% endif %}
<section id="isso-thread"></section>

```

## systemd service
`~/bin/mkdocs-server.sh`:
```bash
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

```bash
systemctl --user daemon-reload

systemctl --user start mkdocs.service

systemctl --user enable mkdocs.service
```


## extensions
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
