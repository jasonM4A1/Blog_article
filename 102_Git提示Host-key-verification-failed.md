---
title: Git提示Host key verification failed
date: 2021-01-28 23:34:35
tags:
    - Git
    - 报错
    - github
categories:
    - 报错
cover:
    https://gitee.com/jasonM4A1/pictureHost/raw/master/img/20210129042155.png
---
# 问题描述
Git提示Host key verification failed.

# 问题描述
## 方法一
输入 ssh -T git@github.com后，出现下面提示:
```fingerprint is SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc.Are you sure you want to continue connecting (yes/no)?```
不能直接按"回车"键过去,要手动输入"yes"才可以.
## 方法二
在生成密钥后，你得把ssh私钥添加到ssh-agent中。不然你是无法链接到已经添加ssh密钥的平台的。

在后台启动ssh-agent。

$ eval "$(ssh-agent -s)"

将SSH私钥添加到ssh-agent。如果您使用其他名称创建密钥，或者要添加具有其他名称的现有密钥，请使用私有密钥文件的名称替换命令中的id_ed25519。

$ ssh-add ~/.ssh/id_ed25519
