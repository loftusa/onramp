## On baulab.us and baukit.org clusters

Two things to set up.

By default, `conda` isn't on your `PATH`, but you can run this command: `/opt/miniconda3/bin/conda init` and it should install properly.

There could be some `TypeError: memoryview: a bytes-like object is required, not 'str'` issues when installing Conda using above command, this is due to the latest Conda updates. This 'init break' could be resolved soon. Until then, treat this issue as a warning. Even though you see this error, conda would have been installed. To check the installation, you can check you ~/.bashrc for the following snippet

```
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```
If this is there, your installation is good! If not, just paste this in the ~/.bashrc 

Also, `conda` has trouble running over NFS, so make a dotconda directory on a local disk as follows: (where [username] is your username on thevisible.net:

```
mkdir -p /disk/u/[username]/dotconda
cd ~
rm -rf .conda # remove the existing .conda directory
ln -s /disk/u/[username]/dotconda .conda
```

This will make a symbolic link from your home `~/.conda` directory to your own `dotconda` directory on the computer's local disk, which will work better for `conda`.

