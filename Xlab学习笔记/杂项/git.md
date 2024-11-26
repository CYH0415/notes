Test line.

# Git
版本控制
分布式
delta encoding （增量编码）
## A list
- .gitignore
- Syncing
- Pull request
- ssh
# General ideas

## SSH
已添加到github account
## Git configurations
1. Local
	  `/.git/config` – Repository-specific settings.
2. Global
      `/.gitconfig` – User-specific settings. This is where options set with the --global flag are stored.
3. System
      `$(prefix)/etc/gitconfig` – System-wide settings.
## Git three trees

Node and pointer-based data structures.

1. Working directory 工作目录
    

Use `git status` to show changes.

2. Staging index 暂存区
    

Tracking Working directory changes.

Use `git add` to promote.

3. Commit history
    
      Use `git commit` to add changes to a permanent snapshot in commit history tree.
    
      Also includes the state of Staging index tree.
    > 8351582 (HEAD -> dev, origin/dev) Add branch file
    > 7f9f25d (origin/master, master) Update test.txt
    > 89f104c add test.txt
      可见，包含了头指针指向的分支，远程仓库中对应的分支，提交对应的分支
    
## .gitignore
git sees files as three things:
1. Tracked
2. Untracked
3. Ignored
## Pointer
1. HEAD pointer
指向当前工作目录状态
2. Branch pointer
指向每个分支的最新提交
仅在commit新提交时改变，并且总是指向这个commit
## Fast forward
合并的过程没有conflict冲突，可以直接移动两个分支的指针来合并
这种合并会删除无用的分支
# Set up repository
Git resposity: virtual storage.
## `git init`

```Plain
git init <directory>
```

- 创建名为`<directory>`的子目录，留空则在当前路径直接创建 .git
- Will not override
## `git clone`

```Plain
git clone <repo url>
```

- SSH: git@HOSTNAME:USERNAME/REPONAME.git
    

# Saving changes

## `git add`

```Plain
git add <file/directory>
git add -p 
```

Add pending changes to `git staging` area

- -p to stage all changes
    

## git commit

```Plain
git commit
git commit -a
```

- 将 `add` 命令暂存的更改提交到本地存储库
    
- 记录文件全部内容而非差异
    
- -a 提交所有更改的快照
    
- 打开文本编辑器编辑。规范格式：
    

> 总结
> 
> // 空行
> 
> 详细解释


```Bash
git commit -m "message"
git commit -am "message"
```


- 跳过文本编辑器直接创建提交
    
- -am 即 -a 与 -m
    

```Bash
git commit -amend
```

- 合并至上一次提交而非创建新提交
    

## git diff

```Plain
git diff
```

- Changes
    

  

# Syncing

hmm

## git remote

Create, view, delete connections to other repo.

Convenient names as references to URLs.

- 当clone一个仓库时，自动创建一个origin链接指向原仓库。
    

```Plain
git remote
git remote -v
```

Show all connections:

> origin https://github.com/CYH0415/Test-Repo.git (fetch)
> 
> origin https://github.com/CYH0415/Test-Repo.git (push)

```Plain
git remote add <name> <url>
```

将一个远程仓库添加到本地配置，并为其添加一个别名（保存在.git/config）

```Plain
git remote rm <name>
git rename <old> <new>
```

显而易见的。

```Plain
git remote get-url [--push] [--add]
```

- --push: show push URLs rather than fetch URLs
    
- --all: all.
    

```Plain
git remote show <name>
```

详细信息

## git fetch

Download commits, files and refs from a remote repo.

隔离操作，对本地工作没有任何影响

```Plain
git fetch <remote>
git fetch <remote> <branch>
git fetch --all
```

## git pull

A combination of `git fetch` and `git merge`.

```Plain
git pull <remote>
```

Fetch the remote copy and merge it into the local copy.

## git push

Upload local repo to a remote repo.

```Plain
git push <remote> <branch>
```

将所选分支上传到`<remote>`对应的远程仓库，如果文件冲突便不会执行

