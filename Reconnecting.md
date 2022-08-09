One of the main problems with working with servers from your laptop is that every time your laptop loses its network connection (e.g., if you close the laptop lid), you will sever all your TCP connections which will log out of `ssh` sessions and lose your jobs.

There are a few ways to fix this.

## Terminal sessions that automatically reconnect


`et` [(Eternal Terminal)](https://eternalterminal.dev/) and `mosh` are both installed in `thevisible.net`, and both are `ssh` alternatives that will automatically re-establish a connection after it is broken.  `et` is a newer and supports jumphosts and port forwarding, so I use `et`.

They are very useful to use instead of `ssh` on your laptop, because then you can close and reopen your laptop lid, and it will work.

I use this:

```
//opt/homebrew/bin/et quadro -j thevisible.net -c 'tmux new -As main'
```