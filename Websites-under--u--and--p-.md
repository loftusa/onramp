## `baukit.org` and `baulab.us` built-in websites

The easiest way to share data is to put it on a website.

If you make a (globally-readable) directory under your `/share/u/[username]/www`, then it will become visible at a website at `https://baulab.us/u/[username]/` if you are at the Khoury baulab cluster, or at `https://baukit.org/u/[username]/` if you are on David's personal cluster.  You don't have to literally copy things to this directory; you can symlink to anything else under `/share` to share it on your website (using `ln -s`).  Note that `baulab.us` and `thevisible.net` are running on very small computers, not your gpu computer, and it only has access to the files under `/share`, not the local files that are just on your computer.

Similarly, for projects, if you make a (globally-readable) directory under your `/share/projects/[projectname]/www`, then it will become visible at a website at `https://baulab.us/p/[username]/` or `https://thevisible.net/p/[projectname]/`. 

These are basic apache servers.  If the default apache configuration isn't working for you, you can put an `.htaccess` in a directory to alter the settings.  [Lots about htaccess on the web.](https://monovm.com/blog/htaccess-tips/)