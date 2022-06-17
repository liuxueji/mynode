1.git status 查看当前文件状态，上面的是之前的，下面的是新的
2.git add . 将新的全部添加
3.git commit -m "完成了登录模块" 将文件提交到本地仓库
4.git branch 查看目前所处分支（所处login分支，master为主分支）
5.git checkout master 切换到主分支（若想合并到主分支，先要切换到主分支）
6.git branch 查看当前所处分支（所处master分支）
7.git merge login 合并login分支
8.git push 推送到云端仓库。首次推送需要给个名字
9.git checkout login 切换到login分支，因为只推送了mastar，云端没有login分支
10.git branch 查看当前所处分支（所处login分支）
11.git push -u origin login 将login推送到云仓库，因为是第一次推送，所以需要给个名字，一般什么分支就给什么名字
12.git checkout -b home 创建并切换到一个新分支，开始新的功能编写

