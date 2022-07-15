There are several gpu machines you can use in the cluster at [thevisible.net](`thevisible.net`).  They are accessible via jump host as follows.

If you are on a mac or linux (where openssh is available), edit your `~/.ssh/config` file to have the following lines:

```
Host uno quadro duo rtx
    User [your username is on thevisible.net]
    ProxyJump thevisible.net
```

Then you should be able to `ssh uno` to get to one of the machines in `thevisible.net` cluster.  You may need to enter your password twice.

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

Similarly you should get an account on the [https://rc-docs.northeastern.edu/en/latest/](Northeastern Discovery cluster) by filling out the [https://rc-docs.northeastern.edu/en/latest/first_steps/get_access.html](form here), and then after you have access...

