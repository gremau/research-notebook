# R notes

## Installing and updating

### On Debian

To install R:

    sudo apt-get install r-base r-base-dev

### On MacOS

    brew install --cask r-app # --cask isn't strictly necessary
    
Using formulae (like the `r` formula)  from Homebrew is not recommended in most cases. See below.

## Package management

Install packages using `install.packages()`. Whenever R has a new update in the distribution (4.4 to 4.5, for example), packages will generally need to be reinstalled also. The location they are installed to can vary and R may ask.

Often R complains about missing Debian packages (curl, ssl) and may fail if miniconda/anaconda is already installed (may want to change dir name).

**On Debian** packages are installed from a CRAN repository to a local library directory. The default users library must be created at `~/R/x86_64-pc-linux-gnu-library/{version#}`, but packages can also be installed to `/usr/local/lib/R/site-library` if permissions allow.

**On MacOS** packages are installed to `/Library/Frameworks/R.framework/Versions/<version num>/Resources/library`

On a fresh install, I usually just start with installing `tidyverse` since it gets used so much, then pick and choose additional packages depending on use cases.

  install.packages(c('tidyverse', 'xts',  'forecast')) # for example if I had some timeseries forecasting to do...

Sometimes the core R packages on Debian go out of date, usually after a new version of R is installed and need to be updated. To update all installed packages, start R with *sudo* and run:

  update.packages(ask=False) # set ask to false if you have a big list of packages

## Jupyter R notebooks

* To run R in Jupyter notebooks install the [IRKernel](https://irkernel.github.io/) package using the appropriate MacOS or Linux method (in linux check for libzmq3-dev first).
* The kernel "spec" must be installed or made available in your environment with `IRkernel::installspec()` into an environment that also has `jupyter`. Usually this is a conda environment or similar, unless you've installed jupyter globally. This involves starting the environment with jupyter

    conda activate jupyter-env-name

Then starting R and running `IRkernel::installspec()`. Do this for any environment you want to run jupyter and R in.


## Compiling packages in MacOS/Homebrew

Its common for R to lose track of compilers or miscellaneous system files needed to build packages when using MacOS and Homebrew, especially when the source formula for `R` was installed. To fix compile errors you may be needed to point `R` to the Homebrew-installed compiler files as shown [here](https://stackoverflow.com/a/79760552). Installing the `r-app` cask, instead of the formula (just called `r`) should help since it uses already-compiled packages.

It might also be advisable to use the recommended `R` binary download method on <https://r-project.org>.

