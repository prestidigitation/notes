## Fix commit authorship issues
When using `git` on a new computer, it's easy to forget to set the author name and email.

```zsh
git config --global user.name "Walter Schmidt"
git config --global user.email "walter@email.com"
```

If you accidentally made a commit with the wrong author name and email:
```zsh
# Change the author of the latest commit:
git commit --amend --reset-author --no-edit

# Force push changes without overwriting other authors' commits:
git push --force-with-lease
```
*this also changes the author timestamp.

```zsh
# Change the author for the last N commits:
git rebase --onto HEAD~N --exec "git commit --amend --reset-author --no-edit" HEAD~N
```

### Troubleshooting
Check to make sure that local repository settings aren't overriding global ones:
```zsh
git config user.name
git config user.email
```

Unset local variables for user name and email:
```zsh
git config --local --unset user.name
git config --local --unset user.email
```

## Prefer `switch` and `restore` over `checkout`
`checkout` and `restore` were both introduced in Git version 2.23 on August 2019. They offer a clearer separation of concerns than `checkout`, which is a Swiss Army knife that can both switch branches and restore files.

Here is a list of actions that `git checkout` handles:
* Switching branches
* Creating new branches
* Restoring files
* Checking out specific commits

`git switch`
```zsh
# Switch to existing branch
git switch feature-branch

# Create and switch to new branch
git switch -c feature-branch

# Long flag form of -c
git switch --create feature-branch
```

`git restore`
```zsh
# Restore a file to its last committed state:
git restore myfile.txt

# Restoring from a specific commit:
git restore --source==<commit_hash> <filename>
```

<!-- `git checkout`
```zsh
``` -->