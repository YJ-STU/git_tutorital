# git window使用文档

## day 1

<font color=blue>基础命令</font>

```git
pwd #输出当前工作路径  

cd #更改当前工作路径

git config --global user.name "YJ-STU" #更改全局名字，用于发表和连接github#

git config --global user.email "15280105292@163.com" 
git init #生成Git管理文件
stat .git #查看管理文件  macos是open init#
touch 1.py #创建空文件1.py
git add #添加文件到版本库中  git add . #一次添加多个文件
git status #观察版本库当前状态
git commit -m "creat 1.py" #提交一次更改

```

## day 2

<font color="blue">访问和修改记录</font>

```git
git log #查看提交状态下的修改记录
git status / git status -s #查看未提交的信息 红绿M要注意
查看两次修改的不同
git diff  #unstaged修改部分和commit文件的不同
git add .
git diff --cached  #查看staged状态下和commit文件的不同#
git diff HEAD     # staged & unstaged间差异

```

<font color="blue">回溯到从前</font>

```git
#修改并覆盖已commit的文件
##首先复制1.py to 2.py
git add 2.py
git commit -amend --no-edit  ##no-edit 表示不编辑直接合并到上一个commit
git log --oneline ##每个commit内容显示一行
#reset 回到add之前
git add 1.py
git status -s 
git reset 1.py  #从stage状态撤回#

#reset回到commit之前 此命令不能回溯中间的某一个特定版本 
##方式1 “HEAD”
git reset --hard HEAD^ #回溯到前几个就加几个^#==git reset --hard HEAD~1 #~1,2,100#
##方式2 “commit id”
git reset --hard c7eab98 ## 此时change2消失了 
#挽回消失的change2#
git reflog  ##
git reset --hard ab2f3ce #类似于从记录中撤回刚刚的删除记录#

#改写文件checkout
$ git checkout c7eab98 -- 1.py #将id为c7eab98的change1 吓得1.py放回1.py文件#
git add 1.py
git commit -m "back to change 1 and add comment for 1.py"
git log --oneline
```

<font color="blue">branch</font>

```git 
git log --oneline --graph #图形样看目前存在的目录和分支
git branch dev #建立dev分支#
git branch #查看当前分支
git branch -D dev #删除某一分支#
git checkout dev #将当前分支切换到dev上
git checkout -b pub #创建并切换到新的分支
git checkout dev
```

`dev` 分支中的 `1.py` 和 `2.py` 和 `master` 中的文件是一模一样的. 因为当前的指针 `HEAD` 在 `dev` 分支上, 所以现在对文件夹中的文件进行修改将不会影响到 `master` 分支.

```
git commit -am "change 3 in dev"
git checkout master
##方式1 采用fast forward格式进行merge 不会有commit信息 log也不会有分支的图案
git merge dev
git log --oneline --graph
#输出
* 53ec587 (HEAD -> master, dev) change 3 in dev
* d65028c (pub) bake to change 1 and add commit 1.py
* ab2f3ce change 2
* c7eab98 change 1
* 2fbb171 creat 1.py
##方式2
git merge --no-ff -m "keep merge info"
git log --oneline --graph
#输出
*   2e60b31 (HEAD -> pub) keep merge info
|\
| * 53ec587 (master, dev) change 3 in dev
|/
* d65028c bake to change 1 and add commit 1.py
* ab2f3ce change 2
* c7eab98 change 1
* 2fbb171 creat 1.py
```

<font color="red"> merge 冲突</font>

当在dev中做出修改同时 master也发生了更改，会使merge无法很好直接操作。具体冲突git会在1.py文件中标注出来

```
a = 1
# I went back to change 1
<<<<<<< HEAD
# edited in master
=======
# edited in dev
>>>>>>> dev
#对其进行修改#
a = 1
# I went back to change 1

# edited in master and dev
##############
#然后
git commit -am "solve conflict"
git log -- online --graph
#结果如图 #
*   7810065 solve conflict
|\  
| * f7d2e3a change 3 in dev
* | 3d7796e change 4 in master
|/  
* 47f167e back to change 1 and add comment for 1.py
* 904e1ba change 2
* c6762a1 change 1
* 13be9a7 create 1.py
```

<font color="blue">rebase</font>

<font color="red">**!!! 只能在你自己的分支中使用 rebase, 和别人共享的部分是不能用 !!!**</font>

```
git checkout master
git rabase dev
#对1.py文件进行修改
git branch #此时停留在一个临时分支上#
git add 1.py
git rebase --continue

```

<font color="blue">临时修改stash</font>

```
git checkout dev
git stash #将修改到一半的文件暂时挂起#
#切换到别的分支做别的事情
git checkout dev
gir stash pop #回复暂存继续操作

```

<font color="red">建立github版本库</font>

```
git remote add origin https://github.com/YJ-STU/study-flow.git
git push -u origin master     # 推送本地 master 去 origin
git push -u origin dev        # 推送本地 dev  去 origin
#推送修改
git commit -am "change 5"
git push -u origin master
```















