---
layout: post
title: "利用github和jekyll建立个人网站小结"
date: "2017-07-20"
abstract: "利用git建站完整流程,以及命令行可能出现的问题。"
keywords: ["web建站"]
---
## 利用github＋Jekyll搭建个人网站小结
@(web建站)[前端开发]
> 本文章主要呈现了我在利用github建站中碰到的各种问题，以及问题的解决方案。本文章若有任何不当之处请大家及时指正，万分感激！！以及郑重感谢呆恋小喵的无私指导！！

#### 使用 Jekyll + Github Page 搭建个人网站
首先，闲话不说，直接上教程。需要从头开始建站的小伙伴请移驾[此处](https://sunmengyuan.github.io/garden/2017/02/24/github-blog.html)。该文档详细介绍了如何利用jekyll和github page搭建个人网站，简单易懂，风趣幽默，特别适合小白食用。ps：写文档的是一个美貌与智慧并存的。。。喵。

因为有详尽的文档，在本文章中就不再赘述如何建立个人网站了。主要和大家分享一下我在建立网站时遇到的各种麻烦，以及作为一个从未接触过git的小白，是如何在大神以及度娘帮助下解决问题的。供大家遇到问题的时候参考。

#### 安装jekyll
安装jekyll就是一个坑。要安装jekyll，你就需要先安装ruby和rubygems的环境。而要安装ruby你需要安装xcode。安装xcode，你需要在appstore里下载3个小时左右。。。呵呵。。。附上教程。。小伙伴们加油。。。

[安装jekyll](http://jekyll.com.cn/docs/installation/)
[安装ruby](http://www.jianshu.com/p/f7f901f5e768) 安装ruby时默认是一起就把rubyGems的环境也搭建好了。

**坑货出没**：不要执着的安装ruby的最新版本（比如现在最新的版本是2.4.0），除非能成功找到路径，不然就是无尽的报错以及空间下降。反正我们最终是为了利用rubyGems成功安装jekyll，所有请选取能找到安装包路径的版本安装。比如目前，ruby 2.0.0版本是没有问题哒，也就是说你就老老实实按照教程来吧，后期玩儿熟了再更新ruby也不迟。


#### 克隆远程库
我们在github网站上建立了一个空的仓库后(repository),我们需要把它clone到本地。其实就是下载下来啦。只不过你都用github了，难道不想用git 命令行装装X（开玩笑啦，其实学会git命令行后真的很方便）。废话不多说，克隆远程库的教程请看[此处](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375233990231ac8cf32ef1b24887a5209f83e01cb94b000)。

简单来说就是，在Github网页上你的仓库中的右上角，找到克隆里的那个标志，点一下就copy下来。接下来打开终端，直接输入`git clone` ，粘贴你刚刚copy的东西，回车，就可以愉快克隆到本地了。

**坑货出没**：注意这里克隆有两种协议供选择，一个是ssh，一个是https。默认ssh，但是当你没有ssh密钥或者有密钥但是不会用时，建议你：
a）选择https协议的内容来copy后再clone(不推荐)
b）使用命令行添加新密钥（有密钥的也添加，然后在网页上删除旧密钥就行。当然根据下面的指南，在你有密钥的情况下，也可以选择直接将旧密钥链加入到ssh-agent中，省略掉建立新密钥过程，在这里就不说了）

[生成SSH密钥指南](https://help.github.com/articles/connecting-to-github-with-ssh/)

不想看英语的筒子们可以看以下简单粗暴的介绍。
1 打开终端 输入以下命令行。注意邮箱这里输入的是你Github账户的邮箱
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
2 然后出现以下代码，直接不停的按enter直到世界尽头。。。然后。。就好了。
这里注意，有些伙伴已经有密钥了，还要执着新建，那么疯狂enter的时候不要闭着眼睛，稍微看着点出现的代码，如果出现说已经存在pub文件，要不要overwrite，**请不要**按enter，输入**yes**

> Generating public/private rsa key pair.
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]

3 把生成的ssh密钥添加到ssh-agent里

输入
```
eval "$(ssh-agent -s)"
```
出现以下代码
> Agent pid 59566

接着输入
```
ssh-add -K ~/.ssh/id_rsa
```
4 把密钥添加如github账户（终于等到你。。还好我没放弃。。）
首先先把密钥复制下来，你可以手动复制一段类似下面这样的代码。但是我劝你还是算了，有命令行的时候最好少动手，因为你不知道自己是不是手贱星人一不小心就复制少东西。这里直接给大家一个复制用的命令行。输入后，直接愉快的去github网页中的setting中的ssh里添加就好。（我在说中文？？？）
```
pbcopy < ~/.ssh/id_rsa.pub
```

#### push内容到远程库上（本地上传github）
关于这个坑，我只有一句话，clone容易push难，秒速逼死英雄汉。
小伙伴将个人网站的仓库克隆到本地后，无论是自己强大的写了各种代码，还是暗搓搓的用了别“神”的代码，你都要重新push回你的github中（远程库）。包括以后各种修改更新你的个人网站，你都需要把内容push回去。
有的小伙伴rp好，很顺利按着步骤就能push成功（也就是成功上传）。而有的幸运E就是死都push不上去（呵呵。。。不要看我。。。）有一个可能的原因。。。敲黑板。。。幸运E们看好啦。。。

你可能在不知不觉中，手贱的新建了一个分支（new branch），并且本地和github上的仓库在不同分支上，那样你死都push不上去。

一般出现这种情况提示的错误是
> error:failed to push some refs to 'git@github.com:flatdemo/flatdemo.github.io.git'
> hint:Updates were rejected because a pushed branch tip is behind its remote

这个是时候，你就需要看一下自己是不是建立新分支了，在**本地仓库文件夹**下运行以下命令行：
```
git branch
```
出现：

![1.png](http://olz69zcwa.bkt.clouddn.com/1.png)

瞧！手贱了吧！不过手贱不要怕，请输入
```
git checkout master
git branch -d newbranch
```
输入完后，终端会出现代码贱兮兮的问你，你真的要删吗？如果要删人家，请run 一下 `git branch -D newbranch`.然后你就乖乖输入这句代码。马上成功删除。接着输入
```
git branch -a
```
出现下图就OK啦！

![2.png](http://olz69zcwa.bkt.clouddn.com/2.png)

接下来你可以输入（注意是在本地仓库根目录下run）
```
git pull --rebase
```
如果报错，就输入
```
git branch --set-upstream-to=origin/master master
```
然后再次输入
```
git pull --rebase
```
这个时候你可以想删删，想改改，想干啥干啥。。。在本地仓库文件中为所欲为。。。为所欲为了一番后，你决定继续荼毒无辜的远程库，这时候你要确保你打开了jekyll server（如何开看下一节）。开了之后，请在**本地仓库文件根目录**下输入
```
git add .
git commit -m '请填写你为什么为所欲为，为所欲为了啥'
git push origin master
```
然后去github的仓库里看一眼，发现荼毒成功。。很好。。high five。。。

#### 启动jekyll server
在本地修改代码，要在用端口号做测试，或者上传github。就一定要先打开jekyll的服务。
启动输入
```
jeykll server
```
如果出现以下错误

![3.png](http://olz69zcwa.bkt.clouddn.com/3.png)

说明jekyll-paginate 这个插件没有，要安装一下。这个时候， 在本地仓库文件夹根目录下输入
```
gem install jekyll-paginate
```

安装好插件就OK！（如果报错，先用`gem install jekyll`命令再装一下jekyll，然后再按上面命令装一次插件，一般不用这样干）

好了，写到这儿，差不多没什么了，不多说了。。祝大家建站顺利！！