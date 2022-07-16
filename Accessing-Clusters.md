## thevisible.net

[thevisible.net](`thevisible.net`) is David's personal GPU cluster.  David can give you an account.  There are a few A6000 GPUs available there, and they are useful for students and collaborators who do not have access to university resources.

The computers are accessible via jump host as follows.

If you are on a mac or linux (where openssh is available), edit your `~/.ssh/config` file to have the following lines:

```
Host uno quadro duo rtx
    User [your username is on thevisible.net]
    ProxyJump thevisible.net
```

Then you should be able to `ssh uno` (or etc) to get to one of the machines in `thevisible.net` cluster.  You may need to enter your password twice.

To avoid requiring your password, you should set up an ssh key.  Do the folowing:

On your mac:
```
> ssh-keygen -t rsa
```
(Hit enter to accept defaults.)  This creates two files: `~/.ssh/id_rsa`, which is a secret key that you should never share or copy somewhere else.  And `~/.ssh/id_rsa.pub`, which is your public key which you will copy to anywhere that you will need it.

Then on your mac:
```
> ssh-copy-id [user]@uno
```

Now you should be able to `ssh uno` without entering your password.

The machines on `thevisible.net` are shared multiuser resources, which means that you can ssh to any machine you like, even if somebody else is already using it.  You can use `nvidia-smi` and `htop` and `who` to see who else may be using the machine, and pick a machine that is unused.  If you need some library on the machine, ask David to install it or ask for `sudo` membership.

## Discovery HPC cluster

As a student at Northeastern you can have free access to the [https://rc-docs.northeastern.edu/en/latest/](Northeastern Discovery cluster).  You just fill out the [https://rc-docs.northeastern.edu/en/latest/first_steps/get_access.html](form here).

Once your account is approved you should be able to `ssh [username]@login.discovery.neu.edu`.  I shorten this to `ssh discovery` by adding the following to my `.ssh/config`:

```
Host discovery
    User [username]
    HostName login.discovery.neu.edu
```

Discovery is run as a supercomputer (HPC = high performance computing) cluster, which means that you need to use SLURM to queue up batch jobs, or to reserve machines to run interactively.
