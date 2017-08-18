## Git push

* *$ git push origin test:master*         // 提交本地test分支作为远程的master分支

* *$ git push origin test:test*              // 提交本地test分支作为远程的test分支
如果想删除远程的分支呢？类似于上面，如果:左边的分支为空，那么将删除:右边的远程的分支。

* *$ git push origin :test*              // 刚提交到远程的test将被删除，但是本地还会保存的，不用担心。

## 常见错误:
### 一.error:failed to push some refs to ...
当要push代码到git时，出现提示.
问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：
1. 强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容
*git push -f*
2. 先把git的东西fetch到你本地然后merge后再push
*$ git fetch*
*$ git merge*
这2句命令等价于
*$ git pull *
可是，这时候又出现了如下的问题：
上面出现的 [branch "master"]是需要明确(.git/config)如下的内容
[branch "master"]

    remote = origin

    merge = refs/heads/master

这等于告诉git2件事:

1. 当你处于master branch, 默认的remote就是origin。

2. 当你在master branch上使用git pull时，没有指定remote和branch，那么git就会采用默认的remote（也就是origin）来merge在master branch上所有的改变

如果不想或者不会编辑config文件的话，可以在bush上输入如下命令行：

*$ git config branch.master.remote origin*

*$ git config branch.master.merge refs/heads/master*

之后再重新git pull下。最后git push你的代码吧。
