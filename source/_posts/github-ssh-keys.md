---
title: Github ssh-key 与 deploy key
toc: true
date: 2016-10-19 22:53:07
tags: Github
---
## ssh-key
ssh-key 是用于用于认证 Github 账户的密钥，从 Github 通过 Git 下载开源项目时要求账户添加了 ssh-key。如果 `git clone` 时账户没有添加 ssh-key 则会在命令行出现下面的错误提示:
```bash
$ git clone https://github.com/npsabari/testrepo.git
Cloning into 'testRepo'...
Permission denied (publickey).
fatal: The remote end hung up unexpectedly
```

<!-- more -->

### 生成 ssh-key
> 示例的操作都是在 Mac 执行的

打开 `终端`，使用 `ssh-keygen` 命令生成 ssh-key
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/kang/.ssh/test.
Your public key has been saved in /Users/kang/.ssh/test.pub.
The key fingerprint is:
SHA256:Z//9gcDS40LheYjxpZOuDgbG5wRQ9X7O7BTXfo134es kang@ruikye
The key's randomart image is:
+---[RSA 2048]----+
| .....           |
|  .   .          |
|   .   o . .     |
|  . . . = X .    |
|   + o oS&oB . . |
|  . =   BoB.+ o.o|
|     +   B ..o.++|
|    . . + .  ..o+|
|      .o .    oE+|
+----[SHA256]-----+
```

### 添加 ssh-key
默认生成的 ssh-key 公钥在 `~/.ssh/id_rsa.pub` 文件，复制公钥上传到 Github。
```bash
# 复制 ssh 公钥
$ cat ~/.ssh/id_rsa.pub | pbcopy
```
在 Github 的账户设置中，打开 `SSH and GPG keys` -> `New SSH key` 添加复制的 `~/.ssh/id_rsa.pub` 公钥

## deploy key
deploy key 每个 Repository 都有一个，主要用于 push 代码时使用。每个 Repo 的 deploy key 都是单独设置的，不能多个 Repo 使用相同的 deploy key。如果 Repository 没有添加 deploy key 时直接 push 代码，会出现权限错误：
````shell
$ git push origin master
Permission denied (publickey).
fatal: Could not write to remote repository.

Please make sure you have the correct access rights
and the repository exists.
````

### 生成 deploy key
deploy key 和 ssh key 本质是一样的，都可以用 `ssh-keygen` 命令生成。
```bash
$ ssh-keygen -f ~/.ssh/deploy_key_repo1
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/kang/.ssh/test.
Your public key has been saved in /Users/kang/.ssh/test.pub.
The key fingerprint is:
SHA256:Z//9gcDS40LheYjxpZOuDgbG5wRQ9X7O7BTXfo134es kang@ruikye
The key's randomart image is:
+---[RSA 2048]----+
| .....           |
|  .   .          |
|   .   o . .     |
|  . . . = X .    |
|   + o oS&oB . . |
|  . =   BoB.+ o.o|
|     +   B ..o.++|
|    . . + .  ..o+|
|      .o .    oE+|
+----[SHA256]-----+
```
> __*注意：*__ 因为在生成 ssh-key 使用了默认 id_rsa.pub 文件存储，所以 deploy key 要放在其他的文件中。

由于 deploy key 不是放在默认文件，所以 deploy key 不能直接使用，需要添加到`SSL`的认证列表中：

```bash
# 添加默认的 id_rsa
$ ssh-add ~/.ssh/id_rsa

# 添加 deploy key
$ ssh-add ~/.ssh/deploy_key_repo1

# 查看所有 add 的 keys
$ ssh-add -l
2048 SHA256:kqQTYy......dsp+8 /Users/your/.ssh/id_rsa (RSA)
2048 SHA256:0b/vdS......62tok /Users/your/.ssh/deploy_key_repo1 (RSA)
```

### 添加 deploy key
```bash
# 复制 deploy key
$ cat ~/.ssh/deploy_key_repo1.pub | pbcopy
```
打开 Repository 的设置页，在 `Deploy keys` -> `Add deploy key` 添加公钥并勾选 `Allow write access`。