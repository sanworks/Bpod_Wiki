# Bpod_Wiki
This repository is used to build the user guide and primary documentation resource for the Bpod behavior measurement system.

Go to [sanworks.github.io/Bpod_Wiki](https://sanworks.github.io/Bpod_Wiki) to view the documentation.

### Contributions

We welcome all contributions from the Bpod community! Contributor guidelines are provided in [CONTRIBUTING.md](/CONTRIBUTING.md)

### Installing and building
[MkDocs](https://www.mkdocs.org/) and the [Material theme](https://squidfunk.github.io/mkdocs-material/) are used to build the site.

`pip install mkdocs-material` will install all of the requirements necessary to build and view the site on your computer.

As a quick guide, mkdocs.yml contains the site structure and settings while docs/ contains all of the content used by MkDocs to generate the site. Each page in the documentation is a [Markdown (.md)](https://www.markdownguide.org/getting-started/) file that you can edit with a text editor like Notepad, a Markdown editor, or even in GitHub itself.

In the terminal, `mkdocs serve` creates a local server on your machine where you can preview the site and live changes by opening http://127.0.0.1:8000/ in a browser.

.github/workflows/deploy-site.yml defines a GitHub workflow that will build the site on GitHub pages whenever changes are made to `main`.

<!-- possible use of python autodoc tool in the future -->
