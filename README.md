
### 说明

由于使用 octopress deploy 部署博客的时候有些与主题相关的文件并不会 push 到 GitHub 中，比如: _layout 等文件；而这些文件是我自己根据原主题 minima 修改而来，我希望这些文件也push到github中。

最初的方法是手动把这些文件 push 到 GitHub，再使用 octopress deploy 来部署，但是这样的话 octopress deploy 的时候会出现部署不了的问题。

所以，暂时的做法是另外开了一个 repo，也就是本 repo 来专门保存博客的所有文件（包括修改的主题内容）。

### 使用该 repo 恢复 blog

当需要使用该 repo 来恢复 blog 的时候：

- 删除原 blog repo：Hicodechar.github.io
- Clone 本 repo 到本地
- 在本地删除本 repo 中的 .git
- 新建 blog repo：Hicodechar.github.io
- 把本 repo 的内容发布到 Hicodechar.github.io 中

其中，把本 repo 的内容发布到 Hicodechar.github.io 的方法：

```
rm -rf .git
git init
git remote add origin https://github.com/Hicodechar/Hicodechar.github.io.git

// 生成 _deploy.yml 文件
octopress deploy init git git@github.com:Hicodechar/Hicodechar.github.io

// build
jekyll build

// 部署
octopress deploy
```

### 注意事项

由于 backup 的备份不及时，可能有的 blog 文章没有备份过来；所以在删除原 blog repo 之前，需要把 _site 目录中的所有内容备份出来，添加到 backup 的 _site 中。

