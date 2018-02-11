这段时间一直在自学前端框架，做了很多小东西，眼看着自己的github也在不断的变绿，没想到人的满足感竟然被这一面墙的绿格子所驯服

满屏的深绿估计是很多人炫耀的资本，本人不是什么大牛，不过自己辛辛苦苦一行一行调试好了，最后连这点炫耀的记录都不给显示！！git你要翻天啊！

本来想着就这几天的，没了自己认倒霉，可是今天一搜索，没想到是可以恢复的，太NM的神奇了，一下是我查询到的恢复github commits记录及统计的办法

首先要分析为什么你的提交记录没有被github识别：

进行Commits的用户没有被关联到你的Github帐号中。

不是在这个版本库的默认分支进行的Commit。

这个仓库是一个Fork仓库，而不是独立仓库。

我估计很多人和我一样都是第一个原因，初用github远程管理代码和那些经常更换使用机器的猿极有可能用错账户名和邮箱，其实我在修改自己的原来的用户名和邮箱是就发现，

当初设置的用户名竟然是自己的密码。。。。用户名是邮箱，但是为什么平时可以正常提交呢。。。想想才反应过来，我都不用bash去push，而是在git的图形工具里进行diff和push，

那就难怪了，在GUI里，一般都是默认提交时输入邮箱和密码的，这里又有一个坑，我每次都是在用户那一个alert里输入自己的邮箱，然后是密码，这里要说，如果你输入邮箱，

就一定要注意你的这个提交账户和简历repo的账户名要关联，不然够呛了，你辛辛苦苦改了几个月发现那个炫富的绿墙里什么鬼都没有，呵呵

至于下面的两个原因应该在多人合作开发中会遇到吧

下面是解决的办法：

这是github官方的办法，全英文

https://help.github.com/articles/changing-author-info/

然后我一直在疑惑里面说的那个script在哪里，后来在另外一个大侠那里找到答案了。。。那块被墙了。。。对，那块代码在天朝看不到

## 开始
然后在bash里执行如下代码，user替换成你的github账户名，repo.git替换成你的repo的名字
```
 git clone --bare https://github.com/user/repo.git
 cd repo.git
 ```
## 复制粘贴脚本，并根据你的信息修改以下变量：旧的Email地址，正确的用户名，正确的邮件地址

```
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="旧的Email地址"
CORRECT_NAME="正确的用户名"
CORRECT_EMAIL="正确的邮件地址"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

按 Enter键 执行脚本。
用git log命令看看新 Git 历史有没有错误
## 把正确历史 push 到 Github
```
git push --force --tags origin 'refs/heads/*'
```
删掉刚刚临时创建的 clone
```
cd ..
rm -rf repo.git
```

## 接下来全局设置好你的正确信息，以后就放心的用Github进行版本管理吧 ^_^
```
git config --global user.email "你的邮件地址"
git config --global user.name "你的Github用户名"
```

## 如果后面出现无法push的情况
```
git pull origin master --allow-unrelated-histories
# 或者
git commit -a
```