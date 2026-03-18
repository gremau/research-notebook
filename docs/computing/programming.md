# Computer programming

This page collects info and links (to this site or others) useful in computer programming and software development, including resources for various languages, editors, or testing tools, and notes/tips for using them effectively. These resources lean towards data science and data analysis applications.

## General rules and philosophies

I like Rob Pike's 5 Rules of Programming:

#### Rule 1. You can't tell where a program is going to spend its time. Bottlenecks occur in surprising places, so don't try to second guess and put in a speed hack until you've proven that's where the bottleneck is.

#### Rule 2. Measure. Don't tune for speed until you've measured, and even then don't unless one part of the code overwhelms the rest.

#### Rule 3. Fancy algorithms are slow when n is small, and n is usually small. Fancy algorithms have big constants. Until you know that n is frequently going to be big, don't get fancy. (Even if n does get big, use Rule 2 first.)

#### Rule 4. Fancy algorithms are buggier than simple ones, and they're much harder to implement. Use simple algorithms as well as simple data structures.

#### Rule 5. Data dominates. If you've chosen the right data structures and organized things well, the algorithms will almost always be self-evident. Data structures, not algorithms, are central to programming.

Pike's rules 1 and 2 restate Tony Hoare's famous maxim "Premature optimization is the root of all evil." Ken Thompson rephrased Pike's rules 3 and 4 as "When in doubt, use brute force.". Rules 3 and 4 are instances of the design philosophy KISS. Rule 5 was previously stated by Fred Brooks in The Mythical Man-Month. Rule 5 is often shortened to "write stupid code that uses smart objects".

