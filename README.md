# My dotfiles

## Installation

### Using Git and the bootstrap script

Clone the repository in a location of choice (for instance in `~/Configuration/dotfiles`, with `~/dotfiles` as a symlink). The bootstrapper script will pull in the latest version and copy the files to the home folder.

```bash
git clone https://github.com/sanderdewitte/dotfiles.git && cd dotfiles && source bootstrap.sh
```

To update, `cd` into the local `dotfiles` git repository, then:

```bash
source bootstrap.sh
```

Alternatively, to update while avoiding the confirmation prompt:

```bash
set -- -f; source bootstrap.sh
```

### Update the `$PATH`

If `~/.path` exists, it will be sourced along with the other files, before any feature testing takes place.

Here’s an example `~/.path` file that adds `/usr/local/bin` to the `$PATH`:

```bash
export PATH="/usr/local/bin:$PATH"
```

### Define custom commands

If `~/.extra` exists, it will be sourced along with the other files. It can be used to add custom commands that are not supposed to be committed to a public repository.

An example `~/.extra` file looks like this:

```bash
# Git credentials
GIT_AUTHOR_NAME="<full_name>"
GIT_COMMITTER_NAME="$GIT_AUTHOR_NAME"
git config --global user.name "$GIT_AUTHOR_NAME"
GIT_AUTHOR_EMAIL="<email_address>"
GIT_COMMITTER_EMAIL="$GIT_AUTHOR_EMAIL"
git config --global user.email "$GIT_AUTHOR_EMAIL"
```

One could also use `~/.extra` to override settings, functions and aliases from this dotfiles repository.