# brew 源的替换

brew install XXX一直卡在Updating Homebrew…的解决办法
运行命令brew install pip3，结果界面一直卡在Updating Homebrew…上


## 1. 替换brew.git

homebrew托管于github，更新homebrew就是从git上拉取最新的版本。
有时候git的速度也很慢，会导致更新受阻，那么就需要给git仓库换一个远程地址。

```
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

```

## 2. 替换homebrew-core.git
替换Homebrew 核心软件仓库的地址。

```
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

## 3. 替换Homebrew Bottles源

官方预先编译好的软件会被装在一个bottle里直接下载解压到系统里，无需本地编译。
Bottle是放在bintray上面的，在国内依然不快。可以通过换bottle的源地址来加速bottle的下载：

对于bash用户：

```
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```

对于zsh用户
```
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

解释一下，安装方法就是替换HOMEBREW_BOTTLE_DOMAIN这个环境变量，所以把export这一行加入到配置文件中，再用source命令让它立即生效就可以了。

# 重置为官方源

要是想换回来，GitHub源的地址在这里：

https://github.com/Homebrew/brew.git
https://github.com/Homebrew/homebrew-core


```
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core
```

另外，如果Homebrew Bottles源也被替换了的话，只要在~/.bash_profile配置文件里删除掉对应的命令所在行，并source一下即可。


