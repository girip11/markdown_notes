---
# I have defined my custom template
title: Sample markdown doc
number: 100
---

This is a sample markdown document. It uses a custom template that looks for variable `number`. That can be populated from either YAML metadata block, `--metadata-file` option or `--metadata` or `--variable`.

```bash
# all of the below command works

# picks from the YAML metadata block
pandoc --template pandoc/example/hello_world/custom_template.html pandoc/example/hello_world/hello_world.md

# picksup from the metadata option
pandoc --template pandoc/example/hello_world/custom_template.html --metadata number=101 pandoc/example/hello_world/hello_world.md

# picks up from the --variable option
pandoc --template pandoc/example/hello_world/custom_template.html --variable number=102 pandoc/example/hello_world/hello_world.md

# picks from the metadata.json file
# we can use YAML, JSON for metadata-file
# Before running it remove the `number` from the yaml metadata.
pandoc --template pandoc/example/hello_world/custom_template.html --metadata-file pandoc/example/hello_world/hello_world_metadata.json pandoc/example/hello_world/hello_world.md

pandoc --template pandoc/example/hello_world/custom_template.html --metadata-file pandoc/example/hello_world/hello_world_metadata.yaml pandoc/example/hello_world/hello_world.md
```
