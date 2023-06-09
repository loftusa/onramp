A few tricks for using nagoya.research.khoury.northeastern.edu:

## Change your home directory

Since the server is in Holyoke, it is physically distant from the `/share` server.  So running with `/share/u/[username]` as your home directory tends to be very slow.  Instead you should `mkdir /disk/u/[your-username]`, which is the local disk on nagoya, and then make a home directory for yourself in that directory.

In my `.bashrc` I add the following, to use `/disk/u/[my-username]` as my homedir whenever it is present

```
# Change homedir on nagoya
if [[ -d /disk/u/$(whoami) ]]
then
    export HOME=/disk/u/$(whoami)
    echo "Changed HOME to ${HOME}"
    cd ${CHOME}
fi
```

## Use its ip address

The static ip address for nagoya is `10.201.22.179` - sometimes the DNS resolver is flaky.