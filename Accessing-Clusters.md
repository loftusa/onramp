## baulab cluster at Khoury (baulab.us)

The workstations at the Bau lab at khoury are organized in a small cluster, physically located at various desks and closets at 177 Huntington.  David can give you an account that will allow you to log into any of the machines.  Each has an A6000 GPU (or two).

To access these machines from outside Khoury, you will need to first ssh through `login.khoury.northeastern.edu`.   Note that your username on `login` will be the one given to you by Khoury whereas the username on the workstations will be the one given to you by David, which is probably different.

You can use the following in your `.ssh/config` (e.g., on your laptop) to set it up

```
Host karasuno karakuri hawaii tokyo umibozu kyoto
    ProxyJump login.khoury.northeastern.edu
    User [your username at baulab]

Host login.khoury.northeastern.edu
    User [your username at Khoury]
```

Note that each workstation is primarily used by one of the students, who may ask you to keep it clear in case they are actively using it. However if the machines are idle, feel free to use them.

 * karasuno - Koyena
 * hawaii - Nikhil
 * tokyo - Eric
 * umibozu - Arnab
 * kyoto - Masters students
 * karakuri - (dual gpu) used to serve the Memit demo
 * saitama - (dual gpu)
 * nagoya.research.khoury.edu - used for LLMs and to prototype the engine service

This cluster also contains a webserver (internally called `bauserver`) that serves `https://baulab.us/`.  In a pinch (e.g., in case of problems with the khoury login), you can log into the cluster directly over https using `https://shell.baulab.us/`.

## baukit.org

[`baukit.org`](baukit.org) is David's personal GPU cluster, which includes a few machines at David's house.  David can give you an account.  There are a few A6000 GPUs available there, and they are useful for students and collaborators who do not have access to university resources.

The computers are accessible via jump host as follows.

If you are on a mac or linux (where openssh is available), edit your `~/.ssh/config` file to have the following lines:

```
Host uno quadro duo rtx
  User [your username is on baukit.org]
  ProxyJump baukit
  LocalForward localhost:8888 localhost:8888

Host baukit
  HostName baukit.org
  User [your username is on baukit.org]
  LocalForward localhost:8888 localhost:8888

```

Then you should be able to `ssh uno` (or etc) to get to one of the machines in `baukit.org` cluster.  You may need to enter your password twice.  (The config above also sets up port forwarding on port 8888 to your local machine; change to your favorite port for jupyter).

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

That command will ask for your password once or twice, maybe for the last time ever, in order to copy you `id_rsa.pub` public key to `baukit.org`.  Now you should be able to `ssh uno` without entering your password.

The machines on `baukit.org` are ordinary Linux multiuser servers, which means that you can ssh to any machine you like, even if somebody else is already using it.  `who` will tell you who else is logged into a machine, and you should use `nvidia-smi` and `htop` to see which GPU and CPU resources are being used.  Pick a machine that is unused.

If you need some library on the machine, ask David to install it or ask for `sudo` membership.

## Discovery HPC cluster

As a student at Northeastern you can have free access to the [Northeastern Discovery cluster](https://rc-docs.northeastern.edu/en/latest/).  You just fill out the [form here](https://rc-docs.northeastern.edu/en/latest/first_steps/get_access.html).  There's a group for some specific permissions called "baulab" that you can ask to be in.

Once your account is approved you should be able to `ssh [username]@login.discovery.neu.edu`.  I shorten this to `ssh discovery` by adding the following to my `.ssh/config`:

```
Host discovery
    User [username]
    HostName login.discovery.neu.edu
```

Discovery is run as a supercomputer (HPC = high performance computing) cluster, which means that you need to use SLURM to queue up batch jobs, or to reserve machines to run interactively.

Read about [how to use SLURM on Discovery here](https://rc-docs.northeastern.edu/en/latest/using-discovery/usingslurm.html).

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

## CAIS Compute Cluster

Some of our projects have been granted access to a compute cluster of 256 A-100s (each with 80GB memory) maintained by the Center for AI Safety (CAIS). David can request access for you by adding you to one of the projects. You will receive an email from a CAIS stuff shortly after then and need to complete the following steps.

* Review CAIS policies and sign a legal contract to confirm that you agree to comply with them.
* Create your SSH key with `ssh-keygen <path>`. It will create a public and a private key on selected destination `<path>`. You will need to send your public key to CAIS.
* After receiving your signed legal contract and ssh key, CAIS will mail you your credentials to access the cluster. You can access the cluster with `ssh -i {path-to-private-key} {user-name}@{cluster-IP}`. *Your ssh keys aren't bound to your workstation. You can copy them to any device and access the cluster from there*.

#### How to work?
1. Install `conda` or `miniconda` and `jupyter`
2. Generate a config in your root directory with `jupyter notebook --generate-config`
3. Set password `jupyter notebook password`
4. Create a `jupyter.job` file and populate it with the following. Remember to change `<user_name>` and `<port>` as you want. 
```
#!/bin/bash


# get tunneling info
port=<port>
node=$(hostname -s)
user=$(whoami)

# you probably want to activate a conda environment. don't need it if you are already in the environment
source /data/<user_name>/.bashrc
conda activate <env_name>

# run jupyter notebook
jupyter-notebook --no-browser --port=${port} --ip=${node}

``` 
6. Submit a job with `sbatch --gpus=<num_gpus> jupyter.job`. Your job will be assigned a job_id. Check `squeue` and get the compute node your job was assigned to. You will need that on the next step.
8. On your local machine run
```
ssh -N -L <local_port>:<compute_node>:<jupyter_port> -i <private_ssh_key> <user_name>@<server>
```
9. On your local machine paste `http://localhost:<local_port>` on your browser. You will be prompted to input a password.
10. Do whatever your heart desires! 
11. If you need to quit, just cancel your job by `scancel <job_id>`.

Please refer to the [documentations](https://slurm.schedmd.com/tutorials.html) to learn more about SLURM and how to use it.
