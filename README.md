# git-demo

This is for git test !

## 基础操作

```bash
git init
git add
git commit
git push
git pull
git log
```

1. UserA：初始化项目并提交推送到Github
	```bash
	# 1. 初始化
	$ git init git-demo
	Initialized empty Git repository in /Users/cj/space/git-demo/.git/

	# 2. 添加一个README file
	$ cd git-demo
	$ vi README.md

	# 3. 提交
	$ git add README.md
	$ git commit -m "first commit"
	[master (root-commit) dbfaace] first commit
	 1 file changed, 3 insertions(+)
	 create mode 100644 README.md

	# 4. 推送到 Github
	$ git remote add origin git@github.com:sixDegree/git-demo.git
	$ git push -u origin master
	Counting objects: 3, done.
	Writing objects: 100% (3/3), 245 bytes | 0 bytes/s, done.
	Total 3 (delta 0), reused 0 (delta 0)
	remote:
	remote: Create a pull request for 'master' on GitHub by visiting:
	remote:      https://github.com/sixDegree/git-demo/pull/new/master
	remote:
	To git@github.com:sixDegree/git-demo.git
	 * [new branch]      master -> master
	Branch master set up to track remote branch master from origin.
	```

2. UserB：拉取项目，新建1.txt并推送到Github
	```bash
	# 1. 克隆项目
	$ git clone git@github.com:sixDegree/git-demo.git
	Cloning into 'git-demo'...
	remote: Enumerating objects: 3, done.
	remote: Counting objects: 100% (3/3), done.
	remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
	Receiving objects: 100% (3/3), done.
	Checking connectivity... done.
	$ tree
	.
	└── git-demo
	    └── README.md
	1 directory, 1 file

	# 2. 添加文件1.txt并提交
	$ cd git-demo
	$ vi 1.txt
	Hello
	$ git add 1.txt
	$ git commit -m "add 1.txt:hello"
	[master cbfd4db] add 1.txt:hello
	 1 file changed, 1 insertion(+)
	 create mode 100644 1.txt

	# 3. 推送到Github 
	$ git push
	Counting objects: 3, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 274 bytes | 0 bytes/s, done.
	Total 3 (delta 0), reused 0 (delta 0)
	To git@github.com:sixDegree/git-demo.git
	   dbfaace..cbfd4db  master -> master    
	```

3. UserA：新建文件1.txt,并推送到Github
	```bash
	# 1. 添加1.txt并提交
	$ vi 1.txt
	$ git add 1.txt
	$ git commit -m "add 1.txt:world"
	[master 145fb14] add 1.txt:world
	 1 file changed, 1 insertion(+)
	 create mode 100644 1.txt

	# 2. 推送到Github，报错，必需先git pull最新版代码
	$ git push
	To git@github.com:sixDegree/git-demo.git
	 ! [rejected]        master -> master (fetch first)
	error: failed to push some refs to 'git@github.com:sixDegree/git-demo.git'
	hint: Updates were rejected because the remote contains work that you do
	hint: not have locally. This is usually caused by another repository pushing
	hint: to the same ref. You may want to first integrate the remote changes
	hint: (e.g., 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.

	# 3. 拉取最新版代码，发现与本地代码有冲突，需要修正后重新提交
	$ git pull
	remote: Enumerating objects: 4, done.
	remote: Counting objects: 100% (4/4), done.
	remote: Compressing objects: 100% (2/2), done.
	remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
	Unpacking objects: 100% (3/3), done.
	From github.com:sixDegree/git-demo
	   dbfaace..cbfd4db  master     -> origin/master
	Auto-merging 1.txt
	CONFLICT (add/add): Merge conflict in 1.txt
	Automatic merge failed; fix conflicts and then commit the result.
	$ git status
	On branch master
	Your branch and 'origin/master' have diverged,
	and have 1 and 1 different commit each, respectively.
	  (use "git pull" to merge the remote branch into yours)
	You have unmerged paths.
	  (fix conflicts and run "git commit")
	Unmerged paths:
	  (use "git add <file>..." to mark resolution)
		both added:      1.txt
	no changes added to commit (use "git add" and/or "git commit -a")

	# 4. 修正冲突文件，重新提交推送
	$ cat 1.txt
	<<<<<<< HEAD
	World
	=======
	Hello
	>>>>>>> cbfd4db28b321c82bc98accca2fb373e94e9c59f
	$ vi 1.txt
	Hello World
	$ git add 1.txt
	$ git commit -m "fix 1.txt conflict: Hello World"
	[master 107efcd] fix 1.txt conflict: Hello World
	$ git push
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (4/4), done.
	Writing objects: 100% (6/6), 582 bytes | 0 bytes/s, done.
	Total 6 (delta 0), reused 0 (delta 0)
	To git@github.com:sixDegree/git-demo.git
	   cbfd4db..107efcd  master -> master
	```

