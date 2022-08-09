## `thevisible.net` built-in websites

The easiest way to share data is to put it on a website.

If you make a (globally-readable) directory under your `/share/u/[username]/www`, then it will become visible at a website at `https://thevisible.net/u/[username]/`.  You don't have to literally copy things to this directory; you can symlink to anything else under `/share` to share it on your website (using `ln -s`).  Note that `thevisible.net` is running on a small computer, not your gpu computer, and it only has access to the files under `/share`, not the local files that are just on your computer.

Similarly, for projects, if you make a (globally-readable) directory under your `/share/projects/[projectname]/www`, then it will become visible at a website at `https://thevisible.net/p/[projectname]/`. 

It's an apache server.  If the default apache configuration isn't working for you, you can put an `.htaccess` in a directory to alter the settings.