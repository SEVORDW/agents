# agents

bunny-approved agent workflows

## git worktrees

ai agents really don't like files changing from under them as they carry out their carefully prepared plan. naturally, you want to isolate them, so they work in separate directories and don't touch each other's changes.

git has a [`git worktree`](https://git-scm.com/docs/git-worktree) subcommand that allows checking out a branch into a separate directory. unfortunately, its ux is not very good, so most likely you will need a wrapper. i will list a few good ones that i use.

the workflow is to create a worktree, make some commits, then either discard it or create a pull request. for this i use `gh pr create` or simply ask `claude`. when it's merged, you can discard the worktree and the prune the merged branch.

### [git wt](https://github.com/k1LoW/git-wt)

####  install

`brew install k1LoW/tap/git-wt`

#### config

my preference is to put worktrees under `.worktrees` in the repo. don't forget to add it to `~/.gitignore_global`. then configue the path in the extension `git config wt.basedir .worktrees`

#### use

`git wt` lists all worktrees

`git wt feat/branch` switch to a worktree, creates the branch if needed

`git wt -d/D feat/branch` soft/hard delete a worktree along with the branch

### [worktrunk](https://worktrunk.dev/)

if you want a more advanced tool, try this one. im considering switching to it as it almost exactly matches my workflow. it has some good features like automatically running install scripts or generating commits with [llm](https://llm.datasette.io/en/stable/) cli.

#### config

i put this into `~/.config/worktrunk/config.toml` to match my preferred naming structure.

```toml
worktree-path = ".worktrees/{{ branch }}"
```

#### use

`wt switch -c -x codex feat/branch` switch to a worktree and run codex

`wt merge` squash, rebase, merge into master, remove the worktree and the branch

`wt step commit` commit your work based on the diff and previous commits style

`wt remove` remove worktree and prune the branch

`wt select` interactive switcher that shows all worktrees and the diff from master


## notifications

i use [this script](codex/notify_telegram/readme.md) to send codex notifications to telegram at the end of turn
