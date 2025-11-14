# research-notebook

This is the repository for [Greg Maurer](https://greg.pronghorns.net)'s research notebook. It is filled with assorted lab and field procedures, activity logs for various projects, preliminary research results, and many "notes to my future self" about data analysis, statistics, programming and computing. The pages are not always well organized or well-written, but if you found your way here maybe it will have some value for you. Feel free to [contact me](greg@pronghorns.net) if you have questions or want to collaborate.

**Caution**: All data, code, results, and discussions are presented in raw, preliminary form. They have not been peer reviewed and are provided without guarantee of quality, accuracy, or safety.

The website is created using the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material) documentation publishing system and served by [GitHub Pages](https://docs.github.com/en/pages). The `main` branch of this repository contains all source files for the web pages, which MkDocs renders into HTML files that are served from the `gh-pages` branch. These HTML files are what you see when you visit [the site](https://gremau.github.io/research-notebook) with your web browser. Source files are written in [Markdown](https://www.markdownguide.org/getting-started/) - a simple, plain-text markup language.

## Site and repository layout

 The [research-notebook](https://gremau.github.io/research-notebook) site has a landing page, and lots of documentation broken into projects or themes. The markdown documents are organized into a folder heirarchy within `docs/`. Check the sidebar navigation or use search to find things.
 
This repository is structured as shown below:

```
research-notebook/
|-- docs/                       # The markdown documents
|  |-- computing/               #   Computing notes
|  |-- media/                   #   Images and other media for display
│  |-- .../                     #   (lots of other thematic subdirectories)
|  `-- index.md                 #   The site's index or homepage
|-- mkdocs.yml                  # The mkDocs configuration file
`-- README.md                   # This file

```

The `index.md` file is the source for the HTML index, or landing page, of the website. The `mkdocs.yml` file is a YAML file that defines how the website is structured, the navigation system, and the rendering options that MkDocs uses to build the HTML and other files.

## Related stuff

???

## Contributing

If you want to contribute feel free to fork and send a pull request, file an issue, of email me.


## Building and deployment

Whenever a new Git commit is pushed to the `main` branch of this repository, a GitHub Action (see it in `.github/workflows/`) will run to rebuild the website with any new content changes in the source files. This action takes all content in the `main` branch and uses MkDocs to render HTML and other files, then commits the result to the `gh-pages` branch. It is these files that are served to visitors at <https://gremau.github.io/research-notebook>.


## Contact

Feel free to [contact me](greg@pronghorns.net) if you have questions or want to collaborate.
