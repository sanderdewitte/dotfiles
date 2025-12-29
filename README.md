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

Alternatively, by using the `addpath` and `cleanpath` functions from `.functions` to make sure there are no duplicate entries:

```bash
addpath "/usr/local/bin"
cleanpath 
```

### Define custom commands

If `~/.extra` exists, it will be sourced along with the other files. It can be used to add custom commands that are not supposed to be committed to a public repository.

An example `~/.extra` file looks like this:

```bash
# Git identity (environment)
export GIT_AUTHOR_NAME="<full_name>"
export GIT_COMMITTER_NAME="$GIT_AUTHOR_NAME"
export GIT_AUTHOR_EMAIL="<email_address>"
export GIT_COMMITTER_EMAIL="$GIT_AUTHOR_EMAIL"
```

Or, slightly more advanced:

```bash
# Git identity (environment)
export GIT_AUTHOR_NAME="<your_full_name>"
export GIT_COMMITTER_NAME="$GIT_AUTHOR_NAME"
export GIT_AUTHOR_EMAIL="<your_email_address>"
export GIT_COMMITTER_EMAIL="$GIT_AUTHOR_EMAIL"

# Git identity (global config)
_set_git_global_identity() {

  # Only do work if git exists
  command -v git >/dev/null 2>&1 || return 0

  # Only write if needed (avoids .gitconfig lock races + reduces churn)
  local cur_name cur_email
  cur_name="$(git config --global --get user.name 2>/dev/null || true)"
  cur_email="$(git config --global --get user.email 2>/dev/null || true)"

  # Set git global identity if needed
  [ "$cur_name"  = "$GIT_AUTHOR_NAME" ]  || git config --global user.name  "$GIT_AUTHOR_NAME"
  [ "$cur_email" = "$GIT_AUTHOR_EMAIL" ] || git config --global user.email "$GIT_AUTHOR_EMAIL"

}

if command -v flock >/dev/null 2>&1; then
  (
    flock 9
    _set_git_global_identity
  ) 9>"$HOME/.gitconfig.startup.lock"
else
  _set_git_global_identity
fi

unset -f _set_git_global_identity
```

One could also use `~/.extra` to override settings, functions and aliases from this dotfiles repository.