
## Uploading Code

Cloning:

```sh
git clone git@github.com:s2t2/pegasus-notes.git
```

See permission issues. Need to configure SSH keys on the server.


Looks like there may already be a key pair ("id_ecdsa" and "id_ecdsa.pub") in your .ssh directory on the server:

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

We can try copying the public key and registering it with your GitHub profile settings (under "SSH and GPG Keys" menu, under "SSH" keys section):

```sh
cat ~/.ssh/id_ecdsa.pub
#> copy then paste in GitHub
```

NOPE that doesn't work. Maybe need a more formal SSH key?


We can try re-doing the SSH generation process, but running the commands on the server. Then use that key and add to GitHub settings.

```sh
ssh-keygen -t ed25519 -C "your.username@gwu.edu"
```


This creates a new "id_ed25519.pub" pair. Upload to GitHub.

```sh
cat ~/.ssh/id_ed25519.pub
#> copy then paste in GitHub
```


## Running Code

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