```Plain
git push <remote> --force
```

强制执行

```Plain
git push <remote> --tags
```

带标签的操作

  

  

  

  

  

# Inspecting

## git status

```Plain
git status
```

- Displays: changes staged/ not staged, files not tracked
    
- No info abt committed project history
    

1. Clean:
    

> ```Plain
> On branch master
> nothing to commit, working tree clean
> ```

2. Changes not staged
    

```Bash
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   test.txt
        
no changes added to commit (use "git add" and/or "git commit -a")
```

3. Changes staged / to be committed
    

```Bash
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   test.txt
```

4. Files not tracked
    

```Bash
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        not tracked.txt

nothing added to commit but untracked files present (use "git add" to track)
```

## git log

按Q退出log模式

```Bash
git log
```

- Committed history
    

> commit e526b7afbda266c17c6d4fd41cf2a814db1ddcf3 (HEAD -> master) Author: CYH <cyh050415@gmail.com> Date: Sun Mar 31 12:09:39 2024 +0800 add modified commit 3219a22910e39000148236188dd49a0d74c97671 Author: CYH <cyh050415@gmail.com> Date: Sun Mar 31 12:08:12 2024 +0800 add new file commit 919a74324c862d5a0f4807b65c8be7c9361337da Author: CYH <cyh050415@gmail.com> Date: Sat Oct 28 15:54:34 2023 +0800 commit_1 commit 4fedfad7994defa636016d9fc952fd3171fa288c Author: CYH <cyh050415@gmail.com> Date: Sat Oct 28 15:29:45 2023 +0800 'initial_version'

```Bash
git log --oneline
```

一个例子，注意新提交在上：

> e526b7a (HEAD -> master) add modified 3219a22 add new file 919a743 commit_1 4fedfad 'initial_version'

另一个例子，新分支有关

> 8351582 (HEAD -> dev, origin/dev) Add branch file
> 
> 7f9f25d (origin/master, master) Update test.txt
> 
> 89f104c add test.txt

- Displays each commit in one line
    

```Bash
git log -n <limit>
```

- Displays limited lines
    

```Bash
git log -p
```

- Displays full diff （类似为每个commit运行一次git diff）
    

```Bash
git log --author="<pattern>"
git log --grep="<pattern>"
git log <file>
```

- Search commits for a particular author / part of commit message / file name.
    

# Undoing

Git: timeline management

Commits: snapshots

Branches: multiple timelines

```Bash
git log --branches=* //show all branches
```

  

## git revert

提交一个新的提交以及reverse的内容，以防丢失历史记录

```Bash
git revert HEAD
```

Revert the last commit in HEAD by adding a new commit.

```Bash
git revert <commit-hash>
```

Revert `<commit-hash>` commit by adding a new commit.

例子：移除了添加到“test.txt”文件中的文本

> commit 69387e465249ca299be658faf0a788caf70dee90
> 
> Author: CYH [cyh050415@gmail.com](mailto:cyh050415@gmail.com)
> 
> Date: Sun Mar 31 14:47:00 2024 +0800
> 
>   
> 
> Revert "add modified"
> 
>   
> 
> This reverts commit e526b7afbda266c17c6d4fd41cf2a814db1ddcf3.
> 
>   
> 
> diff --git a/test.txt b/test.txt
> 
> index bedffab..e69de29 100644
> 
> --- a/test.txt
> 
> +++ b/test.txt
> 
> @@ -1,2 +0,0 @@
> 
> -666
> 
> -999
> 
> \ No newline at end of file
> 
>   
> 
> commit e526b7afbda266c17c6d4fd41cf2a814db1ddcf3
> 
> Author: CYH [cyh050415@gmail.com](mailto:cyh050415@gmail.com)
> 
> Date: Sun Mar 31 12:09:39 2024 +0800
> 
>   
> 
> add modified
> 
>   
> 
> diff --git a/test.txt b/test.txt
> 
> index e69de29..bedffab 100644
> 
> --- a/test.txt
> 
> +++ b/test.txt
> 
> @@ -0,0 +1,2 @@
> 
> +666
> 
> +999
> 
> \ No newline at end of file

