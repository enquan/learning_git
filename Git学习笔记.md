# Git学习笔记

[TOC]

## Git简介

### Git的诞生

- Git是使用C语言开发的
- Git是目前最流行的**分布式**版本控制系统

### 集中式 VS 分布式

```
Git本地仓库包含代码库还有历史库，在本地的环境开发就可以记录历史
而SVN的历史库存在于中央仓库，每次对比与提交代码都必须连接到中央仓库才能进行
这样的好处在于：
1、自己可以在脱机环境查看开发的版本历史
2、多人开发时如果充当中央仓库的Git仓库挂了，任何一个开发者的仓库都可以作为中央仓库进行服务
```

## 安装Git

[Git官方下载](https://git-scm.com/downloads>)

安装完成后，还需要进行以下设置：

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

以上操作即为“自报家门”。

**注意**：`git config`命令的`--global`参数表示对这台机器上所有的Git仓库都使用这个配置。

### 出现的问题

- 安装后运行Git Bash出现以下界面

![错误提示](C:\Users\enquan\Desktop\Markdown\Git\1.jpg)

​	**解决方法**：Git不能安装在含有中文的路径下，卸载后重新安装在不含中文的路径下即可。

## 创建版本库

- ```
  版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
  ```

- 通过`git init`命令将目录变成Git管理的仓库，如将桌面的Git文件夹变成仓库：

  在Git Bash中输入以下命令

  ```
  cd C:\Users\enquan\Desktop\Git
  git init
  ```

### 添加文件到版本库

- 版本控制系统只能跟踪**文本文档**的变动
- 注意：文件统一使用**UTF-8**编码

1. 使用`git add <filename>`，此命令可反复使用，添加多个文件；
2. 使用`git commit -m <message>`，说明添加的文件或改动

## 时光机穿梭

- `git status`：查看仓库的状态——没有变动；存在变动未提交文件；存在变动、提交文件未说明（commit）
- `git diff <filename>`：查看修改内容

### 版本回退

- `HEAD`指针指向当前版本，`HEAD^`指针指向当前版本的上一个版本，`HEAD^^`指针指向当前版本的上两个版本，如此类推
- `git reset --hard <commit_id>`或`git reset --hard HEAD类型指针`：当前版本变为历史版本（`commit_id`为前几位即可）
- `git log`：查看提交历史及各版本对应的commit_id
- `git reflog`：查看操作历史

### 工作区和暂存区

-  暂存区可以类比于“购物车”，工作区可以类比为最终结账（commit）购买的物品

### 管理修改

- `git add`命令实质是将文件添加进暂存区，因此在commit之前需要将文件添加进暂存区
- 在对多个文件进行操作时，每个文件都需要添加进暂存区（`git add`），再进行`git commit`操作才起作用

### 撤销修改

- 丢弃**工作区**的修改：`git checkout -- <filename>`
- 丢弃**暂存区**的修改：`git reset HEAD <filename>`→`git checkout -- <filename>`

- **文件操作和对应的区变化**

  | 文件 | 文件修改后 | add  | add后  | commit |       commit后       |
  | :--: | :--------: | :--: | :----: | :----: | :------------------: |
  |      |   工作区   |      | 暂存区 |        | 当前分支（HEAD指针） |

### 删除文件

- 从版本库中删除文件：`git rm <filename>`→`git commit -m <message>`
- 从版本库中恢复误删文件：`git checkout -- <filename>`

## 远程仓库

- 创建SSH Key：`ssh-keygen -t rsa -C "youremail@example.com"`

### 添加远程库

```git
git remote add origin https://github.com/<username>/<repostiory name>.git
git push -u origin master
```

（第二条指令中，`-u`参数使Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令）

今后本地库提交：`git push origin master`

### 从远程库克隆

```
git clone https://github.com/<username>/<repostiory name>.git
```

## 分支管理

```text
分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。
```

### 创建与合并分支

- 查看分支：`git branch`

- 创建分支：`git branch <name>`

- 切换分支：`git checkout <name>`
- 创建+切换分支：`git checkout -b <name>`
- 合并某分支到当前分支：`git merge <name>`
- 删除分支：`git branch -d <name>`

### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

- 用`git log --graph`命令可以看到分支合并图。

### 分支管理策略

通常，在合并分支时，如果可能，Git会用`Fast forward`模式。这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

- 强制禁用`Fast Forward`模式下的合并操作：

  ```git
  git merge --no-ff -m <message> <branch name>
  ```

- 查看分支历史（`--pretty=oneline`指在一行内显示分支，`--abbrev-commit`指显示缩写的`commit id`）：

  ```
  git log --graph --pretty=oneline --abbrev-commit
  ```

### Bug分支

- `git stash`：将当前工作区储藏起来
- `git stash list`：查看`stash`列表
- `git stsah apply <stash id>`：恢复某个`stash`，当只有一个`stash`时，参数`<stash id>`可以省略（**仅恢复，不删除**）
- `git stash drop <stash id>`：删除某个`stash`，当只有一个`stash`时，参数`<stash id>`可以省略
- `git stash pop <stash id>`：弹出（恢复并删除）某个`stash`，当只有一个`stash`时，参数`<stash id>`可以省略

### 强制删除分支

- 开发新功能，最好新建一个分支
- `git branch -D <branch name>`：强制删除未合并的分支

### 多人协作

- `git remote (-v)`：查看远程库消息（`-v`参数查看详细信息）
- `git push origin <branch name>`：推送分支

#### 抓取分支

- `git clone <Repo address>`：克隆远程库至本地（克隆前需配置SSH Key）
- 默认情况下，克隆远程库在本地只能看到`master`分支，需要自行在本地库中创建`dev`分支进行开发
- `git pull <branch name>`：抓取最新版本的远程库

#### 多人协作的工作模式

在完成`dev`分支的开发后：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`（抓取分支）试图合并；
3. 如果合并有冲突，则解决冲突，并在**本地提交（commit）**；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to origin/<branch-name>（远程） <branch name>（本地）`

- **本地和远程分支的名称最好一致**

### Rebase

- `git rebase`：将本地未push的分叉提交历史整理成直线
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比

## 标签管理

### 创建标签

- `git tag <tag name>`：为当前分支的当前版本创建标签
- `git tag <tag name> <commit id>`：为某个分支的历史版本创建标签
- `git tag -a <tag name> -m <tag message> <commit id>`：添加含有说明的标签
- `git tag`：查看现有的标签（按字母排序的）
- `git show <tag name>`：查看标签对应commit的信息

**注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签**

### 操作标签

- `git tag -d <tag name>`：删除指定的标签
- 创建的标签只存储在本地，不会自动推送到远程。因此，打错的标签可以在本地删除。
- `git push origin <tag name>`：推送某个标签到远程
- `git push origingt --tags`：批量推送尚未推送到远程的标签
- `git tag -d <tag name>`→`git push origin --delete <tagname>`：删除远程标签

## 使用Github

（略）

## 自定义Git

- `git config --global color.ui true`：让Git显示颜色

### 忽略特殊文件

- `touch .gitignore`：创建`.gitignore`文件
- 将需要忽略的文件添加至`.gitignore`文件中
- 提交（`add + commit`）到Git
- `git add -f <filename>`：强制添加文件
- `git check-ignore -v <filename>`：查询被忽略的原因
- [Github官方提供的.gitignore配置文件](https://github.com/github/gitignore)

### 配置别名

- `git config --global alias.<alias> <original name>`：配置别名（`--global`全局参数，不加`--global`则只对当前仓库起作用）
- 每个仓库的Git配置文件都放在`.git/config`文件中

### 搭建Git服务器

（略）

## 期末总结

- Git的官方网站：[http://git-scm.com](http://git-scm.com/)