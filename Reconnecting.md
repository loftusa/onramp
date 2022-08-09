One of the main problems with working with servers from your laptop is that every time your laptop loses its network connection (e.g., if you close the laptop lid), you will sever all your TCP connections which will log out of `ssh` sessions and lose your jobs.

There are a few ways to fix this.

## Terminal sessions that automatically reconnect


`et` [https://eternalterminal.dev/](https://eternalterminal.dev/) and `mosh` [https://mosh.org/](https://mosh.org/) are both installed in `thevisible.net`, and both are `ssh` extensions that will automatically re-establish a connection after it is broken.

They are very useful to use instead of `ssh` on your laptop, because then you can close and reopen your laptop lid, and it will work.

To use them, you need to install them on your laptop.  On a mac, you can install them with Homebrew.

They both work by authenticating and establishing a login using `ssh`, and then handing off the connection to a reconnecting protocol.  `mosh` hands off to udp ports 60000-60100, and `et` uses tcp port 2022.  `mosh` is actually a terminal emulator that will support typeahead and do things like skip transmission of things that are no longer visible; it is designed to be good for very slow connections.   `et` focuses just on the reconnection problem.  `et` is newer and supports more ssh-like features like jumphosts and port forwarding, so I use `et`.


## Virtual screens that persist after you log out

Here the solutions are `screen` and `tmux`.  They let you manage more any number of persistent terminal screens on a server that stay active even when you are not connected.  Then you can reconnect to them next time you `ssh` into the server.  So if you quit your terminal, your jobs still keep running, and next time you re-open your terminal, you can reconnect to the same screens again.

They are linux programs that run on the server, that you run after you log in.  We keep them installed on our cluster computers.

They solve a dual problem from the problem that is solved by `et` and `mosh`, and so you can use them in combination.  I use this inside iTerm2 on my mac, to reconnect to a `tmux` inside an `et` session.

```
//opt/homebrew/bin/et quadro -j thevisible.net -c 'tmux new -As main'
```
