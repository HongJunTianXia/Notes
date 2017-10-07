# git入门
使用Git前，需要先建立一个仓库(repository)。您可以使用一个已经存在的目录作为Git仓库或创建一个空目录。
使用您当前目录作为Git仓库，我们只需使它初始化。<br/>
*git init*<br/>
使用我们指定目录作为Git仓库。<br/>
*git init newrepo*<br/>
从现在开始，我们将假设您在Git仓库根目录下，除非另有说明。<br/>
### 添加新文件
我们有一个仓库，但什么也没有，可以使用add命令添加文件。<br/>
*git add filename*<br/>
可以使用add... 继续添加任务文件。<br/>
### 提交版本
现在我们已经添加了这些文件，我们希望它们能够真正被保存在Git仓库。<br/>
为此，我们将它们提交到仓库。<br/>
*git commit -m "Adding files"*<br/>
如果您不使用-m，会出现编辑器来让你写自己的注释信息。<br/>
当我们修改了很多文件，而不想每一个都add，想commit自动来提交本地修改，我们可以使用-a标识。<br/>
*git commit -a -m "Changed some files"*<br/>
*git commit* 命令的-a选项可将所有被修改或者已删除的且已经被git管理的文档提交到仓库中。<br/>
千万注意，-a不会造成新文件被提交，只能修改。<br/>
### 发布版本
我们先从服务器克隆一个库并上传。<br/>
*git clone ssh://example.com/project.git*<br/>
现在我们修改之后可以进行推送到服务器。<br/>
*git push ssh://example.com/project.git*<br/>
### 取回更新
如果您已经按上面的进行push，下面命令表示，当前分支自动与唯一一个追踪分支进行合并。<br/>
*git pull*<br/>
从非默认位置更新到指定的url。<br/>
*git pull* http://git.example.com/project.git<br/>
### 删除
如何你想从资源库中删除文件，我们使用rm。<br/>
*git rm file*<br/>
### 分支与合并
分支在本地完成，速度快。要创建一个新的分支，我们使用branch命令。<br/>
*git branch test*<br/>
branch命令不会将我们带入分支，只是创建一个新分支。所以我们使用checkout命令来更改分支。<br/>
*git checkout test*<br/>
第一个分支，或主分支，被称为"master"。<br/>
*git checkout master*<br/>
对其他分支的更改不会反映在主分支上。如果想将更改提交到主分支，则需切换回master分支，然后使用合并。<br/>
*git checkout master*<br/>
*git merge test*<br/>
如果您想删除分支，我们使用-d标识。<br/>
*git branch -d test*<br/>
### 清除缓存
*git rm -r --cached .*
