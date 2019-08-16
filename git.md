
GIT - A quick reminder
======================


Create a new remote bare repo and set it as origin for the local one
--------------------------------------------------------------------

  First, on the remote server, create a fresh bare repo.

```bash
git init --bare
```

  Then, on the local machine, add a remote to the local repository.

```bash
git remote add origin [user@server:]/var/git/the_project_name.git
```

  This, by itself, does not do exacly much except to add a remote repository to your local repo config. The remote repo is called origin, which is the default name git chooses if you git clone from a remote repo. The remote repo is not associated with any local branches yet.

  Second, push the accumulated local commits to the remote repo, designating the remote as the default for future push/pull operations.

```bash
git push --set-upstream origin master
```

  This will push the local master branch to the remote origin, creating a master branch there as well, and ties origin to the local master branch as the default for push and pull. Future git pull and git push will work without any specifications of local or remote branches.

  Once done, to change the location of the remote repository use:

```bash
git remote set-url origin [user@server:]/var/git/new_project_name.git
```


Useful commands
---------------

  List all the commits and the affected files:

```bash
    $ git log --name-status

  List all commits and their hashtag:

    $ git rev-list --all --pretty=oneline

  Delete from the local repository all files that were locally deleted without using the `git rm` command:

    $ git ls-files -d -z | xargs -0 git update-index --remove

  Set the local `<branch>` to track the `<remote/branch>`:

    $ git branch -u <remote/branch> <branch>

  Set a different push URL for a remote repository:

    $ git remote set-url --push <remote> <URL>
    $ git remote -v  # to check the change has correctly been applied

  Show the local branches tracking information:

    $ git branch -vv

  Cloning syntax:

```bash
git clone ssh://<user>:<password>@<server>:<port>/<path>
git clone <user>@<server>:<path>
```

Init and pull a submodule:

```bash
git submodule update --init --recursive
```

Configuration
-------------

  Colors:

    $ git config --global color.ui true


How to…
-------

### Move commits to another branch

  The trick is to create the branch, and rebase `master`.

  If the aim is to do something like:

                                         A--B--E--F (master)
    A--B--C--D--E--F (master)      =>        \
                                              C--D (topic)

  First create the branch, ending at the desired commit:

    $ git branch <topic> D

  Then rebase `master`, starting after `B`:

    $ git rebase -i B

  Instead of an interactive rebase, it's possible to use the `--onto` option.
In this example it makes the commit `B` take the place of `D`, skipping the
in-between ones.

    $ git rebase D --onto B


### Undo

  To get a specific file as it was in a specific commit (eg. `HEAD`) and cancel
its working dir changes:

    $ git checkout <commit> <file.ext>

  To restore the whole repository as it was in a spcific commit (eg. `HEAD^`):

    $ git reset --hard <commit>

  To do the same as previously, but keep the local changes and just reset the
index use `--soft` instead.

  To see the history of the local repo (and the hash of a lost commit):

    $ git reflog


### Use subtrees

  To keep a clean history and revision graph **don't** use the command `git 
subtree`. 

#### Adding a subtree

    $ git remote add plugin ../remotes/plugin
    $ git fetch plugin
    $ git read-tree --prefix=vendor/plugins/demo -u plugin/master
    $ git commit -m "Added demo plugin subtree in vendor/plugins/demo"

#### Updating a subtree

    $ git fetch plugin
    $ git merge -s subtree --squash plugin/master
    $ git commit -m "Updated the plugin"

  Source: [medium.com][subtree]


### Detach subdirectory into separate Git repository

  For cases when a subdirectory of a repo needs to be made a standalone
repository.

  Prepare the old repo

    $ pushd <big-repo>
    $ git subtree split -P <name-of-folder> -b <name-of-new-branch>
    $ popd

  _Note:_ <name-of-folder> must NOT contain leading or trailing characters btoa != ./btoa/

  Create the new repo

    $ mkdir <new-repo>
    $ pushd <new-repo>

    $ git init
    $ git pull </path/to/big-repo> <name-of-new-branch>

  Link the new repo to Github or wherever

    $ git remote add origin <git@github.com:my-user/new-repo.git>
    $ git push -u origin master

  Cleanup, if desired

    $ popd # get out of <new-repo>
    $ pushd <big-repo>
    $ git rm -rf <name-of-folder>

  Source: [stackoverflow.com][detach]


### Updating

  To update a local repo (the local commits will be merged, creating a merge
commit):

    $ git pull

      A--B--C (origin/master)            A--B--C--E (master)
          \                        =>        \   /
           D (master)                         -D-


  To update a local repo, and prevent a merge commit (doing so, the rebased
commits **must not** have been pushed to a public repository; if they have the
subsequent pushes must be done withe the `--force` flag):

    $ git pull --rebase

      A--B--C (origin/master)            A--B--C (origin/master)
          \                        =>           \
           D (master)                            D' (master)


### Update a production repository from the bare after a push

  Add the following in the `repo.git/hooks/post-update` file:

    echo
    echo "===== Pulling changes into live server ====="
    echo

    cd /var/www/gregseth.net
    unset GIT_DIR
    git pull origin master


Configuration
-------------

Here's a basic configuation file:

    [alias]
      ci = commit
      co = checkout
      df = diff
      rv = rev-list --all --pretty=oneline
      ls = ls-files
      ign = ls-files -o -i --exclude-standard
      hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
    [color]
      ui = true
    [push]
      default = current


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




  [detach]: http://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository
  [subtree]: https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc4MDUyMTg5MiwtMTU0NzEzMTExOF19
-->