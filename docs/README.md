# Sweet Home 3D Online documentation

This sub-project contains the SH3DO documentation.

## How to generate

The documentation source is written in Latex. [Pandoc](http://pandoc.org) version 2+ is required to generate the documentation in markdown and HTML.

Generate the markdown version:

```
> cd docs
> pandoc -w gfm --shift-heading-level=2 -s --toc --number-sections -B header.md -o sh3d.online.md sh3d.online.tex
```

Generate PDF version (with pdflatex):

```
> pdflatex sh3d.online.tex
```

Generate HTML format:

```
> pandoc -s --css=sh3d.online.css -o sh3d.online.html sh3d.online.tex
```

## License

Copyright (C) 2020 CINCHEO <renaud.pawlak@cincheo.fr>
All rights reserved
 
