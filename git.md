
GIT - A quick reminder
======================


Useful commands
---------------

List all the commits and the affected files:

```bash
git log --name-status
```

List all commits and their hashtag:

```bash
git rev-list --all --pretty=oneline
```

Delete from the local repository all files that were locally deleted without using the `git rm` command:

```bash
git ls-files -d -z | xargs -0 git update-index --remove
```

Set the local `<branch>` to track the `<remote/branch>`:

```bash
git branch -u <remote/branch> <branch>
```

Set a different push URL for a remote repository:

```bash
git remote set-url --push <remote> <URL>
git remote -v  # to check the change has correctly been applied
```

Show the local branches tracking information:

```bash
git branch -vv
```

Update all submodules recursively, and init them if necessary:

```bash
git submodule update --init --recursive
```

How to…
-------

### Move commits to another branch

The goal is to do something like:

                                         A--B--E--F (master)
    A--B--C--D--E--F (master)      =>        \
                                              C--D (topic)

1.  Create the branch, ending at the desired commit:

```bash
git branch <topic> D
```

2.  (a) Rebase interactively `master`, starting after `B`:

```bash
git rebase -i B
```

2.  (b) Instead of an interactive rebase, it's possible to use the `--onto` option. In this example it makes the commit `B` take the place of `D` in `master`, skipping the in-between ones.

```bash
git rebase D --onto B
```


### Undo

To get a specific file as it was in a specific commit (eg. `HEAD`) and cancel its working dir changes:

```bash
git checkout <commit> <file.ext>
```

To restore the whole repository as it was in a spcific commit (eg. `HEAD^`):

```bash
git reset --hard <commit>
```

To do the same as previously, but keep the local changes and just reset the
index use `--soft` instead.

To see the history of the local repo (and the hash of a lost commit):

```bash
git reflog
```


### Use subtrees

To keep a clean history and revision graph **don't** use the command `git 
subtree`. 

#### Adding a subtree

```bash
git remote add plugin ../remotes/plugin
git fetch plugin
git read-tree --prefix=vendor/plugins/demo -u plugin/master
git commit -m "Added demo plugin subtree in vendor/plugins/demo"
```

#### Updating a subtree

```bash
git fetch plugin
git merge -s subtree --squash plugin/master
git commit -m "Updated the plugin"
```

  Source: [medium.com][subtree]


### Split subdirectory into separate Git repository

To filter out the subfolder from the rest of the files in the repository, run:

```bash
git filter-branch --prune-empty --subdirectory-filter <folder-name> <branch-name>
```

with:
-   `<folder-name>`: The folder within your project that you'd like to create a separate repository from.
-   `<branch-name>`: The default branch for your current project, for example,  `master`.

Source: [help.github.com][split]


### Updating (to rebase or not)

To update a local repo (the local commits will be merged, creating a merge commit):

```bash
git pull
```

      A--B--C (origin/master)            A--B--C--E (master)
          \                        =>        \   /
           D (master)                         -D-


To update a local repo, and prevent a merge commit (doing so, the rebased commits **must not** have been pushed to a public repository; if they have the subsequent pushes must be done withe the `--force` flag):

```bash
git pull --rebase
```

      A--B--C (origin/master)            A--B--C (origin/master)
          \                        =>           \
           D (master)                            D' (master)


### Update a production repository from the bare after a push

Add the following in the `repo.git/hooks/post-update` file:

```bash
echo
echo "===== Pulling changes into live server ====="
echo

cd /var/www/gregseth.net
unset GIT_DIR
git pull origin master
```


### Remove file from history

Run the following command, replacing `<path/to/the/file>` with the  **path to the file you want to remove, not just its filename**. These arguments will:

-   Force Git to process, but not check out, the entire history of every branch and tag
-   Remove the specified file, as well as any empty commits generated as a result
-   **Overwrite your existing tags**

```bash
git filter-branch --force --index-filter \
   "git rm --cached --ignore-unmatch <path/to/the/file>" \
   --prune-empty --tag-name-filter cat -- --all
```

Source: [help.github.com][erase]


Configuration
-------------

Here's a basic configuation file:

```ini
[alias]
  ci = commit
  co = checkout
  df = diff
  rv = rev-list --all --pretty=oneline
  ls = ls-files
  ign = ls-files -o -i --exclude-standard
  hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
[status]
  submoduleSummary = true
[color]
  ui = true
[push]
  default = current
```

Ignoring files
--------------

Just add a `.gitignore` file at the root of the project folder.

    # Visual studio
    *.user
    *.suo
    *.ncb
    *.pdb
    *.idb
    *.aps
    Deleted

    # Eclipse
    .metadata
    .settings

    # Source safe
    *ssc

    # Temporary files
    tmp
    temp
    obj
    *.bak*
    *.out
    _*
    *.swp
    ~$*

    # Output files and binaries
    *.exe
    *.lib
    *.dll
    *.a
    *.so
    Exec
    out
    bin

    # Project specific


  [split]: https://help.github.com/en/articles/splitting-a-subfolder-out-into-a-new-repository
  [subtree]: https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec
  [erase]: https://help.github.com/en/articles/removing-sensitive-data-from-a-repository
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ1NTQzNTg2NF19
-->