# git-fix

A simple [git](https://git-scm.com/) command to open merge conflicts as errors in [Vim](https://www.vim.org/)'s [quickfix](https://vimhelp.org/quickfix.txt.html) list.

If you're looking for a way to do this from Vim instead of the git CLI, use a plugin like [fugitive](https://github.com/tpope/vim-fugitive).

## Installation

Using [Homebrew](https://brew.sh/):

```console
$ brew install ajvondrak/tap/git-fix
```

Alternatively, copy [bin/git-fix](bin/git-fix) somewhere on your [`PATH`](https://en.wikipedia.org/wiki/PATH_%28variable%29) and make sure to give it executable permissions:

```console
$ git clone git@github.com:ajvondrak/git-fix.git
$ cp git-fix/bin/git-fix /usr/local/bin/git-fix
$ chmod +x /usr/local/bin/git-fix
```

## Usage

When you have a merge conflict, git dumps you out into a staging area where all the unstaged files have conflict markers that need to be resolved.

```console
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   file

no changes added to commit (use "git add" and/or "git commit -a")
```

From this point, just run

```console
$ git fix
```
Under the hood, this will generate a list of messages in the [error format](https://vimhelp.org/quickfix.txt.html#errorformat) `%f:%l:%m` for each `<<<<<<<` marker found in the unstaged files. This list is passed into the [`GIT_EDITOR`](https://git-scm.com/docs/git-var#Documentation/git-var.txt-GITEDITOR) (presumably something Vim-like) with the `-q` flag, as well as any other flags you specified on the command line.

For example, if your `GIT_EDITOR` is the terminal Vim, you might open the quickfix items in GUI Vim with:

```console
$ git fix -g
```

At any rate, you can then hop between all the merge conflicts in Vim with commands like [`:cn`](https://vimhelp.org/quickfix.txt.html#%3Acnext) and [`:cp`](https://vimhelp.org/quickfix.txt.html#%3Acprevious).
