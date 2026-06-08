# Text files

Text files are perhaps the simplest and most interoperable format for
storing and exchanging data. Here are some tips and utilities for
manipulating and effectively using text files in a UNIX-like
environment.

 **See also:** [General programming page](programming.md), [the Awk page](awk.md), [Vim notes](vim_notes.md), the [shell scripting page](shellscripts.md).

## Text operations

### Find (grep) text in the contents of multiple files

    grep -Hrn 'search term' path/to/files

where `-H` prints the filename (default if called on a dir), `-r` is
recurse (search in subdirectories), and `-n` prints the line number
where the search term occurs in the file. To match a particular filename
or extension add `--include=file.*` (or any regexp).
`--exclude-dir=dir` is useful for excluding directories like `.svn` and
`.git`.

`find`, followed by a call to `grep` is also an option.

    find /path -type f -exec grep -l "string" {} +

where `-type f` means search only files, `-exec grep...` specifies the
`grep` search to run for `{}` (each file found).

### Search/replace text in multiple files

Using just sed:

~~~{.bash}
# Replace "phrase1" with "phrase2" in all "file*.txt" files in the current directory, 
# and append ".bak" to the original files.
sed -i.bak 's/phrase1/phrase2/g' file*.txt 
~~~

Using find, and piping to sed - no backup copy made

~~~{.bash}
  find . -type f -print0 | xargs -0 sed -i 's/phrase1/phrase2/g'`
~~~

## Diff/merge

There are plenty of diff-merge tools, including *vimdiff*. For diffing
files with long lines (paragraphs in documents),
[wdiff](http://www.gnu.org/software/wdiff/) should help.

## Markup languages

When drafting text documents, markup languages can be used to add
formatting or other information to plain text. This makes text more
readable, and the markup text can be parsed by an interpreter to render
the content in other document formats (ie. HTML, LaTeX documents, .docx
files, etc.).

* **Lightweight markup languages**
  * [Markdown](http://daringfireball.net/projects/markdown/)
  * [reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickstart.html)
  * [ASCIIdoc](http://www.methods.co.nz/asciidoc/)
* **Markdown extensions/supersets**
  * [Pandoc](http://pandoc.org) Markdown (part of the Pandoc document converter)

I tend to use [Pandoc](pandoc.md) to create documents.
