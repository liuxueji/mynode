1. fork项目到自己仓库，名字和原项目一致

2. 将自己仓库中的项目clone到本地

   > 此时，本地已经有自己仓库里的项目了，但是只关联了自己仓库的，我们还需要关联原仓库的，因为原仓库的代码可能会更新，能够让我们每次都拿到最新代码

3. 关联原仓库

   > git remote -v 查看关联的仓库
   >
   > git remote add upstream 关联原仓库（即上游仓库）
   >
   > 再次输入 git remote -v 检查是否关联成功上游仓库







示例

我们已经将原仓库进行fork到自己仓库

![image-20220606102002175](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606102002175.png)

![image-20220606102043831](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606102043831.png)

并且，已经将仓库clone到本地

![image-20220606102105613](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606102105613.png)

接下来查看一下我们关联的仓库



```
git remote -v
```

![image-20220606102201151](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606102201151.png)



接下来，我们关联上游仓库

```
git remote add upstream https://github.com/woai3c/visual-drag-demo.git
```



查看已经关联的仓库

```
git remote -v
```

![image-20220606102330922](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606102330922.png)



成功关联到上游仓库



如何将本地仓库更新为原仓库最新的内容呢？

git fetch upstream   //抓取原仓库的修改文件

![image-20220606103232306](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606103232306.png)



git checkout -b test_branch 新建并切换分支

> 原仓库更新的代码，需要把它放到新建的分支中

![image-20220606103342060](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606103342060.png)



git merge upstream/main 合并原仓库中的分支（上游仓库/分支）

![image-20220606103426132](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220606103426132.png)

git push origin test_branch 将分支推送到仓库

总结：

github 开发程中， 我们常需要fork出一个仓库进行开发， 但是原来的仓库更新之后，fork出的仓库需要进行一波同步。

下面介绍一下终端的操作：

1. git remote -v 查看远程库地址；

2. git remote add upstream XXXXXXXXXXXXXXX.git     //upstream 设置原仓库的名字，后面是原仓库的地址；

3. git fetch upstream   //抓取原仓库的修改文件

4. git checkout XXX  // 切换到需要合并的本地仓库的本地分支。

   > git checkout -b xxx(直接新建一个分支然后切换至新创建的分支).就是创建加切换分支.

5. git  merge upstream/dev   //将原仓库的Dev 分支与本地仓库的当前分支合并

6.  XXX_branch   //将当前仓库的本地分支推送到远程分支