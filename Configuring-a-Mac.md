## Homebrew

To get modern unix utilities on your mac, install [homebrew](https://brew.sh/), which is a command-line package manager for Mac OS.

As explained on the [homebrew](https://brew.sh/) webpage, you can:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then it's useful to install things like:

```
brew install texlive   # For all sorts of LaTeX and pdf manipulation stuff
brew install brew install MisterTea/et/et   # better than ssh, survives laptop sleep
brew install wget      # sometimes easier than curl
```

## iTerm2

For terminal connections, I like [iTerm2](https://iterm2.com/), which I use together with `et`.

For example, for keeping a session open to quadro, I use:

```
//opt/homebrew/bin/et quadro -j thevisible.net -c 'tmux new -As main'
```

See [Reconnecting](Reconnecting-Automatically) for more about `et` and `tmux`.
