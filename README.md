# post-receive

一种服务端 git 钩子的实现，git 钩子请参考：https://git-scm.com/docs/githooks

## 背景

类似 Nodejs、Python 等的动态语言，在进行部署的时候，是可以直接运行源码的。

如果我们将代码存储在类似 github、gitlab 等 git 仓库，在目标服务器部署时，为了不暴露代码的历史信息，一般是会将将源码下载之后，删除对应的 git 记录，也就是删除项目根目录下的 .git 文件。但是随之而来的问题是，运维无法判断下载的代码是否是最新的。

本钩子可以实现，当 git 服务端接收到请求时，会根据 git tag 生成对应的*CHANGELOG.md*文件，从而判断代码对应的版本号。

## 标签

开发测试完整后，开发同学需要将功能分支代码合并到 master 分支，并打上版本号标签。

版本号参考：https://semver.org/

可以通过 git 命令行打标签：

```sh
git tag -a v1.6.0 -m "this is some message for the version"
git push origin master --tags
```

## 使用

将该文件直接放入项目的 *.git/hooks* 目录下

![image-20200608214357452](/Users/videopls/Library/Application Support/typora-user-images/image-20200608214357452.png)

当 git 服务端接收到请求时，会根据 git tag 生成对应的*CHANGELOG.md*文件（已有则覆盖）

*CHANGELOG.md*的格式如下：

```markdown
# v2.0.0 - 2020-6-3 10:47:52
add some feature

# v1.0.0 - 2020-6-3 10:46:43
first version
```