# Mac 软件管理方案

## 备份
软件安装有以下 4 种方式
1. Mac App Store 安装，位置：/Applications
2. 手动下载安装，位置：/Applications
3. Homebrew 安装，位置：/usr/local/Cellar，主要是一些命令行工具
4. Homebrew Cask 安装，位置：/usr/local/Caskroom，主要是各种普通软件，如 Alfred、Steam 等

```sh
# All Apps
ls -lh /Applications > ~/Library/Mobile\ Documents/com~apple~CloudDocs/AppList/All_AppList

# MAS Apps
/usr/local/bin/mas list > ~/Library/Mobile\ Documents/com~apple~CloudDocs/AppList/MAS_AppList

# brew Apps
/usr/local/bin/brew list > ~/Library/Mobile\ Documents/com~apple~CloudDocs/AppList/Brew_AppList

# brew cask Apps
/usr/local/bin/brew cask list > ~/Library/Mobile\ Documents/com~apple~CloudDocs/AppList/BrewCask_AppList
```

## 自动安装
需要说明的是，第一行命令生成的所有软件列表，鱼龙混杂，需要你自己挑选并安装。

后面几种可以自动安装。但是，在安装前，你应该检查列表文件，去除一些不再需要的 App，确保内容无误。

```sh
# 进入 iCloud 中的 AppList 文件夹
cd  ~/Library/Mobile\ Documents/com\~apple\~CloudDocs/AppList

# 生成 MAS_AppList 安装命令
cat AppList/MAS_AppList | sed "s/(.*)//g" | sed -Ee 's/([0-9]+) (.+)/mas install \1 #\2/g' > ~/Desktop/AppInstaller

# 生成 Brew_AppList 安装命令
echo "\nbrew install $(cat AppList/Brew_AppList | tr '\n' ' ')" >> ~/Desktop/AppInstaller

# 生成 BrewCask_AppList 安装命令
echo "\nbrew cask install $(cat AppList/BrewCask_AppList | tr '\n' ' ')" >> ~/Desktop/AppInstaller

# 开始安装
chmod +x ~/Desktop/AppInstaller
~/Desktop/AppInstaller
```