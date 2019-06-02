# ionic4 + angular7 的集成环境

如使用该前端脚手架，可以先安装

```bash
npm install -g poetry-cli

poetry init <project-name>

? 请选择使用的模板
❯ ionic4
  umi
  electron
  next

cd <project-name> && yarn install

yarn start
```

## 目录说明

```bash
src
├─app\@core 核心控制
│       ├─control 基础页面类，使用者可继承
│       ├─data 权限控制、用户、状态类
│       ├─helpers 工具类
│       ├─i18n 语言类
│       ├─ionic cordova插件集成
│       ├─model 数据定义接口
│       ├─net http和拦截器
│       ├─startup 初始化服务
│       ├─utils 提醒等工具
│
├─app\@shared 共享模块
│       ├─ component 共享组件
│       ├─ directives 共享指令
│
├─app\layout 公共布局类
│       ├─ passport 登录布局
│       ├─ tabs 主tabs布局
│
├─app\routes 业务组件类
│       ├─ account 账户业务
│       ├─ support 扩展业务
│       ├─ tabs 主业务页面
│       ├─ tutorial 首次启动时业务介绍页
│
├─assets 静态资源类
├─environments 环境配置
├─theme 自定义样式类
```

## 环境配置

### 1. JAVA环境

- 安装JDK
- 新建->环境变量名`"JAVA_HOME"`，变量值`"C:\Java\jdk1.8.0_05"`（即JDK的安装路径） 
- 编辑->环境变量名`"Path"`，在原变量值的最后面加上`“ ;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin ” `
- 新建->环境变量名`“CLASSPATH”`,变量值`“.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar”`


### 2. ANDROID SDK 环境

- 安装SDK
- 新建->环境变量名`"ANDROID_HOME"`，变量值`"D:\Android\android-sdk"`
- 编辑->环境变量名`"Path"`，在原变量值的最后面加上`“ ;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools ”`


### 3. gradle 环境

- 下载 `gradle `， 解压放到相关目录
- 新建->环境变量名`"GRADLE_HOME"`，变量值`"D:\Android\gradle-4.4"`
- 新建->环境变量名`"GRADLE_USER_HOME"`，变量值`"D:\Android\.gradle"`（本地仓库位置）
- 编辑->环境变量名`"Path"`，在原变量值的最后面加上`“ ;%GRADLE_HOME%\bin ”`


## 编译

* ionic cordova build android --release
* ionic cordova platform add android --save
* ionic cordova platform rm android --save
* git config --system core.longpaths true

## 调试

* ionic serve -lc
* ionic cordova run android --device --livereload
* ionic serve --address 192.168.168.202
* weinre --boundHost 192.168.168.202
* chrome://inspect/#devices
* 然后运行 ionic cordova run android -l -c -s 或者 ionic cordova emulate android -lcs
* ios 运行 ionic run/emulate ios -livereload -consolelogs -serverlogs
* ionic serve -lc 感觉 ionic serve 已经支持热更新了，好像没有必要加上-lc 这个调试参数，谷歌浏览器本身就有 APP 模式，加上这个参数以后可以在地址后面加上平台参数来模拟平台如http://localhost:8100?ionicplatform=android
* ionic cordova run android 在 android 模拟器或者真机上运行
* ionic cordova run android -lc 可以在真机上远程调试
* ionic cordova run android --device
* ionic cordova run android --prod --release
* ionic cordova build android --prod --release 生成发布的 apk

## 生成资源

* ionic cordova resources

## 热更新

* `cordova-hcp build` 将 `conrdova-hcp.json` 文件中 `content_url` , `update` 复制到 `chcp.json` 到这个中
* `cordova-hcp server`
* `cordova build --chcp-dev`

## 生成 key

- 另一种配置方法是使用 `Gradle` ，一个 `Android` 的自动化构建工具。
- `cordova build android` 的过程其实就是使用它。
- 你要在 `platforms/android` 目录下建立 `release-signing.properties` 文件，内容类似下面这样：

```
storeFile=relative/path/to/keystore
storePassword=SECRET1
keyAlias=ALIAS_NAME
keyPassword=SECRET2
```

## 连接不上手机

* `adb start-server` 启动服务
* `adb kill-server` 停止服务
*` adb devices ` 查看已连接设备

## 编译过程错误（先清除平台，再编辑）

* 如果在编译中发生错误，极有可能是`plugin`版本不对，或`android`版本不对，解决办法是 `npm run unplatform` 删除当前系统及`plugin`，并且一定要保证`config.xml`的`plugin`版本是最新的可用的，最好用 `https://www.npmjs.com/package/package `查询下版本
* 删除 `C:\Users\Administrator\.gradle\caches` 缓存
* `cordova clean android`
* `cordova build android`

