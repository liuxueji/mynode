# github提交失败：

> 在使用git准备将本地仓库推送到云端时，报错了，经排查后解决了该问题并记录解决过程。

## 1.问题

> `error: failed to push some refs to 'github.com:liuxueji/myNode.git'`

## 2.原因

> 远程仓库与本地仓库依赖不同，所以会导致本地仓库提交失败。

## 3.解决办法

> 先使用pull更新本地仓库，使本地仓库与远程仓库保持一致，然后再push到远程仓库

#### 先pull

```
git pull origin master  
```

#### 再push

```
git push -u origin master
```