4. UserA：查看提交记录 （与Github上一致）
	```bash
	$ git log
	commit 107efcd81db18ba6675adc4816660ab0f2ec974c (HEAD -> master, origin/master,origin/HEAD)
	Merge: 145fb14 cbfd4db
	Author: sixDegree <chenjin.zero@163.com>
	Date:   Tue Oct 30 23:13:26 2018 +0800

	    fix 1.txt conflict: Hello World

	commit 145fb14d464e6ab8059ff4cf9ec2b10bbfe26248
	Author: sixDegree <chenjin.zero@163.com>
	Date:   Tue Oct 30 23:04:47 2018 +0800

	    add 1.txt:world

	commit cbfd4db28b321c82bc98accca2fb373e94e9c59f
	Author: Tom <tom@example.com>
	Date:   Tue Oct 30 22:56:05 2018 +0800

	    add 1.txt:hello

	commit dbfaace57481f4f461d96b8966f466274432588b
	Author: sixDegree <chenjin.zero@163.com>
	Date:   Tue Oct 30 22:24:12 2018 +0800

	    first commit
	```

## 撤销

### `checkout` : Workspace <- Stage

1. 在Workspace添加文件2.txt：AAA
  ```bash
  $ vi 2.txt
  AAA
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Untracked files:
    (use "git add <file>..." to include in what will be committed)

      2.txt

  nothing added to commit but untracked files present (use "git add" to track)
  ```

2. 添加2.txt到Stage(开始追踪该文件的变化)
  ```bash
  $ git add 2.txt
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

      new file:   2.txt
  ```
  
3. 在Workspace修改2.txt：AAABBB
  ```bash
  $ vi 2.txt
  AAABBB
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

      new file:   2.txt

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

      modified:   2.txt
  ```
  
4. 撤销刚才在Workspace中对2.txt的修改，回归到加入到Stage中的版本：AAA
  ```bash
  $ git checkout 2.txt
  $ cat 2.txt
  AAA
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

      new file:   2.txt
  ```

### `reset`: Workspace <- Repository

1. 提交2.txt:AAA
  ```bash
  $ git commit -m "add 2.txt:AAA"
  [master bd7a058] add 2.txt:AAA
   1 file changed, 1 insertion(+)
   create mode 100644 2.txt
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)
  nothing to commit, working tree clean
  ```

2. 提交2.txt: AAABBB
  ```bash
	$ vi 2.txt
	AAABBB
	$ git commit -a -m "update 2.txt:AAABBB"
	[master 2af07d4] update 2.txt:AAABBB
	 1 file changed, 1 insertion(+), 1 deletion(-)
  ```
  
