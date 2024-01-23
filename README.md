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
  + Research Group: "Disinformation Research Group" (will be renaming soon to "Data Science Research Group")
  + Clusters: "Pegasus" only should be fine for now.

Paste your public key contents, or upload the public key file directly.

## Usage

### Logging In

Consult this [Getting Started guide](https://hpc.gwu.edu/documentation/getting-started-guide/) for more info.

```sh
ssh <username>@pegasus.arc.gwu.edu

ssh <username>@pegasus.arc.gwu.edu -i ~/.ssh/id_rsa.pub
```

> NOTE: login is currently timing out. Emailed hpchelp@tickets.arc.gwu.edu. Need to investigate further.


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
