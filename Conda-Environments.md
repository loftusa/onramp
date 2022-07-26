## The visible.net Cluster
By default, `conda` isn't on your `PATH`, but you can run this command: `/opt/miniconda3/bin/conda init` and it should install properly.

Also, `conda` has trouble running over NFS, so make a dotconda directory on a local disk as follows: (where [username] is your username on thevisible.net:

```
mkdir -p /disk/u/[username]/dotconda
cd ~
rm -rf .conda # remove the existing .conda directory
ln -s /disk/u/[username]/dotconda .conda
```


