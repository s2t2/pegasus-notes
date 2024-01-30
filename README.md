# pegasus-notes

notes for connecting to GW's high performance computing cluster

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
  + Clusters: "Pegasus" only should be fine for now.

Paste your public key contents, or upload the public key file directly.

### Secure Network

You must be connected to GWireless on campus, or remotely through the [GW VPN](https://it.gwu.edu/vpn-global-protect).

So you will need a GW email account. Assistants at other universities can use this form to [request a GW account](https://my.gwu.edu/mod/accounts/affiliates/index.cfm).

#### GWireless

Login success when connected to GWireless!

#### VPN

In terms of downloading the VPN, here are some notes from the site:

> Palo Alto GlobalProtect 6.0.7 for macOS
>
> Download PaloAltoGlobalProtect-6.0.7-Mac.pkg (76 MB) (File will begin downloading in a few seconds)
> Palo Alto GlobalProtect allows remote access to GW resources through an encrypted connection to GW. Portal Address: gwvpn.gwu.edu
>
>Compatible with macOS 11 and higher. Note: During installation, also install System Extensions.

After you have downloaded and installed the VPN, use the "GlobalProtect" program, and enter the portal address. Then sign in with your GW microsoft account only to see that apparently your account is blocked due to too many sign in attempts. Come back another day.

TBD - login success not yet acheived via VPN.

## Usage

### Logging In

Consult this [Getting Started guide](https://hpc.gwu.edu/documentation/getting-started-guide/) for more info.


```sh
ssh <username>@pegasus.arc.gwu.edu

ssh <username>@pegasus.arc.gwu.edu -i ~/.ssh/id_rsa.pub
```

If you see a "Permission Denied" issue, email support, and they might say... "You will need to use a one-time multifactor code to log in. Please use one of these codes when prompted..." and provide you with some codes. Try to login again, and supply the code. It works. Great!

### Creating a Python Environment

Here are some notes about getting Python installed on the server:

> You can install your own Python by installing miniconda or anaconda. If you download the Linux installer on Pegasus you can install it to your home or group directory, as mentioned regarding a virtual Python environment. By using miniconda/anaconda you can create multiple virtual environments with any Python version you wish.

### Uploading Code


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
