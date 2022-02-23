---
title: Pandoc partials example
---

Pandoc will look in the current directory for partials as well as in a sub directory called **templates** of the current directory.

Inside the partial template we inherit the parent metadata object. You can use all the built-in Pandoc template functions and variables provided by Pandoc in your partial templates.

```bash
# run
pandoc --from=markdown --to=html \
        --template partials_template.html \
        --data-dir . \
        --metadata-file partials_metadata.json \
        partials.md
```
