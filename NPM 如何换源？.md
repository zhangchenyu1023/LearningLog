# NPM 如何换源？

对于 NPM，下载完后不换源的话，下载依赖包就相当的慢，最后超时下载失败也经常发生。

这时候我们就要改一下 NPM 的 registry 配置，换成国内的镜像源。

## 查看源

先看看源指向哪里：

```text
npm config get registry
```

默认是指向 `https://registry.npmjs.org/`，也就是官方源。

## 更换源

国内源有很多，我这里用淘宝源吧。毕竟是大公司，会比较稳定。

```text
npm config set registry https://registry.npmmirror.com/
```

> 一些文章还是写着旧的淘宝 NPM 镜像 `registry.npm.taobao.org`，但它已于 2022 年 05 月 31 日 废弃，读者需要更换为新的 `registry.npmmirror.com` 源。

这个配置会持久化保存到 `~/.npmrc` 文件中，你也可以通过手动改该文件来修改配置。

## nrm

因为换源要记域名很麻烦，还要手打较长的命令，所以我们可以考虑安装 nrm 包

```text
npm i -g nrm
```

nrm 其实就是 NPM registry manager，管理 NPM 源泉的简单命令行工具。

> 令人悲伤的是，如果用国外源安装 nrm，有可能会因为超时而安装失败。

通过 `nrm ls` 会列出一些可选择的公有源：

```text
$ nrm ls

  npm ---------- https://registry.npmjs.org/
  yarn --------- https://registry.yarnpkg.com/
  tencent ------ https://mirrors.cloud.tencent.com/npm/
  cnpm --------- https://r.cnpmjs.org/
  taobao ------- https://registry.npmmirror.com/
  npmMirror ---- https://skimdb.npmjs.com/registry/
```

通过 `nrm use <源的名称>`，则会配置为对应的 registry url。

```text
$ nrm use taobao


   Registry has been set to: https://registry.npmmirror.com/
```

nrm 工具的子命令不只是这些，比如可以在列表中新增自己的私有源。不过基本来说，也就前面提到的这两个最常用。具体可以阅读它的文档。

话说它好像有点小 bug。作者其实并没有好好维护，曾经有一段时间 npm 升级，nrm 没有更近，导致不可使用，后来是修好了。