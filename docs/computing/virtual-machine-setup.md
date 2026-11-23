# Setting up a VM

I commonly do development projects, particularly application-level development like web or database apps, inside a virtual machine (VM). This lets me install and configure software components at the OS level, then any necessary code to glue it together, without messing up my laptop environment. Its also easier to deploy an application from a virtual machine like this. I generally use [Multipass](https://multipass.run/) to create very minimal Ubuntu Linux VMs, and then develop from there. This usally means installing R, a Python environment, and possibly PostgreSQL, NginX, or other things necessary for the use-case. You can use an AI agent (like [Claude Code](https://claude.com/product/claude-code)) to help build this out, but that consumes a lot of tokens as the agent figures out what is available in the VM and installs everything. So, if you already know what tools you want to use, it makes sense have a basic installation of those ready to go before your or the agent start writing code.

## Set up Python

Python 3 should already be installed in an Ubuntu VM and accessible as `python3`. However, this is just the interpreter and you will still need standard class libraries and support for virtual environments (`venv`) to do development. To get those install the `python3-full` package from the Ubuntu archive, which will bring these in.

    sudo apt install -y python3-full

Virtual environments can now be created with `venv`, but there is still no way to install new Python modules into them. You'll want to use `pip`, so install it from the Ubuntu archive.

    sudo apt install -y python3-pip python3-pip-whl

Now that you have `venv` and `pip`, you or an agent can create a virtual environment and install packages into it.

    python3 -m venv .venv        # Create the .venv environment in your project
    source .venv/bin/activate    # Activate it (your shell prompt should change)
    pip install pandas openpyxl  # Install modules you need

The basics of this process are in the [Python packaging guide](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/). There are a number of other options for managing Python environments during development. The standard is `pyenv`, which is in the archive (`sudo apt install pyenv`), but you could also use [uv](https://github.com/astral-sh/uv) or [pixi](https://github.com/prefix-dev/pixi/). I've been trying `uv` during Python development recently and it is fast, lightweight, and fairly easy to learn. Install it with `curl` (which is already available).

    curl -LsSf https://astral.sh/uv/install.sh | sh

With `uv` installed, creating the Python virtual environment is handled automatically and dependencies can be declared for any project or script. For a uv-based project you'd initialize your project directory and add dependencies in two steps.

    uv init                  # Initialize the project, which creates .venv
    uv add pandas openpyxl   # Install modules you need

The creation of `.venv` is automatic and the environment is managed automatically. For running a simple script, dependencies can be declared inline using the `--with` option.

    uv run --with pandas,openpyxl my_script.py    # Run a script with dependencies.

There is plenty more about `uv` in [the docs](https://docs.astral.sh/uv/). The [pixi](https://github.com/prefix-dev/pixi/) package manager works with multiple languages and platforms, not just Python. Its a little more complex, but is a good solution when you need scientific Python packages and dependencies from the [conda ecosystem](https://conda.org/), or are building a multi-language system. 

Most of these suggestions come from the Ubuntu for Developers [Python setup page](https://documentation.ubuntu.com/ubuntu-for-developers/howto/python-setup/).


## Installing R

An up-to-date R is available in the Ubuntu software repository with a selection of useful packages, but you'll probably want to install other packages with `install.packages()`. To do that you need to add the CRAN source PPA (personal package archive).

First you need the Ubuntu archive GPG key from "Michael Rutter":

    wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc

    # You may want to verify it was installed with this:
    gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc

This lets you access the Ubuntu CRAN R PPA securely once you add it to your `sources.list`. There are a few ways add the CRAN R archive for Ubuntu to `sources.list`. You can just edit `/etc/apt/sources.list` to contain this line:

    deb https://cloud.r-project.org/bin/linux/ubuntu resolute-cran40/

But, the **recommended way** is to have a separate file in `/etc/apt/sources.list.d/`. To do that try

    # Note that the $(lsb_release -cs) expands to the Ubuntu release codename
    sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"

This will create a new file with the repository address as the filename and the recommended line inside. You should check to make sure that is the case. Once this looks ok update `apt` and install R.

    sudo apt update && sudo apt install r-base r-base-dev

Once ready, run R (just type `R` in the shell and press enter) and you should have the latest R version available. While runing R you can install some of your favorite R packages from CRAN. For example

```r
install.packages("tidyverse")
```

For more information and troubleshooting see CRAN's [Ubuntu Packages for R README](https://cran.r-project.org/bin/linux/ubuntu/fullREADME.html). Be aware that when using `install.packages()` the default behavior is for R to download and compile CRAN packages from source. This can take a long time. At times you may want to use package versions in the Ubuntu repository (`sudo apt install r-cran-tidyverse`, for example). There is also the [Posit "Public Package Manager"](https://packagemanager.posit.co/client/#/) that provides access to pre-compiled Linux R packages, and the [**r2u**](https://github.com/eddelbuettel/r2u) project. 


## Coding agents

If you are doing any agentic development (or vibe coding) you'll want to install a terminal coding agent that lets you access LLMs from the shell of your VM. I use [Claude Code](https://claude.com/product/claude-code) the most, but [OpenCode](https://opencode.ai/) or the commercial LLM provider tools, like [Codex](https://openai.com/codex/) and [Gemini CLI](https://geminicli.com/) all work similarly. Installing the agent usually involves just a one-line bash install script that you run from the terminal. For example, to install Opencode just issue

    curl -fsSL https://opencode.ai/install | bash

. The script will install most of the pieces you need, and then you can start the agent, authenticate with your LLM provider from the terminal interface, and start prompting. Be aware that running an AI agent in the shell this way gives it full access to any software and files available from the shell. If there are sensitive files in the VM, or the VM mounts the local host machine (such as your work or personal laptop), be careful not to give the agent free reign. See the [sandboxing guide](sandboxing_ai.md) for more.

## Scripting this process

It might be helpful to write a script to rebuild your favorite VM configuration when needed (such as when they fail --- see below). In `multipass` you can do this by passing a `cloud-init` YAML configuration with install commands during the launch. For example, I've created an `initvm-ubu-py-r.yaml` file in one of my GitHub repositories that sets a VM with the Python and R configurations shown above, and that can be passed as:

    multipass launch \
      --name my-dev-vm \
      --cpus 2 \
      --memory 4G \
      --disk 20G \
      --cloud-init https://raw.githubusercontent.com/gremau/sysconfigs/refs/heads/main/cloud-init-yaml/initvm-ubu-py-r.yaml

## Troubleshooting

VMs with agents running can get corrupted and stop working and you may have to kill instances and rebuild them sometimes. In `multipass` on MacOS, this happens annoyingly often, but it may be worth shutting down the instance and then trying to restart the Multipass daemon (background service) first:

    sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist
    sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist

