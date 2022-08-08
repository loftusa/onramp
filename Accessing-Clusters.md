## thevisible.net

[thevisible.net](`thevisible.net`) is David's personal GPU cluster.  David can give you an account.  There are a few A6000 GPUs available there, and they are useful for students and collaborators who do not have access to university resources.

The computers are accessible via jump host as follows.

If you are on a mac or linux (where openssh is available), edit your `~/.ssh/config` file to have the following lines:

```
Host uno quadro duo rtx
  User [your username is on thevisible.net]
  ProxyJump thevisible
  LocalForward localhost:8888 localhost:8888

Host thevisible
  HostName thevisible.net
  User [your username is on thevisible.net]
  LocalForward localhost:8888 localhost:8888

```

Then you should be able to `ssh uno` (or etc) to get to one of the machines in `thevisible.net` cluster.  You may need to enter your password twice.  (The config above also sets up port forwarding on port 8888 to your local machine; change to your favorite port for jupyter).

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

That command will ask for your password once or twice, maybe for the last time ever, in order to copy you `id_rsa.pub` public key to `thevisible.net`.  Now you should be able to `ssh uno` without entering your password.

The machines on `thevisible.net` are ordinary Linux multiuser servers, which means that you can ssh to any machine you like, even if somebody else is already using it.  `who` will tell you who else is logged into a machine, and you should use `nvidia-smi` and `htop` to see which GPU and CPU resources are being used.  Pick a machine that is unused.

If you need some library on the machine, ask David to install it or ask for `sudo` membership.

## Discovery HPC cluster

As a student at Northeastern you can have free access to the [https://rc-docs.northeastern.edu/en/latest/](Northeastern Discovery cluster).  You just fill out the [https://rc-docs.northeastern.edu/en/latest/first_steps/get_access.html](form here).

Once your account is approved you should be able to `ssh [username]@login.discovery.neu.edu`.  I shorten this to `ssh discovery` by adding the following to my `.ssh/config`:

```
Host discovery
    User [username]
    HostName login.discovery.neu.edu
```

Discovery is run as a supercomputer (HPC = high performance computing) cluster, which means that you need to use SLURM to queue up batch jobs, or to reserve machines to run interactively.

Read about [https://rc-docs.northeastern.edu/en/latest/using-discovery/usingslurm.html](how to use SLURM on Discovery here).

A typical sbatch file looks like this.  Except at the end, instead of just running `nvidia-smi` you would select some modules, activate a conda environment, and then run your python program.

```
#!/bin/bash
#SBATCH --job-name=gpu_a100_check     # Job name
#SBATCH --mail-type=END,FAIL          # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=[username]@northeastern.edu
#SBATCH --partition=gpu               # Use the public GPU partition
#SBATCH --nodes=1                     # Run on a single CPU
#SBATCH --ntasks=1                    # Run on a single CPU
#SBATCH --gres=gpu:a100:1             # Run on a single a100
#SBATCH --mem=1gb                     # Job memory request
#SBATCH --time=00:05:00               # Time limit hrs:min:sec
#SBATCH --output=gpu_a100_check.log   # Standard output and error log

# The question: A100's come in two different memory sizes.  Which size do we have?

hostname
date
nvidia-smi
```

To queue up the job, you would save this file as something like `gpu_a100_check.sbatch` and then run `sbatch gpu_a100_check.sbatch`.

Then you can check on your queued jobs with `squeue`.

Instead of queuing a batch, you can also run an interactive session like this:

```
srun --partition=gpu --nodes=1 --ntasks=1 --gres=gpu:k40m:1 --mem=10G --pty /bin/bash
```

If you run this command, you will get a shell session inside a k40m GPU machine, and you can just use it interactively just like any other ssh session.