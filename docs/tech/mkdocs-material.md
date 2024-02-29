---
title: Mkdocs material
comments: true
---

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
