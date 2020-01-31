### `git directories: working && staging(index) directories`

When you `git clone` a project from a specific source like `GitHub`, the local copy on your local machine will be called a `Local Repository`. The `Working Directory` is the directory of files with which you're currently working and are called `un-tracked` (in the beggining the `Working Directory` is the same as the `Local Repository`). After adding files with `git add .` you're adding these files to the `Staging Directory`. And later, when one wants the `staging` changes to be part of the `Local Repository`, you do `git commit `. After this whole process, all your made changes will be in the `Local Repository` and in order to push to `Remote` only `git push` is needed.

```
    Local  Repository -> your computer
    Working Directory -> untracked
    Staging Directory -> staged
    Remote Repository -> GitHub / GitLab
```

### `remotes` 

A `remote` is some repository on the internet, which hosts and stores your `git` project, f.e: `GitHub`. By default, when you create a new repository in `GitHub` you have add this `remote` to your project. Which is done with:

```
  git remote add origin <remote-url>
```

Have in mind the `origin` name. This is the default naming for your default remote repository. Meaning that when you operate on some of the commands like `git fetch`, it is really `git fetch origin`. A repository can have multiple remotes and they can be named differently, f.e: you can host your project in `GitHub` and in `BitBucket`. In this situation you would you have a different remote like `bitbucket` and be able to push to only that specific `remote` (`git push bitbucket master`)


### `git fetch vs git pull`

A `git fetch` will download the changes from your remote repositories to your local machine, but won't merge with your current branch. For example, if you're on `feature/get-users` and have made some changes, after a `git fetch` you can see something like:

```
remote: Enumerating objects: 345, done.
remote: Counting objects: 100% (345/345), done.
remote: Compressing objects: 100% (106/106), done.
remote: Total 345 (delta 163), reused 328 (delta 153), pack-reused 0
Receiving objects: 100% (345/345), 243.51 KiB | 13.53 MiB/s, done.
Resolving deltas: 100% (163/163), completed with 104 local objects.
From github.com:username/repository
   2684e66f8a6..da0136e3cee  master                     -> origin/master
 * [new branch]              feature/get-members        -> origin/get-members
```

This means that these branches have been updated and the changes have been downloaded, but not merged with your current branch.

`git pull` does a `git fetch && git merge` which automatically downloads the remote changes and merges with your current actiive branch. Therefore, if you have some different changes than the remote. You will have to resolve `merge conflicts`. `git fetch` is recommended more because you can look up ahead what are the remote changes with `git diff origin <branch-name>`.


### `git checkout`

`git checkout` besides allowing to checkout a specific branch, also allows to discard changes in the `working directory`. For example, after some changes in a file `index.rs` and doing `git status` you'll see a message:

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  	modified:   index.rs
```

which indicates that your changes are in the `working directory`. After `git checkout index.rs` the unstagged changes will be discarded. To remove all unstagged changes one can do `git checkout .`

> Have in mind that in GIT 2.23.0 a command called `git restore` was introduced. Which also allows to remove unstagged changes from the `working directory`, to remove from staging a flag `--staged` is needed.


### `git reset`


`git reset` has three main different flags: 
```
--soft,
--mixed (default),
--hard
```

`git reset --soft <commit-hash>` would return you to the commit with the specified commit hash and put all those branch changes to the `staging directory`. 

`git reset --mixed <commit-hash> (short: git reset <commit-hash>)` would return you to the commit with the specified commit hash and put all those changes to the `working directory`. If you would run this on your local branch `git reset`, all  the changes from `staging directory` will be moved `working directory`.

`git reset --hard <commit-hash>` would return to the commit of the specified hash and all that commit changes would be discarded. On a local branch `git reset --hard` removes all the files from `staging directory` and `working directory`.


### `git revert`

This command is for remote repository changes. For example, if you have pushed all your `Local Directory` changes to remote an want to undo without removing history. `git revert <commit-hash>` would create a new commit showing the removed changes on top the history. Thus not destroying any history.


### `git clean` 

Whereas `git checkout` and  `git reset` mainly worked on files in the `staging directory` (except `git reset --hard` and `git checkout .`). `git clean` is only for untracked files in your `working directory` to change. This command is reponsible for removing files from the `working directory`.
By default it requires the `-f (force)` flag to execute.

The main flags are:

`-f (force)`. Have in mind that by default it will removes files but not directories and files within them.

`-d (directories)`. This will delete all directories and files within those directories.

`-n (dry run)`. Shows files which would be removed.

`-i (interactive)`. This command will show the changes that will be executed and will allow to tell patterns by which files can be deleted. Starting an interactive shell.

All of these flags can be combined, for example if you want to delete all directories & files and check the changes before executing, you can do `git clean -fdn`.