These were found [here](https://www.cs.unc.edu/~stotts/COMP590-059-f24/robsrules.html) but are restated in various places around the internet.

## Some techniques and conventions

Notes about data analysis techniques/conventions, independent of
language/interface.

* [Sensor data notes](sensordata_notes.md) on working with continuous sensor timeseries (from dataloggers, SNOTEL sites, etc.)
* [Data analysis workflow](data_analysis_workflow.md) - Notes on collecting, storing, and moving data through the analysis process.


## Text editing and data file handling

**[VIM](http://www.moolenaar.net/vim.html) is a great text editor**. Below are a few resources on using it effectively.

* A fairly complete [Vim commands cheatsheet](http://bullium.com/support/vim.html).
* [The Vim tips wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
* [Seven Habits](http://www.moolenaar.net/habits.html) for effective text editing.
* [My Vim notes](vim_notes.md)

**An excellent general overview of text/data file handling** in  a Unix environment
is provided by Unix for Poets, by Kenneth Ward Church. PDFs of this are all over the internet.

### Other useful resources (including some on this wiki)

* [My textfile notes](textfiles.md) - various command-line ways of manipulating text.
* [My shell scripting notes](shellscripts.md), including Unix shell scripts and useful utilities.
* [BASH hackers site](http://wiki.bash-hackers.org/doku.php) is helpful.
* [Shell scripting tutorial](http://www.vectorsite.net/tsshell.html) by Greg Goebel/Public Domain
* sed is a text stream editor great for pattern matching and replacing
    - See [this tutorial](http://www.grymoire.com/Unix/Sed.html)
    - [This page](http://sed.sourceforge.net/sed1line.txt) gives great one-line examples.\
* [Awk](https://en.wikipedia.org/wiki/AWK) is also very useful for manipulating text files.
    - [My awk notes](awk.md)
    - [The awk gateway](http://awk.info/)
    - [Awk one-liners explained part one](http://www.catonmat.net/blog/awk-one-liners-explained-part-one/)
    - [Awk one-liners explained part two](http://www.catonmat.net/blog/awk-one-liners-explained-part-two/)
    - [Awk tutorial](http://www.vectorsite.net/tsawk_1.html) by Greg Goebel/Public Domain


## Python

Python is a high-level, open-source programming language that, when
combined with some numerical, scientific, and plotting packages, makes a
very powerful tool for scientific computing and data analysis (on par
with Matlab). Useful Python extensions for scientific computing are:

* [NumPy](http://numpy.scipy.org/) - provides n-dimensional array objects and other useful numeric extensions to Python
* [SciPy](http://www.scipy.org/) - provides a number of high-level mathematical tools for use in scientific computing (integration, optimization, fourier transforms...etc
* [Matplotlib](http://matplotlib.sourceforge.net/) - a plotting library that provides publication quality plots and plotting routines that are similar to Matlab's.
* [IPython](http://ipython.org/) - an interactive shell that is designed to work well with NumPy, SciPy, and Matplotlib.
* [SciKits](http://scikits.appspot.com/scikits) - add on toolkits that complement SciPy (various statistical models, timeseries analysis, machine-learning, image processing, etc.
* The [pandas library](http://pandas.pydata.org/) - provides high-performance, easy-to-use data structures (like data frames) and data analysis tools that sit on top of NumPy. 

### Official Python resources

* [The Python documentation page](http://www.python.org/doc/) including tutorials and HowTo's
* [Python Language Reference](http://docs.python.org/reference/) - describing the syntax and core semantics of the language.
* [Python Standard Library](http://docs.python.org/library/) - describing the standard library (modules, functions, etc) distributed with Python.
* Coding in python should follow the [Python Style Guide](http://www.python.org/dev/peps/pep-0008/).
* [Official NumPy/SciPy documentation](http://docs.scipy.org/doc/)
    - [Additional NumPy/SciPy documentation](http://www.scipy.org/Additional_Documentation])
* [PyPlot documentation](http://matplotlib.sourceforge.net/api/pyplot_api.html) for the Matlab-like plotting framework in matplotlib.
* [Python package index](http://pypi.python.org/pypi) - an index of many add-on tools discussed in this wiki.

### Python forums

Python (and its scientific extensions) have a large user/developer community supporting them. These are some forums that might be helpful:

* [http://www.scipy.org/Mailing_Lists](http://www.scipy.org/Mailing_Lists)
* [http://old.nabble.com/Scipy-User-f33045.html](http://old.nabble.com/Scipy-User-f33045.html)

### My Python notes

Collected notes, tips, and tricks for using any of the Python tools
above.

* [General Python notes](python_notes.md) on debugging, code structure, and other aspects of development.
* [Ipython](ipython.md)
* [NumPy notes](numpy_notes.md) - Various notes on using the NumPy package.

### Other

* [The Python Wiki Vim page](http://wiki.python.org/moin/Vim) and [this blog
entry](http://dancingpenguinsoflight.com/2009/02/python-and-vim-make-your-own-ide/) give some interesting tips about using vim as a python source editor.


## MATLAB (and clones)

[MATLAB](http://www.mathworks.com/products/matlab/) is a
proprietary programming language and IDE that is widely used in
scientific and engineering computing.

### Resources

* [Official MathWorks documentation](http://www.mathworks.com/help/techdoc/)
* [Function reference](http://www.mathworks.com/help/techdoc/ref/f16-6011.html)
* [MATLAB Central](http://www.mathworks.com/matlabcentral/) - the official user/developer community, including a [file exchange](http://www.mathworks.com/matlabcentral/fileexchange/).
* [Kluid forums](http://www.kluid.com/mlib/index.php) has matlab and octave forums.
* [My MATLAB notes](matlab_notes.md)

### Clones of Matlab

There are a bunch of free/open-source clones of Matlab that have various
levels of syntax compatibility.

* [GNU Octave](http://www.gnu.org/software/octave/) - generally very compatible with Matlab, though some functions are missing.\
* [SciLab](http://www.scilab.org/)
* [FreeMat](http://freemat.sourceforge.net/)


## R

R is a free, open-source software environment for statistical computing
and graphics.

* [R-project homepage](http://www.r-project.org/index.html)
* [R manuals](http://cran.r-project.org/manuals.html)
* [R wiki](http://rwiki.sciviews.org/doku.php)
* [knitr](https://github.com/yihui/knitr#readme) - a nice report generating engine for R
* [My R notes](r_notes.md)


## Math and Stats tools

Many toolboxes are available, either standalone or in Python, R, and Matlab,
for math and statistical applications. See the [math toolbox page](../math/toolboxes.md) page.


## Testing data analysis functions

Code used in data analysis can perform fairly complex operations on
datasets and generate output that may be significantly changed from the
original data. The code itself can also be fairly complex and its actual
function may be difficult to discern just by reading the code or looking
at the data. It is important to verify that the result of running this
code is what is expected and that the output is accurate. Writing test
functions that call data analysis code and analyze their output is a
useful way to do this.
