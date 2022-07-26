## Setting up jupyter to serve remotely over an ssh tunnel

When you run `jupyter notebook`, by default it is not configured to let you interact with the notebook over a remote ssh session.  You have to set up a couple things to fix that.

1. Configure jupyter to allow connections from browsers running on different machines; that means setting the `ip = 0.0.0.0` option in jupyter, and selecting a specific `port` to run on.
2. Configure a `password` for jupyter.
3. And then use `ssh -L` port forwarding to make the port visible on your local machine.  Here is a specific example.

Step 1. On the cluster computer where `jupyter` is running (e.g., on `uno` or `duo`), you want to create a file `~/.jupyter/jupyter_notebook_config.json` that contains the following options:

```
{
  "NotebookApp": {
    "iopub_data_rate_limit": 0,
    "ip": "0.0.0.0",
    "open_browser": false,
    "port": 8888,
    "password": "argon:xxxxxx"
  },
  "NotebookNotary": {
    "db_file": ":memory:"
  }
}
```

(The memory db file and the iopub data rate limit are not related to remote access, but I've found them to be helpful.)  You can pick some other arbitrary port number to use in case this number conflicts with other users.

Step 2. Then to set your jupyter password hash, run `jupyter notebook password`, which will prompt you for some password.  I recommend using a different password from your regular login password, because jupyter does not protect the password very well.

Step 3. Finally, connect to the machine by saying `ssh [machine] -L 8888:localhost:8888`.  This will connect port 8888 on your own machine to port 8888 on localhost at the other end, so that when you use your own browser to visit `http://localhost:8888/` it will access juypter.

## Using jupyter on the Discovery cluster

The Discovery cluster has a special user interface to enable jupyter access, called Open On-Demand.

[Read about it here.](https://rc-docs.northeastern.edu/en/latest/using-ood/interactiveapps.html)