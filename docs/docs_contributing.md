# Contributing to the Documentation  [:fab fa-github:](https://github.com/phootwork/phootwork.github.io)

The Phootwork documentation site is written in [markdown](https://daringfireball.net/projects/markdown/) and built
with [MkDocs](https://www.mkdocs.org).

## Clone and Install

1. Fork and clone the documentation [repository](https://github.com/phootwork/phootwork.github.io)
2. [Install MkDocs](https://www.mkdocs.org/#installation)
3. Install [Cinder theme](https://sourcefoundry.org/cinder/) by running: `pip install mkdocs-cinder`
4. Install [fontawesome-markdown](http://bmcorser.github.io/fontawesome-markdown/) extension by running:
   `pip install https://github.com/bmcorser/fontawesome-markdown/archive/master.zip`
   
## Markdown flavour

MkDocs uses [Python-Markdown](https://python-markdown.github.io/) with some extensions active by default. It supports the
standard markdown, markdown-extra and some of the Github-flavoured markdown features. You can find detailed information on
[https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown).

In Phootwork environment, we active also [admonition](https://python-markdown.github.io/extensions/admonition/) and
[fontawesome-markdown](http://bmcorser.github.io/fontawesome-markdown/).

### Admonition

[admonition](https://python-markdown.github.io/extensions/admonition/) extension helps to write beautiful notes or
warnings or other (see the official documentation) with a syntax like the following:

```bash
!!! Danger
    Very dangerous operation!
```

which translates into the following:

!!! Danger
    Very dangerous operation!

### Fontawesome

[fontawesome-markdown](http://bmcorser.github.io/fontawesome-markdown/) extension allow to include
[Fontawesome](https://fontawesome.com/) icons in any part of the documentation, via a simple notation: `:icon-type icon:`.
In example:
 
```bash
    *I :fas fa-heart: phootwork!*
```
translates into: *I :fas fa-heart: phootwork!*.
