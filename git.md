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
5. git reflog: 所有的 HEAD 的改动, 以便返回 reset 之前的commit

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