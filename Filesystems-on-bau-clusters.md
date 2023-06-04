On all the bau cluster machines, you should be aware of three types of filesystems:

## NFS filesystems

NFS filesystems are mounted under `/share`.  These files are actually pulled off of a shared NFS server for the whole cluster, so that whatever files you see under `/share` will be the same even if you switch to another machine on the same cluster.  Notice that there are two different clusters: the university baulab.us cluster, and David's personal baukit.org cluster, and each one has its own NFS server which is unrelated to the other one.  But they both have the same overall directory organization, as follows. (The subdirectoriees of `/share` are mounted on-demand so may not appear if you just `ls /share`, but if you `ls /share/u` etc one level further, you should be able to see the following).  Under `/share` there are three subdirectories that should be available:
    * `/share/u` includes all network user home directories.
    * `/share/projects` includes a directory for each project that is shared among more than one person.  You can create a new directory for your own project.  To be useful, you will generally want to make sure the project directories are group-writeable, e.g., `chmod g+rwX -R /share/projects/myproject`.  (If you create something in here that is not, David or somebody might go and make it writeable later.)
    * `/share/data/datasets` includes a bunch of standard datasets.  If you find yourself using some dataset files that you'd like to make available to lots of projects, arrange them nicely in here, and once it's nicely arranged, you can make them read-only.

## Local bulk disk

On some machines there is a local `/disk` directory.  This is a large local directory designed for you to use, in case you need something potentially faster than NFS.  NFS has the disadvantage that the data all needs to come over the network, but `/disk` will be inside the box of the machine.  This is especially important for `nagoya` which is far away from the khoury NFS server. You can create your own directories under `/disk` and you should follow these conventions.
    * `/disk/u/[username]` for your own personal directories.  On nagoya, I set it up to be just like my homedir and set `$HOME` to point to this.
    * `/disk/projects/[projectname]` for your projects.

## Local system disk

Directories outside `/share/` and outside `/disk/` are ordinary systems directories that are local to the machine.  For example, each machine has its own `/etc/` and `/usr/` and `/var/` and `/opt/` directories.  These are all for unix configuration, binaries, logs, and libraries, and usually won't want to write anything into these directories.