3. 查看提交记录
  ```bash
	$ git log
	commit 2af07d4122e2d38c249234d7ff20dde823be26cd (HEAD -> master)
	Author: chenjin.zero <chenjin.zero@163.com>
	Date:   Wed Oct 31 13:35:50 2018 +0800

		update 2.txt:AAABBB

	commit bd7a0584c72842ec6f38e5f60559e29a6c3b3e5c
	Author: chenjin.zero <chenjin.zero@163.com>
	Date:   Wed Oct 31 13:20:39 2018 +0800

		add 2.txt:AAA

	commit 107efcd81db18ba6675adc4816660ab0f2ec974c (origin/master, origin/HEAD)
	Merge: 145fb14 cbfd4db
	Author: sixDegree <chenjin.zero@163.com>
	Date:   Tue Oct 30 23:13:26 2018 +0800

		fix 1.txt conflict: Hello World
	...
  ```
  
4. 在Workspace中修改2.txt：AAACCC，并添加到Stage
  ```bash
	$ vi 2.txt
	AAACCC
	$ git add 2.txt
	$ git status
	On branch master
	Your branch is ahead of 'origin/master' by 1 commit.
	  (use "git push" to publish your local commits)

	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

			modified:   2.txt
  ```
  
5. 撤销刚才在Workspace中对2.txt的修改，回归到commit版本：AAA
  ```bash
  $ git reset --hard bd7a0584c7
  HEAD is now at bd7a058 add 2.txt:AAA
  $ cat 2.txt
  AAA
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)

  nothing to commit, working tree clean
  ```
  
6. 查看提交日志，发现最新日志回截取到刚才reset的版本bd7a0584c7
  ```bash
  $ git log
  commit bd7a0584c72842ec6f38e5f60559e29a6c3b3e5c (HEAD -> master)
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:20:39 2018 +0800

    add 2.txt:AAA

  commit 107efcd81db18ba6675adc4816660ab0f2ec974c (origin/master, origin/HEAD)
  Merge: 145fb14 cbfd4db
  Author: sixDegree <chenjin.zero@163.com>
  Date:   Tue Oct 30 23:13:26 2018 +0800

    fix 1.txt conflict: Hello World
  ...
  $ git reflog
  bd7a058 (HEAD -> master) HEAD@{0}: reset: moving to bd7a0584c7
  2af07d4 HEAD@{1}: commit: update 2.txt:AAABBB
  ...
  ```

###  `revert`：Workspace <- Repository

1. 提交2个版本的3.txt
  ```bash
  $ vi 3.txt
  123
  $ git add 3.txt
  $ git commit -m "add 3.txt:123"
  $ vi 3.txt
  123456
  $ git commit -a -m "update 3.txt:123-456"
  $ git log
  commit bb8e3df5cb459ccfc38afb76a4437c9e83299a40 (HEAD -> master)
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:53:19 2018 +0800

    update 3.txt:123-456

  commit e71cfa8e23e4bbbdbd6512a629e51c67d00d76ca
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:52:32 2018 +0800

    add 3.txt:123

  commit bd7a0584c72842ec6f38e5f60559e29a6c3b3e5c
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:20:39 2018 +0800

    add 2.txt:AAA

  commit 107efcd81db18ba6675adc4816660ab0f2ec974c (origin/master, origin/HEAD)
  Merge: 145fb14 cbfd4db
  Author: sixDegree <chenjin.zero@163.com>
  Date:   Tue Oct 30 23:13:26 2018 +0800

    fix 1.txt conflict: Hello World
  ...
  ```
2. 在Workspace中修改3.txt：123-456-789，并添加到Stage
  ```bash
	$ vi 3.txt
	123-456-789
	$ git add 3.txt
  ```
  
3. 撤销刚才在Workspace中对3.txt的修改，回归到commit版本：123 (注：需使用123的下一个版本)
  ```bash
  $ git revert bb8e3df5cb
  error: your local changes would be overwritten by revert.
  hint: commit your changes or stash them to proceed.
  fatal: revert failed
  ```
  
