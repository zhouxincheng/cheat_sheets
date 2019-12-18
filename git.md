### 新建 git 账户:

1. git config --global user.name "zhouxincheng"
2. git config --global user.email "zhouxincheng@pku.edu.cn"

### 创建版本库:

1.  git init: 初始化本地版本库
2. git clone git@github.com:zhouxincheng/xxx.git

### 文件状态

1. unstaged: 文件没有添加到 git 管理中, 也就是说该文件无法被追踪
2. staged: 文件添加到管理中了, 可以被提交
3. stash: 文件被暂存在额外的空间, 等待被取回后操作. 业务情景: boss 让你先添加一个 feature, 将手头正在进行的工作进行 stash, 待 boss 交代的任务完成后进行恢复.
4. git status: 查看目录中文件的状态

### 添加文件管理

1. git add . # . 代表添加所有的unstage 文件
2. git add filename # 添加指定文件

### 提交修改

1. git commit -m "there is description about the this commit"
2. git commit -am "commit the modified file directory without add first"
3. ![第一个版本库 Repository](/Users/whatever/Work/git.assets/2-1-1.png)



### 记录修改

1. git log: 查看当前分支的 commit 记录

   1. git log --oneline # 简约的形式显示
   2. git log --graph # 图形状的方式显示(显示所有分支)

2. git diff

   1. 不加参数: 查看 unstaged 的部分和上一个 commit 的文件不同
   2. --cached: 查看 staged 的文件与上一个 commit 的异同
   3. HEAD: 查看 staged 和 unstaged 文件与上一个 commit 的异同

3. ```plain
   # 对比三种不同 diff 形式
   $ git diff HEAD # (-- filename //查看某个文件)
   @@ -1 +1,3 @@
   -a = 1  # 已 staged
   +a = 2  # 已 staged
   +b = 1  # 已 staged
   +c = b  # 还没 add 去 stage (unstaged)
   -----------------------
   $ git diff          # unstaged
   
   @@ -1,2 +1,3 @@
    a = 2  # 注: 前面没有 +
    b = 1  # 注: 前面没有 +
   +c = b  # 还没 add 去 stage (unstaged)
   -----------------------
   $ git diff --cached # staged
   
   @@ -1 +1,2 @@
   -a = 1  # 已 staged
   +a = 2  # 已 staged
   +b = 1  # 已 staged
   ```

### 回溯

#### commit 回溯

1. git commit --amend --no-edit ## "--no-edit": 修改已经 commit 的版本; 不编辑, 直接合并到上一个 commit
2. git reset "staged_file": 将 staged file 改成 unstaged 的状态
3. git reset  --hard HEAD^: 回到上一个 commit, ^的个数代表往回走几个 commit, 也可以表示成 HEAD~N
4. git reset --hard commit_id: HEAD 指针指向 commit-id 的那个 commit
5. git reflog: 记录你的每一次命令, 以便返回 reset 之前的commit
6. git reset HEAD file: 把暂存区的修改回退到工作区
7. git checkout -- file: 将工作区的内容丢弃,

#### 文件回溯

1. git checkout commit_id  --    the_file: 将 the_file 改成 commit_id 时的内容

### 分支管理

#### 分支

1. git branch: 查看分支, "*"指示当前HEAD 指向的分支
2. git branch dev: 创建 dev 分支
3. git checkout dev: 切换到 dev 分支
4. git checkout -b dev: 直接创建分支且切换
5. git merge dev: (切换到其他分支) 将 dev 分支合并到当前的分支下
   1. --no-ff: no fast forward, fast forward 指的是当前的合并不会留下合并信息
   2. -m "merge info": 记录下 merge 的信息, 将 merge 之后的 master 作为新的 commit
6. **尽量不要使用 rebase,进行分支合并**

#### 分支冲突

1. master 与所要合并的分支(比如 dev)存在代码上的不一致, 需要手动的修改并重新提交. 产生原因是因为, dev 分支之后 master 又进行了新的修改

#### 临时修改

1. git stash: 将手头的工作保存到stash空间, 当成缓存,转手去做其他事情
2. git stash list : 查看在 stash 中缓存有哪些内容
3. git stash pop: 提取这个缓存并继续工作



## 廖雪峰教程--情景笔记

#### 撤销修改

1. 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`, **其实是用版本库里的版本(或暂存区)替换工作区的版本**

2. 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file> `，就回到了场景1，第二步按场景1操作。

3. 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，`git reset --hard commit_id` (HEAD 表示当前版本, ^/~N表示之前的版本)，不过前提是没有推送到远程库。
4. 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
5. 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
6. 场景4: 从版本库中删除该文件, 先显示的删除某个文件, 然后再版本库中`git rm text.txt`
7. 场景5: 将本地仓库与远程仓库关联起来 `git remote add origin git@server-name:path/repo-name.git`
8. 场景6: 关联好仓库之后, 将本地的 master 分支推送到远程仓库的 master 上 `git push -u origin master` , -u 参数将本地 master 与 远程 master 进行绑定, 方便后序操作
9. 场景7: clone github远程仓库时, Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快, 在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`.
10. 场景8: 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；
11. 场景9: 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick `命令，把bug提交的修改“复制”到当前分支，避免重复劳动
12. 场景10: 丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除
13. 场景11: 本地分支和远程分支关联 `git branch --set-upstream-to <branch-name> origin/<branch-name>` 
14. 场景12: 将远程仓库的某个分支拉取,并关联到本地的分支`git checkout -b dev origin/dev`
15. 场景13: 多人协同合作
    1. git pull # 拉取最新
    2. 解决冲突
    3. git push origin <branch-name>
16. 场景14: 标签管理
    1. tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起, 也是版本库的快找
    2. git tag <tag_name> (commit_id): 默认标签是打在最新提交的commit上的, 或者指定的 commit id 上
    3. 'git tag'查看标签, `git show <tag_name>`查看标签信息
    4. 命令`git tag -a  -m "blablabla..."`可以指定标签信息
    5. 推送某个标签到远程 `git push origin <tagname>`(--tags 一次性推送)
    6. 命令`git tag -d `可以删除一个本地标签
    7. 命令`git push origin :refs/tags/`可以删除一个远程标签
17. 自定义 git
    1. 忽略特殊文件: 编写.gitignore, 并将其加入仓库中; `git check-ignore -V file` 检查哪个规则写错
    2. 配置别名: git config --global alias.st status
    3. Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`, 可以直接修改这个文件