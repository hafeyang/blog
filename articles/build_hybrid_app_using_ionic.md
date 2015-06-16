# 使用ionicframework开发hybrid app

[ionicframework][1]提供一套非常简洁扁平的控件库，用户快速的构建web app。他使用[cordova/phonegap][2] 构建app。cordova/phonegap为不同的平台提供相同的JavaScript接口。UI封装基于[angular][3]。目前适用于Android4+,iOS7.1+。同时还集成一套图标库[ionicons][4]。他是一个低成本的快速开发跨平台app的不二选择。

##Android开发环境搭建
先下载Android SDK。官方的SDK下载地址被墙，可以到[androiddevtools.cn][5]下载，ionic支持的API Level为21。创建虚拟机用于开发。虚拟机如果比较慢，可以采用与开发及其相同的架构，比如采用x86，增大内存能缓解。不需要安装很大的IDE，因为业务逻辑代码都是web，可以采用类似[Sublime Text][6]比较轻便的编辑器。

##iOS开发环境搭建
iOS开发需要安装Xcode，[申请iOS开发者账号][7].

##通用开发环境搭建
首先需要安装[Node.js][8]，安装完后在命令行输入`node -v` 能看到Node.js版本号即安装成功。使用npm 安装 cordova 和ionic

```shell
npm install -g cordova ionic
```
还需要安装[gulp][9]构建工具
```shell
npm install -g gulp
```
可以使用ionic 提供的命令行命令创建一个项目:这里以myApp为例
```shell
ionic start myApp tabs
```
tabs表示app自带tab，可以根据需要选择 `blank` `tabs` `sidemenu`

可以直接在web中访问
```shell
cd myApp
ionic serve #启动浏览器用http://localhost:8100访问，支持livereload
ionic platform add android #添加android平台支持
#ionic build android #编译android apk
#ionic emulate android #在模拟器上运行
#ionic run android #运行apk，如果有USB连接手机直接在手机上运行.
#ios 类似
```

#部署应用
Android上打包发布版apk
```shell
ionic build android --release
```
签名，首先生成keystore文件, keytools在android sdktools下
```shell
keytool -genkey -alias demo.keystore -keyalg RSA -validity 40000 -keystore demo.keystore
```
签名
```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore caref.keystore platforms\android\build\outputs\apk\android-armv7-release-unsigned.apk caref -signedjar CarefWatch-arm.apk
```
验证签名
```shell
jarsigner -verify -verbose -certs  CarefWatch-arm.apk
```
iOS下使用Xcode中的Product->Archive工具根据向导一部一部做。插件已经发布的版本可以点击菜单中Window->Orgnizer

##调试web
通过`ionic serve` 可以开启一个浏览器调试web app。ionic 是基于[sass][10]。如果修改了sass文件，需要起作用可以使用`gulp sass`，也可以直接运行`gulp watch` ，他会自动监听scss文件，及时更新css并刷新浏览器。
当app部署到设备上时如果需要调试，有以下几种办法：

###使用weinre
[weinre][11]是Web Inspect Remote的简称。可以使用npm安装
```shell
npm -g install weinre
```
在命令行
`weinre --boundHost=localhost`
可以在浏览器中输入`http://localhost:8081/client`远程调试
在web页面最下方嵌入js
```html
<script src="http://10.10.10.113:8080/target/target-script-min.js#anonymous"></script>
```
需要注意的是在部署的时候需要去掉该script。

###利用浏览器远程调试功能
Android可以参考[此处][12]
iOS可以参考[此处][13]
Android上如上面文章中提到的需要修改java代码。也可以安装[crosswalk][14]，可以直接用chrome远程调试，当然crosswalk不仅仅有这个功能，后面会详细提到。

##使用数据推送
app关掉时可以通过各平台的数据推送。可以使用 cordova的[Push插件][15]


  [1]: http://ionicframework.com/
  [2]: http://phonegap.com/
  [3]: https://angularjs.org/
  [4]: http://ionicons.com/
  [5]: http://www.androiddevtools.cn/
  [6]: http://sublimetext.com/
  [7]: https://developer.apple.com/devcenter/ios/index.action
  [8]: http://nodejs.org/
  [9]: http://gulpjs.com/
  [10]: http://sass-lang.com/
  [11]: http://people.apache.org/~pmuellr/weinre-docs/latest/Home.html
  [12]: https://developer.chrome.com/devtools/docs/remote-debugging
  [13]: https://developer.apple.com/library/safari/documentation/AppleApplications/Conceptual/Safari_Developer_Guide/GettingStarted/GettingStarted.html
  [14]: https://crosswalk-project.org/
  [15]: https://github.com/phonegap-build/PushPlugin



[gimmick:Disqus](hafeyang.disqus.com)