4. 撤销报错，先提交改动再执行撤销
  ```bash
	$ git commit -m "update 3.txt:123-456-789"
	$ git log
	commit 9fb2e0323d73254c11e22e83c11cb97f663eb4bc (HEAD -> master)
	Author: chenjin.zero <chenjin.zero@163.com>
	Date:   Wed Oct 31 14:25:39 2018 +0800

		update 3.txt:123-456-789

	commit bb8e3df5cb459ccfc38afb76a4437c9e83299a40
	Author: chenjin.zero <chenjin.zero@163.com>
	Date:   Wed Oct 31 13:53:19 2018 +0800

		update 3.txt:123-456

	commit e71cfa8e23e4bbbdbd6512a629e51c67d00d76ca
	Author: chenjin.zero <chenjin.zero@163.com>
	Date:   Wed Oct 31 13:52:32 2018 +0800

		add 3.txt:123

	...
  ```
  
5. 执行撤销，手动解决冲突后再提交，完成撤销
  ```bash
  $ git revert bb8e3df5cb
  error: could not revert bb8e3df... update 3.txt:123-456
  hint: after resolving the conflicts, mark the corrected paths
  hint: with 'git add <paths>' or 'git rm <paths>'
  hint: and commit the result with 'git commit'
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 4 commits.
    (use "git push" to publish your local commits)

  You are currently reverting commit bb8e3df.
    (fix conflicts and run "git revert --continue")
    (use "git revert --abort" to cancel the revert operation)

  Unmerged paths:
    (use "git reset HEAD <file>..." to unstage)
    (use "git add <file>..." to mark resolution)

      both modified:   3.txt

  no changes added to commit (use "git add" and/or "git commit -a")
  $ cat 3.txt
  <<<<<<< HEAD
  123-456-789
  =======
  123
  >>>>>>> parent of bb8e3df... update 3.txt:123-456
  $ vi 3.txt
  123
  $ vi 3.txt
  123
  $ git commit -a
  [master ef62cbc] Revert "update 3.txt:123-456"
   1 file changed, 2 insertions(+), 1 deletion(-) 
  ```
  
6. 查看提交日志，发现多了一个revert commit版本
  ```bash
  $ git log
  commit ef62cbc98372fa657f0df0b9b428c216f986e159 (HEAD -> master)
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 14:27:27 2018 +0800
    Revert "update 3.txt:123-456"
    This reverts commit bb8e3df5cb459ccfc38afb76a4437c9e83299a40.

  commit 9fb2e0323d73254c11e22e83c11cb97f663eb4bc
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 14:25:39 2018 +0800

    update 3.txt:123-456-789

  commit bb8e3df5cb459ccfc38afb76a4437c9e83299a40
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:53:19 2018 +0800

    update 3.txt:123-456

  commit e71cfa8e23e4bbbdbd6512a629e51c67d00d76ca
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:52:32 2018 +0800

    add 3.txt:123

  commit bd7a0584c72842ec6f38e5f60559e29a6c3b3e5c
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 13:20:39 2018 +0800

    add 2.txt:AAA

  commit 107efcd81db18ba6675adc4816660ab0f2ec974c (origin/master, origin/HEAD)
  Merge: 145fb14 cbfd4db
  Author: sixDegree <chenjin.zero@163.com>
  Date:   Tue Oct 30 23:13:26 2018 +0800

    fix 1.txt conflict: Hello World
  ```

## 分支

下游代码同步到下游（eg： 主干master -> myfeature分支）

### `rebase`

1. 获取主干最新代码
  ```bash
  $ git checkout master
  $ git pull
  ```

