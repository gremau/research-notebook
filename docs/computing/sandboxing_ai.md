# Sandboxing AI agents

Running agentic AI (like [Claude Code](https://claude.com/product/claude-code)) on your local machine seems a little risky since an agent could conceivably gain access to private data, system files, etc. There are a few options to confine the AI to a "sandbox," which could be something like a Docker container or virtual machine (VM). A virtual machine is a little more isolated from the local system, so that is what I use. 

## Using Multipass with Claude Code

[Multipass](https://multipass.run/) is a lightweight VM orchestrator from Canonical that runs in MacOS, Windows, and Linux. You can use Multipass to set up Ubuntu VMs from the commandline in a fairly frictionless way on your local host machine (your laptop, for example). On Mac I install with 

    brew install multipass

, but this would be different for Windows or Linux. Once it is installed, you can start the GUI if you want, or just use the commandline interface to spin up VMs. Here is how to start a VM for Claude to work in.

    multipass launch --name claude-sandbox --disk 20G --memory 4G

It takes just a minute to spin up the VM, then you can enter the shell and explore.

    multipass shell claude-sandbox

There isn't much there - Multipass just creates an Ubuntu Linux installation with a few things installed, and your shell will be in the home directory of the `ubuntu` user. If you want to mount a local directory on your host machine to the new VM you can do that from the host shell. For example, you could mount your home directory to the `claude-sandbox` VM.

    multipass mount $HOME claude-sandbox

If you now run `multipass info claude-sandbox` you'll see the mount listed. The home directory is mounted at `/home/ubuntu/username`, and you'll find it there if you are logged into the `claude-sandbox` VM's shell. **You probably want to be careful about what parts of your local filesystem you mount in the VM!!!** Claude, or you, can read, edit and delete things on the host filesystem once it is mounted in the VM. You can pass the `--read-only` option during mounting to prohibit write access. To unmount a local directory just use `multipass umount`.

    multipass umount claude-sandbox:/home/ubuntu/username 

!!! note
    
    On MacOS, the directories in `~/Library/CloudStorage` are not accessible to VMs by default (unclear why). To access their content after they are mounted to your VM, you will have to enable "Full Disk Access" by the VM by toggling on access for the `multipassd` application in **System Settings > Privacy & Security > Full Disk Access**. This seems slightly dangerous, but I think as long as we can trust Multipass and not be careless about granting directory access to Claude it should be ok. Right?

Once your environment is set up and you are in the shell of your VM, you'll want to install Claude Code.

    curl -fsSL https://claude.ai/install.sh | bash

From here, you'll follow the prompts to connect Claude to your Anthropic account, and away you go.