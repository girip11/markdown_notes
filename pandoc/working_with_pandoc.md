# Markdown, Pandoc and templates

## YAML metadata block

We know pandoc can be used to convert documents from one format to many other format(hence the name)Pandoc supports YAML metadata block. The keys that are supported by default in the YAML metadata block for a particular language can be found from [this github repository of pandoc templates][1]

Default templates for a given pandoc installation can be found at `/usr/share/pandoc/data/templates`.

In short, pandoc template decides how the yaml metadata is used when converting to a target format. So we will always populate the variables in the yaml metadata blocks that are defined in the target language template.

Say we author a markdown document and want to generate HTML document. Then we will look at the varialbes defined in `default.html5` template and populate the required variables in the YAML metadata block (ex- `header-includes`, `author-meta` etc).

## Example markdown with YAML metadata block

- [Markdown default template can be found here][2]. Default template for a language can be obtained from the command `pandoc -D <LANG_FORMAT>`. To know the valid values of `LANG_FORMAT` look at the `default.*FORMAT*` in the [pandoc-templates repo][1].

`pandoc -D markdown` outputs the below template

```text
$if(titleblock)$
$titleblock$

$endif$
$for(header-includes)$
$header-includes$

$endfor$
$for(include-before)$
$include-before$

$endfor$
$if(toc)$
$table-of-contents$

$endif$
$body$
$for(include-after)$

$include-after$
$endfor$
```

Say we want to convert our markdown to html we need to look at `default.html`(or custom html template defined by us) and populate the required variables in the YAML metadata block.

```Markdown
---
# notice the keys match the variables in the default.html5 template
title: "Example markdown document"
toc: true
header-includes: |
  <style>
    h1 {
      text-align: center;
    }
  </style>
include-before: |
  <script>
    console.log("Hello world");
  </script>
include-after: |
  <script>
    document.body.className = "markdown-body";
  </script>
---

This is the body of the markdown document.
```

## Custom templates

We can author custom templates and pass to pandoc using `--template` option. This has the following benefits.

- We can transform the document in a custom way
- We can define custom template variables

We can use custom templates readily available openly for our purposes. [kjhealy/pandoc-templates](https://github.com/kjhealy/pandoc-templates) is one such github repo containing custom templates for PDF, HTML, docx.

Syntax for authoring custom template is available [here](https://pandoc.org/MANUAL.html#template-syntax)

## Template variables

We can pass the values to the variables defined in the template in the following ways

- Authoring the YAML metadata metadata block inline
- Place it inside a separate YAML/JSON file and pass to `pandoc` via `--metadata-file` option (helpful when same metadata needs to be applied to multiple markdown documents)
- Directly pass the key and value through the command line option `--metadata=key:value`.
- Directly pass the key and value through the command line option `--variable key=value`.

Refer to the example `example/hello_world/hello_world.md` markdown to understand the variable substitution

### Template variables types

We have predefined variables serving different purposes.

- Metadata variables like author, description, date
- Target language specific variables like variables for HTML, HTML math, HTML slides(to convert to reveal-js slide), latex etc

### Template variable priority

Listing in the order of decreasing priority.

- `--variable` commandline option (highest priority)
- `--metadata`
- Inline YAML metadata block
- `--metadata-file`

Refer to the example `example/hello_world/hello_world.md` markdown to understand the variable priority.

## Partial templates

We can split the templates in to subtemplates.

Inside the partial template we inherit the parent metadata object. You can use all the built-in Pandoc template functions and variables provided by Pandoc in your partial templates.

To invoke the partial template we can either use `partial_template_basename()` or `partial_template_basename.extension()`

### Template look up directory

Pandoc will look in the current directory of invoking template for partials as well as in a sub directory called `templates` of the current directory.

If the templates are not found, then the default data directories are searched. Default data directories can be obtained using `pandoc --version`.

We can change the data directory using `--data-dir` option. Refer to the example `example/partials/partials.md` markdown to understand partial templates.

---

## References

- [Pandoc man](https://pandoc.org/MANUAL.html)
- [What can I control with YAML header options in pandoc?](https://stackoverflow.com/questions/26395374/what-can-i-control-with-yaml-header-options-in-pandoc)
- [Pandoc templates](https://github.com/jgm/pandoc/blob/master/doc/customizing-pandoc.md#template-variables)
- [Pandoc partial templates](https://rsdoiel.github.io/blog/2020/11/09/Pandoc-Partials.html)

[1]: https://github.com/jgm/pandoc-templates
[2]: https://github.com/jgm/pandoc-templates/blob/master/default.markdown
