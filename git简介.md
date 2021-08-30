# git简介





### 1.git属于分布式版本管理系统


### 2.相较于集中式版本管理系统，git的优势

不必联网、安全、分支管理、没有中央服务器（分布式原理图）

![image-20210401100428685](C:\Users\T\AppData\Roaming\Typora\typora-user-images\image-20210401100428685.png)



分布式原理图

![image-20210401100757735](C:\Users\T\AppData\Roaming\Typora\typora-user-images\image-20210401100757735.png)

### 3.git常用命令

准备工作：

1.在合适的地方创建一个空的文件夹

<span style="color:red">注意：在window系统下千万别用记事本去修改文件，会乱码报语法错误</span>

git bush

- pwd	      查看当前所在目录
- git init       将当前目录创建成git版本仓库
- ls -ah         查看是否初始化完成
- git add . /  文件名   将文件添加到暂存区
- git commit -m'备注'     将暂存区的文件提交到版本库
- git log / git log --pretty=oneline       查看提交历史
- git reflog  查看命令历史
- git reset --hard head ^ （一个^代表一个版本） /  git reset --hard  commitID  （id不一定需要写全，可以使用git log查找commitID）   版本回退
- git checkout --file    将工作区本次修改的内容回滚到本次修改之前的内容
- git status 查看提交状态
- rm 文件名   在工作区将文件删除
- git rm 文件名 =》git commit -m'备注'  在版本库将文件删除
- git checkout 使用版本库版本替换工作区版本
- git ls-files 查看暂存区文件

远程仓库管理

1.创建一个空仓库

- git remote add origin  远程仓库地址

- git push -u origin 分支名   将本地版本库推到远程仓库  （第一次的时候要加-u）

- git clone  远程仓库地址


分支管理

- git branch 查看分支

- git branch 分支名  创建分支

- git checkout 分支名 or git switch 分支名

- git checkout -b 分支名  or  git switch -c 分支名  创建加切换分支

- git merge 分支名 将某个分支合并到当前分支

- git branch -d <name>  删除某个分支

- git remote -v 查看远程仓库信息
  
git 常见问题
  
  Q1:git@github.com: Permission denied (publickey).fatal: Could not read from remote repository.
  
  问题点：ssh公钥配置不对或者本地git仓库并未和ssh key 关联上
  
  A1:在/Users/用户名/.ssh 找到公钥文件，如没有可以打开Git Bash
     ssh-keygen -t rsa -C "youremail@example.com" 生成好公钥，将公钥复制，在github 上新增一个ssh key
     （第二种情况参考：https://blog.csdn.net/dotphoenix/article/details/100130424）
  
  
  Q2: failed to push some refs to 'github.com:jimingzhuang/kuaiquxiu-uniApp.git 提交文件拒绝
  
  问题点：主要原因是github中的README.md文件不在本地代码目录中
  
  A2:先将线上仓库的文件拉去下来git pull --rebase origin master，然后再提交git push -u origin master
  