2. 新建一个开发分支myfeature
  ```bash
  $ git checkout -b myfeature
  Switched to a new branch 'myfeature'
  $ git status
  On branch myfeature
  nothing to commit, working tree clean
  $ git branch -a
    master
  * myfeature
    remotes/origin/HEAD -> origin/master
    remotes/origin/master
  $ git log --pretty=oneline
  ef62cbc98372fa657f0df0b9b428c216f986e159 (HEAD -> myfeature, origin/master, origin/HEAD, master) Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  bb8e3df5cb459ccfc38afb76a4437c9e83299a40 update 3.txt:123-456
  e71cfa8e23e4bbbdbd6512a629e51c67d00d76ca add 3.txt:123
  bd7a0584c72842ec6f38e5f60559e29a6c3b3e5c add 2.txt:AAA
  107efcd81db18ba6675adc4816660ab0f2ec974c fix 1.txt conflict: Hello World
  145fb14d464e6ab8059ff4cf9ec2b10bbfe26248 add 1.txt:world
  cbfd4db28b321c82bc98accca2fb373e94e9c59f add 1.txt:hello
  dbfaace57481f4f461d96b8966f466274432588b first commit
  ```
  
3. 在myfeature提交3次
  ```bash
  $ vi 3.txt
  123-Hello
  $ vi 4.txt
  ABC
  $ git add .
  $ git commit -m "myfeature: 3.txt:123-Hello;4.txt:ABC"

  $ vi 3.txt
  123-Hello-World
  $ git commit -a -m "myfeature: 3.txt:123-Hello-World" 

  $ vi 5.txt
  KKK
  $ git add 5.txt
  $ git commit -m "myfeature: 5.txt:KKK"
  ```

4. 查看提交日志
  ```bash
  $ git log --pretty=oneline
  5b5cc3c69942a8c49a90144eba5298d0d88d94c0 (HEAD -> myfeature) myfeature: 5.txt:KKK
  31cf30725deba4e0d98eb9be737716c65d966023 myfeature: 3.txt:123-Hello-World
  ede2940bed06c49ffed14c75e5f6498fe0994ef3 myfeature: 3.txt:123-Hello;4.txt:ABC
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  ...
  ```

5. 切换回主干master，添加4.txt：EFG 并提交推送到Github
  ```bash
  $ git checkout master
  $ vi 4.txt
  EFG
  $ git add 4.txt
  $ git commit -m "add 4.txt:EFG"
  $ git log --pretty=oneline
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 (HEAD -> master, origin/master, origin/HEAD, myfix) add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  ...
  ```

