# pegasus-notes

Notes for connecting to GW's [high performance computing cluster](https://hpc.gwu.edu), Pegasus.

## Getting Started

### Generating SSH Keys

Generate a new SSH public / private key pair by following this [guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

The public key can be shared with anyone, without concern. The private key should never be shared.

Verify you see some files ("id_rsa" and "id_rsa.pub"):

```sh
ll ~/.ssh
```

Print the contents of the public key:

```sh
cat ~/.ssh/id_rsa.pub
```

### Submit Access Reqest Form

Fill out this [access requet form](https://hpc.gwu.edu/getting-access/), with the following info:

  + PI: Michael Rossetti
  + Research Group: "Data Science Research Group" (rossettigrp)
  + Clusters: "Pegasus" only should be fine for now

Paste your public key contents, or upload the public key file directly.

### Secure Network

You must be connected to GWireless on campus, or remotely through the [GW VPN](https://it.gwu.edu/vpn-global-protect).

For either option, you will need a GW email account. Researchers and assistants at other universities can use this form to [request a GW account](https://my.gwu.edu/mod/accounts/affiliates/index.cfm).

#### GWireless

Login success when connected to GWireless!

#### VPN

In terms of [downloading the VPN](https://it.gwu.edu/vpn-global-protect), here are some notes from the site:

> Palo Alto GlobalProtect 6.0.7 for macOS
>
> Download PaloAltoGlobalProtect-6.0.7-Mac.pkg (76 MB) (File will begin downloading in a few seconds)
> Palo Alto GlobalProtect allows remote access to GW resources through an encrypted connection to GW. Portal Address: gwvpn.gwu.edu
>
>Compatible with macOS 11 and higher. Note: During installation, also install System Extensions.

After you have downloaded and installed the VPN, you may need to give access to it via the Security and Privacy settings.

To use the VPN, launch the "GlobalProtect" program, and enter the portal address. Then sign in with your GW microsoft account (net_id@gwu.edu).


## Logging In

Consult this [Getting Started guide](https://hpc.gwu.edu/documentation/getting-started-guide/) for more info.


```sh
ssh <username>@pegasus.arc.gwu.edu

ssh <username>@pegasus.arc.gwu.edu -i ~/.ssh/id_rsa.pub
```

If you see a "Permission Denied" issue, email support, and they might say... "You will need to use a one-time multifactor code to log in. Please use one of these codes when prompted..." and provide you with some codes. Try to login again, and supply the code. It works. Great!

Every time you log in, it will prompt you for a 2FA code, so store those OTP codes somewhere, and setup multifactor auth as soon as possible (see section below).

### Multifactor Auth

After using one of the OTP codes to login for the first time, run `google-authenticator` to configure multifactor auth via your Authenticator app. Answer "y" for all the questions. Scan the QR code with your Authenticator app. In the future you will use a code generated by the Authenticator app instead of the OTP / scratch codes.

### Navigating the Filesystem

Your home folder is `/SEAS/home/<username>`.

Be aware of your SSH keys and SSH config:

```sh
ll ~/.ssh
#> authorized_keys
#> cluster
#> cluster.pub
#> config
#> id_ecdsa
#> id_ecdsa.pub
#> known_hosts
```




## Python Environment Setup

There are many [installable "modules"](https://hpc.gwu.edu/available-modules/) available, including a module for Python 3.10. However we want project-specific virtual environments, so let's use the miniconda module:

```sh
# module load python3/3.10.11

module load miniconda/miniconda3
```

Once installed, we should have access to anaconda command line tool.

Listing environments:

```sh
conda info --envs
```

Creating and activating an environment:

```sh
conda create -n my-first-env python=3.10

#conda activate my-first-env
# conda command may require some bashrc setup, they want us to use source instead:
source activate my-first-env
```

Verify the environment is setup properly:

```sh
python --version
pip --version

python -i # enter into python shell, test things out
# exit()
```










## Uploading Code

We need to clone repositories from GitHub. It looks like git is pre-installed on the server:

```sh
which git
git --version
```

Attempting to clone a repo from GitHub:

```sh
git clone git@github.com:s2t2/pegasus-notes.git
```

You may run into permissions issues. Need to configure SSH connection from server to GitHub (see section below).

### Configuring SSH Keys for GitHub

Generating SSH key on the server:

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

This creates a new "id_ed25519.pub" key pair. Upload it to GitHub via the SSH settings.

```sh
cat ~/.ssh/id_ed25519.pub
#> copy then paste in GitHub
```


TBD - Also setup ssh agent?

```sh
eval "$(ssh-agent -s)"
#> Agent pid 59566


ssh-add ~/.ssh/id_ed25519
```


Try to clone again and it should work? Might need to wait a few minutes for changes to take effect?




## Running Python Programs

Here is some guidance from support about getting Python code set up on the server...

> The git module should be available to run git clone and git pull, and git push, you cannot push from github, but initiate the transfer from Pegasus.
>
> The python virtual environment can be built in your home directory (/SEAS/home/<username>), or group directory (/SEAS/groups/<groupname>) to share with others in your group, or on the lustre (/lustre/groups/<groupname>), and similar to any packages you need.
>
> Please do not run jobs against the NFS shares (/SEAS/home/) and groups (/SEAS/groups) but use /lustre instead, i.e. files should be read from and written to /lustre/groups/<groupname> directory.
>
> We use slurm for job scheduling which can run into interactive move (salloc) or batch mode (sbatch). The batch mode requires a shell script as a wrapper to call your python code.

Example shell script:

```sh
#!/bin/bash
#SBATCH --time 00:10:00
#SBATCH -p nano
#SBATCH -o temperature.out
#SBATCH -e temperature.err
#SBATCH -N 1

. ~/miniconda3/etc/profile.d/conda.sh
python3 ~/temperature.py | sort -n
```
