# GIT

**GIT Bash** : Unix与Linux风格命令行，使用最多，推荐最多

**Git CMD** ：Windows风格命令行

**Git GUI** ： 图形界面Git，不建议初学者使用

***

> 常用的Linux命令学习：

1. cd：改变目录
2. cd.,：回退到上一个目录
3. pwd：显示当前所在的目录路径
4. Is(II)：都是列出当前目录的所有文件，只不过更详细
5. rm：删除一个文件， rm index.js就会把index.js删除
6. torch：新建一个文件，torch index.js就会在当前目录下新建一个index.js文件
7. mkdir：新建一个目录
8. rm -r：删除一个文件夹，rm -r src 删除src目录  **rm -rf / 切勿在Linux下尝试**
9. mv 移动文件, mv index.html src index.html 是我们要移动的文件在当前目录下，src是目标目录
10. reset:重新初始化终端/清屏
11. clear:清屏 （windows：cls）
12. history：查看命令历史
13. help：帮助
14. exit：退出
15. #表示注释

***

Git命令:

1. git config -l
2. git comfig --system --list
3. git config --global --list

> 所有的配置文件都保存在本地

1）、Git\etc\gitconfig:Git 安装目录下的gitconfig --system 系统级

2）、Administor\ .config

配置用户名和邮箱

```
git config --global user.name "yu" #名称
git config --global user.email "1141956855@qq.com" #邮箱
```

> **Git命令**

```
git add .
git commit
git push
```



> 本地仓库搭建

```
git init #初始化项目
git clone "url" #远程克隆
```

> Git相关操作

```
#添加所有文件到暂存区
git add .  //step 1

#查看所有文件状态
git status	//step 2

#指定文件
git status [filename]

# 提交暂存区中内容到本地仓库  -m 提交信息
git commit -m "消息内容"	//step 3
```



> 忽略文件

1. 忽略文件中的空行或以#开始的行
2. 可以使用Linux通配符。如：（*）代表多个字符，（？）代表一个字符，（[abc]）代表可选字符范围，（{string1,2}）可选字符串
3. 如果名称最前面是一个（！），表示例外规则，不被忽略
4. 如果名称最前面是一个（/），表示要忽略的文件在此目录下，而子目录不忽略
5. 如果名称最后面是一个（/），表示要忽略此目录下的该名称的子目录，而非文件（默认文件或目录都忽略）

```
#写在 '.gitignore'文件下
*.txt 				#忽略所有.txt结尾的文件
!lib.txt			#但是lib.txt除外
/temp 				#忽略项目根目录下的TODO文件，不包括其他目录下的temp
build/				#忽略build/目录下的所有文件
dos/*.txt			#忽略doc/notes.txt 但不包括dos/sever/arch.txt
```

> 生成公钥

```
ssh-keygen -t rsa
```

> 修改文件，使用IDEA操作git

- 添加到暂存区
- commit提交
- push到远程仓库



> ### Git分支

```
git branch			#查看分支
git branch -r		#远程分支
git branch dev 		#新建分支
git merge [branch] 	#合并指定分支到当前分支
```

