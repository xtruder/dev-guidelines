# Filestyle

Filestyle denotes how different files are idented, how lines are separated and how
files are enoded, if there is new line at the end of the file and other filestyle
details.

## So many editors, so many filestyles

Different editor configs for file indent styles, line separators and file encodings
can cause a lot of trouble when editing files. While developers can arrange on these
details, it can still cause a lot of problems, since many developers have no idea
how they are actually editing their code. A stanadard is needed that every editor can
read and write without programmer intervenience.

### EditorConfig

[`.editorconifg`](http://editorconfig.org/) is one widely used specification to define
file styles. Editorconfig is provided as a plugin for any major editor. More information
can be found on http://editorconfig.org/

#### Example `.editorconfig` file

```
root = true

[*]
indent_style = tab
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[{package.json,*.yml,*.json}]
indent_size = 2

[{*.js}]
indent_size = 4
```
