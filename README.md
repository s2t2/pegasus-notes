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