6. 切换回分支myfeature
  ```bash
  6.1 同步主干master内容
  # 执行交互式rebase操作，合并myfeature分支的多个commit为一个commit，
  # 操作对象是那些自最后一次从origin仓库拉取或者向origin推送之后的所有提交
  $ git rebase -i origin/master
  Auto-merging 4.txt
  CONFLICT (add/add): Merge conflict in 4.txt
  error: could not apply ede2940... myfeature: 3.txt:123-Hello;4.txt:ABC

  Resolve all conflicts manually, mark them as resolved with
  "git add/rm <conflicted_files>", then run "git rebase --continue".
  You can instead skip this commit: run "git rebase --skip".
  To abort and get back to the state before "git rebase", run "git rebase --abort"
  .

  Could not apply ede2940... myfeature: 3.txt:123-Hello;4.txt:ABC

  6.2 解决冲突后提交
  $ cat 4.txt
  <<<<<<< HEAD
  EFG
  =======
  ABC
  >>>>>>> ede2940... myfeature: 3.txt:123-Hello;4.txt:ABC
  $ vi 4.txt
  ABC-EFG
  $ git add 4.txt
  $ git commit -m "fix 4.txt confilct:ABC-EFG"

  6.3 查看当前提交日志和branch
  $ git branch
  * (no branch, rebasing myfeature)
    master
    myfeature
  $ git log --pretty=oneline
  b397de6d83d394503f619959d9c23b0c3e7d3b2d (HEAD) fix 4.txt confilct:ABC-EFG
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 (origin/master, origin/HEAD, master) add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  ...

  6.4 继续未完成的rebase
  $ git rebase --continue
  [detached HEAD 1df1fcb] fix 4.txt confilct:ABC-EFG
   Date: Wed Oct 31 16:06:35 2018 +0800
   3 files changed, 3 insertions(+), 2 deletions(-)
   create mode 100644 5.txt
  Successfully rebased and updated refs/heads/myfeature.  

  6.5 查看当前提交日志和branch
  $ git branch
    master
  * myfeature

  $ git log
  commit 1df1fcb81a04b28657a69157def8e8bd204b6b40 (HEAD -> myfeature)
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 16:06:35 2018 +0800

      fix 4.txt confilct:ABC-EFG

      myfeature: 3.txt:123-Hello-World

      myfeature: 5.txt:KKK

  commit f3ef100d56c9501c5e7286a61accb9171f31f2a5 (origin/master, origin/HEAD, mas
  ter)
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 15:53:21 2018 +0800

      add 4.txt:EFG

  commit ef62cbc98372fa657f0df0b9b428c216f986e159
  Author: chenjin.zero <chenjin.zero@163.com>
  Date:   Wed Oct 31 14:27:27 2018 +0800

      Revert "update 3.txt:123-456"
  ...

  $ git log --graph --pretty=oneline --abbrev-commit
  * 1df1fcb (HEAD -> myfeature) fix 4.txt confilct:ABC-EFG
  * f3ef100 (origin/master, origin/HEAD, master) add 4.txt:EFG
  * ef62cbc Revert "update 3.txt:123-456"
  * 9fb2e03 update 3.txt:123-456-789
  * bb8e3df update 3.txt:123-456
  * e71cfa8 add 3.txt:123
  * bd7a058 add 2.txt:AAA
  *   107efcd fix 1.txt conflict: Hello World
  |\
  | * cbfd4db add 1.txt:hello
  * | 145fb14 add 1.txt:world
  |/
  * dbfaace first commit

  6.6 将分支myfeature推送到Github 
  $ git push origin myfeature
  Counting objects: 5, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (5/5), 474 bytes | 94.00 KiB/s, done.
  Total 5 (delta 0), reused 0 (delta 0)
  remote:
  remote: Create a pull request for 'myfeature' on GitHub by visiting:
  remote:      https://github.com/sixDegree/git-demo/pull/new/myfeature
  remote:
  To github.com:sixDegree/git-demo.git
   * [new branch]      myfeature -> myfeature

  $ git log --graph --pretty=oneline --abbrev-commit
  * 1df1fcb (HEAD -> myfeature, origin/myfeature) fix 4.txt confilct:ABC-EFG
  * f3ef100 (origin/master, origin/HEAD, master) add 4.txt:EFG
  * ef62cbc Revert "update 3.txt:123-456"
  * 9fb2e03 update 3.txt:123-456-789
  * bb8e3df update 3.txt:123-456
  * e71cfa8 add 3.txt:123
  * bd7a058 add 2.txt:AAA
  *   107efcd fix 1.txt conflict: Hello World
  |\
  | * cbfd4db add 1.txt:hello
  * | 145fb14 add 1.txt:world
  |/
  * dbfaace first commit
  ```

### `merge`

1. 获取主干最新代码
  ```bash
  $ git checkout master
  $ git pull
  ```
  
2. 新建一个分支myfix
  ```bash
  $ git checkout -b myfix
  Switched to a new branch 'myfix'
  $ git log --pretty=oneline
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 (HEAD -> myfix, origin/master, origin/HEAD, master) add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  ...
  ```
  
3. 在myfix提交2次
  ```bash
  $ vi 5.txt
  Dev
  $ git add 5.txt
  $ git commit -m "myfix: add 5.txt:Dev" 
  $ vi 6.txt
  Hello
  $ git add 6.txt
  $ git commit -m "myfix: add 6.txt:Hello"
  $ git log --pretty=oneline
  a76fafb9fc46fbc132b5d10c4a638c11c9b0be5a (HEAD -> myfix) myfix: add 6.txt:Test
  69c9a7867913ca5c103bfe6c6cd6679a5a9bb878 myfix: add 5.txt:Dev
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 (origin/master, origin/HEAD, master) add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  ...
  ```
  
