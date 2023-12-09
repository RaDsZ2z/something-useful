# P0

https://www.bilibili.com/video/BV1HM411377j?p=1&vd_source=8924ad59b4f62224f165e16aa3d04f00

# P2 安装和初始化配置

```shell
git config --global user.name RaDsZ2z
git config --global user.email 1070245317@qq.com
git config --global credential.helper store

git config --global --list #查看git配置信息
```

# P3 新建仓库

```shell
#1.
git init#在当前目录生成.git(把此目录当成仓库)
#2.
git init my-repo#创建my-repo文件夹 并在其中生成.git(将my-repo当成仓库)
#3.
git clone xxx#可以创建仓库

```

# P4 工作区域和文件状态

## 工作区域

Git的本地数据管理分为三个区域：工作区、暂存区、本地仓库

工作区：就是我们自己电脑上的目录

暂存区：临时存储区、用于保存即将提交到Git仓库的修改内容

本地仓库：通过git init创建的仓库，包含了完整的项目历史和元数据
![image-20230819115946455](https://github.com/RaDsZ2z/git---github/assets/129292565/9b465fcf-a0ef-4cd0-a1c7-113e20fb2dbc)


## 文件状态

![image-20230819120047650](https://github.com/RaDsZ2z/git---github/assets/129292565/4b13ae65-1b49-48cc-8397-92e5ad85f68a)


# P5添加和提交文件

涉及到的命令

```shell
git init #创建仓库
git status #查看仓库的状态
git add #添加到暂存区
git commit #提交
#git commit -m "commit"
git log #查看提交记录 会有提交ID 作者 邮箱之类的信息
#作者 邮箱 就是P2初始化时自己设置的信息
git log --oneline #查看简洁的提交记录(只有提交ID和commit备注)
```

# P6 git reset回退版本

![image-20230819142128900](https://github.com/RaDsZ2z/git---github/assets/129292565/30532a90-dd5e-4b58-988c-9eea68e6cded)


```shell
git reset 模式 版本号 #模式省略的话 默认为--mixed
git reset ^HEAD #回退到上一版本 模式为--mixed
git ls-files #查看已经被add的文件
```

# P7 git diff查看差异

```shell
git diff #查看工作区和暂存区之间的差异
git diff HEAD #查看工作区和版本库(本地仓库)之间的差异
git diff cached #查看暂存区和版本库之间的差异

#git diff还可以用来比较两个版本之间的差异
#用法就是在后面加上两个版本的提交ID就行了
git diff ID1 ID2
```

除了使用提交ID之外 还可以使用HEAD来表示当前分支的最新提交

**HEAD 是Git中的一个非常重要的概念，它指向分支的最新提交节点**

```shell
#此外
HEAD~
GEAD^
#都表示上一个版本
HEAD~2表示之前的两个版本
HEAD~3表示之前的三个版本
```

还可以只查看文件差异内容

```shell
git diff HEAD HEAD~ file3.txt
#查看最新版本和上一版本文件file3.txt的差异内容
```

“还可以查看两个分支之间的差异 学到分支再详细讲解”

# P8 使用git rm删除文件(从版本库中删除文件)

```shell
git rm filename #把对应文件从工作区和暂存区都删除
#commit之后就从版本库中删除了
gir rm --cached filename #从暂存区删除 而不删除工作区的本地文件
```

# P9 gitignore 忽略文件(夹)

## 忽略文件

创建.gitignore文件

将文件名输入其中，例如

```shell
#.gitignore
a.txt
b.txt
*.log
#两个txt文件和所有.log文件都将不会被add到暂存区
只有当文件是未被追踪的文件时，忽略才会生效
忽略生效后，git status将不显示被忽略的文件
```

## 忽略文件夹

也可以忽略文件夹，文件夹名后要以/结尾

```shell
# .gitignore
temp/
```

这样就忽略了temp文件夹

顺带一提，git会默认忽略空文件夹

## .gitignore文件匹配规则

1.空行或以#开头的行会被Git忽略

2.使用标准的Blob模式匹配

​	*通配任意字符

​	？匹配单个字符

​	[]表示匹配列表中的单个字符，例如[abc]表示a/b/c

3.两个星号**表示匹配任意的中间目录

4.中括号可以使用短中线连接，比如：

​	[0-9]表示任意一位数字，[a-z]表示任意一位小写字母

5.感叹号！表示取反

![image-20230819172531132](https://github.com/RaDsZ2z/git---github/assets/129292565/4f6ba48a-5f49-4b7a-bc37-7829e42b769c)


# P11 SSH配置和克隆仓库

**生成SSH密钥**

```shell
ssh-keygen -t rsa -b 4096
Enter file in which to save the key: #输入此密钥的文件名 也可直接回车(使用默认文件名id_rsa)
#如果直接回车会覆盖之前的密钥id_rsa
#然后输入密码 确认密码(也可以直接回车 密码为空)
Enter passphrase:
Enter same passphrase agin:
#SSH密钥的配置都这里就完成了
```

**如果刚刚指定了一个新的文件名(test)，那么还需要增加一步配置**

创建一个config文件，添加以下五行

```txt
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IndetityFile ~/.ssh/test
```

这个配置文件的意思是当访问github.com指定使用ssh下的test这个密钥

现在使用命令

```shell
git clone ...
#被提示输入密码 就是创建密钥时的密码
然后就克隆成功了
```

# P12关联本地仓库和远程仓库

```shell
git remote add origin ${SSH} #添加一个远程仓库 origin是远程仓库在本地的别名
git remote -v #查看当前仓库对应的远程仓库的别名和地址
git branch -M main #重命名本地分支为main(默认为main)
git push -u origin main #把本地的main分支和远程origin仓库的main分支关联起来
#该命令全称如下
git push -u origin main:main
#origin:远程仓库的别名
#main:main 把本地仓库的main分支推送给远程仓库的main分支
#左边的main对应本地分支名 右边的main对应远程仓库分支名
```

# P13 P14 P15

# P16 分支简介和基本操作

```shell
git branch #查看所有分支
git branch dev #创建一个名为dev的分支
git checkout dev #切换到dev分支
#git checkout还可以用来恢复文件或者目录到之前的某一个状态
#若文件与分支重名则有歧义 默认是切换分支而不是恢复文件
git switch dev #此命令专门用来切换分支 语义更加明确
```



创建的dev分支会有master分支的文件

在dev分支下执行的操作不会影响到master分支

```shell
#切换到master分支 然后
git merge dev #这样就把dev分支合并到master分支了
```



查看分支图

```shell
git log --graph --oneline --decorate --all
```



合并之后分支还是存在的，如果不再需要可以删除分支

```shell
git branch -d dev #如果dev分支已经被合并，可以使用此命令删除
git branch -D dev #强制删除还没有被合并的分支
```

