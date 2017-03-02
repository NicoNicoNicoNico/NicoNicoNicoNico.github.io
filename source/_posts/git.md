---
title: git踩坑全记录
date: 2017-03-02 15:59:49
tags:
---

# npm安装错误
![](http://i1.piimg.com/567571/1e5edf20962813cd.png)

这是一个权限问题，由Tim Schaub提供的修复是递归地将/ usr / local文件夹中的文件所有者更改为当前用户：

`sudo chown -R $USER /usr/local`

<!-- more -->
链接[https://ar.al/scribbles/npm-install-g-please-try-running-this-command-again-as-root-administrator/](https://ar.al/scribbles/npm-install-g-please-try-running-this-command-again-as-root-administrator/)   


## fatal: Not a git repository (or any of the parent directories): .git

提示说没有.git这样一个目录 ------>  `git init`


## git push 的时候说failed to push some refs to 'xxxxx'

```
To git@192.168.1.48:xxxxgit

 ! [rejected]        master -> master (non-fast-forward)

error: failed to push some refs to 'git@192.168.1.48:xxxx.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
```


在`.git/config`中缺少以下两句，加上就可

```
[branch "master"]
      remote = origin
      merge = refs/heads/master
```

链接 [git使用常见问题](http://blog.csdn.net/huguohu2006/article/details/7045052) 

## permission denied

```
Initialized empty Git repository in `/Users/username/Documents/cakebook/.git/`
Permission denied (publickey).
fatal: The remote end hung up unexpectedly
```
ssh出问题了吧

* 先找到并打开你放ssh的文件夹,终端运行
`cd ~/.ssh && ssh-keygen`

* 运行:

On OS X run: `cat id_rsa.pub | pbcopy`

On Linux run: `cat id_rsa.pub | xclip`

On Windows (via Cygwin/Git Bash) run: `cat id_rsa.pub | clip`

* 去网站（git）添加ssh.
* .gitconfig.
```
git config --global user.name "bob"
git config --global user.email bob@... 
```
(别忘了重启命令行来确定配置重新加载)

链接 [http://stackoverflow.com/questions/2643502/git-permission-denied-publickey](http://stackoverflow.com/questions/2643502/git-permission-denied-publickey)

## md编辑

不废话到只放个链接
[Markdown，你只需要掌握这几个](http://www.tuicool.com/articles/fmeMbqR)




