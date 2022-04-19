> 在使用Git的过程中，我们喜欢有的文件比如日志，临时文件，编译的中间文件等不要提交到代码仓库，这时就要设置相应的忽略规则，来忽略这些文件的提交。

## .gitignore

```
.DS_Store
node_modules
/dist


# local env files
.env.local
.env.*.local

# Log files
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# Editor directories and files
.idea
.vscode
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

```

## 规则 作用

```
/mtk 过滤整个文件夹
*.zip 过滤所有.zip文件
/mtk/do.c 过滤某个具体文件
!/mtk/one.txt 追踪（不过滤）某个具体文件
注意：如果你创建.gitignore文件之前就push了某一文件，那么即使你在.gitignore文件中写入过滤该文件的规则，该规则也不会起作用，git仍然会对该文件进行版本管理。
```

### 配置语法

>以**斜杠**“/”开头表示目录；
>以**星号**“*”通配多个字符；
>以**问号**“?”通配单个字符
>以**方括号**“[]”包含单个字符的匹配列表；
>以**叹号**“!”表示不忽略(跟踪)匹配到的文件或目录。
>注意： git 对于 `.gitignore`配置文件是按行从上到下进行规则匹配的

#### Git 忽略文件提交的方法

> 有三种方法可以实现忽略Git中不想提交的文件。

##### 1. 在Git项目中定义 `.gitignore` 文件

> 这种方式通过在项目的某个文件夹下定义 `.gitignore` 文件，在该文件中定义相应的忽略规则，来管理当前文件夹下的文件的Git提交行为。

`.gitignore` 文件是可以提交到共有仓库中，这就为该项目下的所有开发者都共享一套定义好的忽略规则。

在 `.gitingore` 文件中，遵循相应的语法，在每一行指定一个忽略规则。如：

```
*.log
*.temp
/vendor
```



##### 2. 在Git项目的设置中指定排除文件

这种方式只是临时指定该项目的行为，需要编辑当前项目下的 `.git/info/exclude` 文件，然后将需要忽略提交的文件写入其中。

需要注意的是，这种方式指定的忽略文件的根目录是项目根目录。

##### 3. 定义Git全局的 .gitignore 文件

> 除了可以在项目中定义 .gitignore 文件外，还可以设置全局的 git .gitignore 文件来管理所有Git项目的行为。这种方式在不同的项目开发者之间是不共享的，是属于项目之上Git应用级别的行为。

这种方式也需要创建相应的 `.gitignore` 文件，可以放在任意位置。然后在使用以下命令配置Git：

`git config --global core.excludesfile ~/.gitignore`