可见，创建了一个新的提交，其中，“add modified”之提交中添加的文本被删掉了。

```Bash
-e
--edit
```

Default option, open editor.

```Bash
--no-edit
```

Don't open the editor.

```Bash
-n
--no-commit
```

Don't create commit, but add inverse changes to staging index.

  

  

## git reset

类似于git checkout，但checkout仅移动HEAD，保持分支指针不变；reset将头指针与分支指针一起移动。

势必造成commit tree的改变，根据所选参数，可能改变working directory与staging index。

![](https://xn4zlkzg4p.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWRkZmRlZmFmNTNmYmI2OGI1YWIyZDY3NzlhYzk1ZTVfWW4yZG9lbDV5NkJpYjVNMHpGSnA5U0tKeTdCYzNLc0xfVG9rZW46SU9zZmJWeHQ0b3E5OE54bTQxT2NMbTBPbmwxXzE3MTI3NDk3Njk6MTcxMjc1MzM2OV9WNA)

```Bash
git reset
git reset --mixed HEAD
```

等价物，混合撤销latest commit

```Bash
git reset <commit-hash>
```

撤销特定的提交，此后的提交会变为遗弃状态，或通过hash找回，或被清理

```Bash
git reset <file name>
```

Remove specified file from Staging index, leave working directory unchanged.

### --soft

仅移动HEAD指针至该提交处

暂存区与工作目录不发生变化，改变Commit history

### --mixed

更新HEAD指针

重置暂存区至此提交前的状态

Staging index中修改的文件会被标记为not staged；跟踪的新文件会被标记为untracked

### --hard

Working directory and Staging index will be reset to match that of the specified commit.

文件修改会丢失

新文件会被清除

## git rm

Remove files from Staging index and working directory, or both, but not only working directory.

Inverse of git add.

移除的文件必须与HEAD中的文件一致，如果HEAD中的文件与暂存区不一致，则不会移除。

需要提交至commit tree

```Bash
-f
--force
```

不进行与HEAD中的文件比对

```Bash
-n
--dry-run
```

不会实际删除文件

```Bash
-r
```

Remove a target directory and all files in it.

```Bash
--cached
```

仅移除暂存区的文件，不改变工作目录

```Bash
-q
-quiet
```

Hides the output.

### Undo git reset

```Bash
git reset HEAD
git checkout .
```

# Using branches

## git branch

```Bash
git branch
git branch -a
```

Give a list of all branches.

```Bash
git branch <branch>
```

Create a new branch, but don't checkout.

```Bash
git branch -d <branch>
git branch -D <branch>
```

Delete branch. But prevent it (-D to force delete) from deleting unmerged changes.

```Bash
git branch -m <branch>
```

Rename current branch.

## git checkout

Switch HEAD pointer

```Bash
git checkout <commit-hash> // e.g. 4fedfad
```

- HEAD is now at 4fedfad 'initial_version'
    
- 'detached HEAD' state, none of changes will be saved in repository（分离头指针）
    
- 仅用于查看历史版本，而非修改
    

```Bash
git checkout master
```

- Switch back to 'master'
    
- Exit 'detached HEAD' state
    

```Bash
git checkout <branch>
```

- 切换到`<branch>`分支的最新提交节点
    

```Bash
git checkout -b <branch>
```

从HEAD创建新分支并切换至其。

```Bash
git checkout -b <new-branch> <existing-branch>
```

从`<existing-branch>`创建新分支，并切换。

  

  

```Bash
git checkout --filename
```

- 撤销对文件的修改并使其恢复到最近提交的状态
    

## git merge

合并分支

1. 切换到接受合并的分支
    

# Making a pull request

## Feature branch workflow

Bitbucket

Only one public repo

Open a pull request before integrating.

Merge feature branch to main branch

  

## Gitflow workflow

主分支用于发布稳定的版本

开发分支用于集成新功能