4. 切换回主干master，添加5.txt：Test,并提交推送到Github
  ```bash
  $ vi 5.txt
  Test
  $ git add 5.txt
  $ git commit -m "add 5.txt:Test"
  $ git push
  $ git log --pretty=oneline
  38796b3126275a39d37cb9bc788af31303cd6a26 (HEAD -> master, origin/master, origin/HEAD) add 5.txt:Test
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  ...
  ```
  
5. 切换回分支myfix
  ```bash
  $ git merge master
  Auto-merging 5.txt
  CONFLICT (add/add): Merge conflict in 5.txt
  Automatic merge failed; fix conflicts and then commit the result.

  $ cat 5.txt
  <<<<<<< HEAD
  Dev
  =======
  Test
  >>>>>>> master
  $ vi 5.txt
  Test-Dev
  $ git add 5.txt
  $ git commit -m "fix 5.txt conflict: Test-Dev"

  $ git log --pretty=oneline
  cdfdf809068dfccaccee4229e1c9d242ade70f34 (HEAD -> myfix) fix 5.txt conflict: Test-Dev
  38796b3126275a39d37cb9bc788af31303cd6a26 (origin/master, origin/HEAD, master) add 5.txt:Test
  a76fafb9fc46fbc132b5d10c4a638c11c9b0be5a myfix: add 6.txt:Test
  69c9a7867913ca5c103bfe6c6cd6679a5a9bb878 myfix: add 5.txt:Dev
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789

  $ git log --graph --pretty=oneline --abbrev-commit
  *   cdfdf80 (HEAD -> myfix, origin/myfix) fix 5.txt conflict: Test-Dev
  |\
  | * 38796b3 (origin/master, origin/HEAD, master) add 5.txt:Test
  * | a76fafb myfix: add 6.txt:Test
  * | 69c9a78 myfix: add 5.txt:Dev
  |/
  * f3ef100 add 4.txt:EFG
  * ef62cbc Revert "update 3.txt:123-456"
  * 9fb2e03 update 3.txt:123-456-789
  * bb8e3df update 3.txt:123-456
  * e71cfa8 add 3.txt:123
  * bd7a058 add 2.txt:AAA
  *   107efcd fix 1.txt conflict: Hello World
  |\
  | * cbfd4db add 1.txt:hello
  * | 145fb14 add 1.txt:world
  |/
  * dbfaace first commit

  $ git checkout master
  $ git log --pretty=oneline
  D:\Space\git-demo>git log --pretty=oneline
  38796b3126275a39d37cb9bc788af31303cd6a26 (HEAD -> master, origin/master, origin/HEAD) add 5.txt:Test
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789
  ...

  $ git push origin myfix
  Counting objects: 9, done.
  Delta compression using up to 4 threads.
  Compressing objects: 100% (6/6), done.
  Writing objects: 100% (9/9), 743 bytes | 74.00 KiB/s, done.
  Total 9 (delta 3), reused 0 (delta 0)
  remote: Resolving deltas: 100% (3/3), completed with 1 local object.
  remote:
  remote: Create a pull request for 'myfix' on GitHub by visiting:
  remote:      https://github.com/sixDegree/git-demo/pull/new/myfix
  remote:
  To github.com:sixDegree/git-demo.git
   * [new branch]      myfix -> myfix

  $ git log --pretty=oneline
  38796b3126275a39d37cb9bc788af31303cd6a26 (HEAD -> master, origin/master, origin/HEAD) add 5.txt:Test
  f3ef100d56c9501c5e7286a61accb9171f31f2a5 add 4.txt:EFG
  ef62cbc98372fa657f0df0b9b428c216f986e159 Revert "update 3.txt:123-456"
  9fb2e0323d73254c11e22e83c11cb97f663eb4bc update 3.txt:123-456-789 
  ...

  ```
