#  M1 配置 ReactNative 环境
> 必须安装的依赖：CocoaPods、Node、Watchman、yarn、npm、Xcode/Command Line Tools，下面是我的电脑配置好后各个依赖项的当前版本号。（时间：2121.4.1）

```c++
hmc@bogon ~ % pod --version
1.10.1
hmc@bogon ~ % node -v
v14.16.0
hmc@bogon ~ % watchman -v
4.9.0
hmc@bogon ~ % brew -v
Homebrew 3.0.9-4-g9308a25-dirty
Homebrew/homebrew-core (git revision db93e97262; last commit 2021-03-25)
Homebrew/homebrew-cask (git revision 18b68ccc10; last commit 2021-03-25)
hmc@bogon ~ % npm -v
6.14.11
hmc@bogon ~ % yarn -v
1.22.10
```

> 首先找到应用程序 -> 实用工具 -> 终端.app，然后右键显示简介勾选 **使用 Rosetta 打开**，同样当 Xcode 安装完成后也要进行勾选，保证终端和 Xcode 都以 Rosetta 兼容模式运行。

## 1. Homebrew 安装
&emsp;首先 Homebrew 的安装需要本机先安装 Xcode 以及 Command Line Tools。（现在下载完 Xcode 会默认安装 Command Line Tools）然后下面这个链接是 Homebrew 的安装教程：[Homebrew国内如何自动安装（国内地址）](https://zhuanlan.zhihu.com/p/111014448)
```c++
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```
&emsp;Homebrew 安装时如果 homebrew-core 安装失败，此时可直接 sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core 创建文件夹，然后 cd 到此文件夹下执行 sudo git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git。homebrew-cask 同理：sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask 创建文件夹，然后 cd 到此文件夹下执行 sudo git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask。

## 2. Node 的安装
&emsp;直接 [nodejs 官方](https://nodejs.org/en/) 下载 pkg 进行安装。

## 3. watchman 的安装
&emsp;直接使用 brew install watchman 命令安装 watchman 如果出现一个 .git/config 权限不够的问题，可用如下指令改变目录权限。
```c++
sudo chgrp -R admin /usr/local
sudo chmod -R g+w /usr/local
```

## 4. CocoaPods 的安装
&emsp;修改 Ruby 的 gem源（gem sources）到 https://gems.ruby-china.com/。
```c++
gem sources // 列出默认源
gem sources --remove https://rubygems.org/  // 移除默认源
gem sources -add https://gems.ruby-china.com/  // 添加国内源
gem sources -l // 确保只有 gems.ruby-china.com
sudo gem install cocoapods // 安装 cocoapods
```
&emsp;安装过程中如果遇到：warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777 错误，执行如下指令。
```c++
sudo chmod go-w /usr/local/bin
sudo chmod 775 /usr/local
```
&emsp;安装 ffi：sudo gem install ffi。

&emsp;cocoapods 安装完成后 pod setup。

&emsp;Yarn 安装：npm install -g yarn。

## 5. 测试 RN 环境
&emsp;以上需要的库全部安装完成后，测试 RN 环境。[React Native 搭建开发环境](https://reactnative.cn/docs/environment-setup)

&emsp;在本地创建一个文件夹，并 cd 到该文件夹执行：npx react-native init AwesomeProject，然后执行：yarn ios，如果能正常启动则表示 RN 环境已经搭建完成。 

## 运行我们的项目
1. 首先保证 ReactNative 环境配置完整，能正常运行 demo。
2. 代码拉下来后首先在根目录下执行 npm i 指令，下载 node_modules 文件夹。
3. 然后在 ios 文件夹下执行 pod install，执行后修改 src/config/config.ts.example 文件，去掉 .example 后缀。
4. node_modules/react-native-render-html/src/HTMLImage.js 文件 109 行修改为：`style={[{ width: this.state.width, height: this.state.height, resizeMode: "cover" }, style]}`。
5. Xcode 运行过程中如果遇到：libPods.a 无法找到的问题，进入 Pods 的 PROJECT 选中 Build Settings，更改 Architectures 的 Base SDK 为 iOS 后重新编译运行。

### 真机运行时
1. AppDelegate.m 文件中的 **jsCodeLocation** 要设置为当前电脑的 IP 如：jsCodeLocation = [NSURL URLWithString:@"http://192.168.5.128:8081/index.bundle?platform=ios"];
2. 保证电脑和手机在同一个 WIFI 网络下。
3. 也可以配置到 release 模式下。

### 代码提交注意事项
1. 需要在根目录下执行：npm i -D eslint husky lint-staged
