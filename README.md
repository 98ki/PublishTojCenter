#gradle-publish

##介绍
本项目包含一些Gradle脚本及属性文件，用于使用Gradle发布项目.

`bintray.gradle`: 用于发布到JCenter的脚本。

`build.gradle`: 怎么使用它的Demo.

`gradle.properties`: 在`bintray.gradle`中要用到的属性，需要拷贝到你的项目中并进行配置。

##如何使用
###1. 保存bintray账号信息
首先注册bintray账号，地址：https://bintray.com/ ，账号生成后会自动为你分配一个API Key，账号名以及API Key是我们能够上传库到bintray的钥匙。 
我们需要将这些信息存到本地，也就是你的系统.gradle目录，这里要注意，我们保存在系统下，而不是你的project下的.gradle目录，如果你没有配置过GRADLE_USER_HOME的环境变量，则是在你的用户目录下，编辑gradle.properties（如果没有则创建），加入配置：
```
BINTRAY_USER=bintray account name
BINTRAY_KEY=bintray API Key
```
并且你的OS是Windows， 并且是XP，那么一般是在` C:\Documents and Settings\用户名\.gradle`，而如果是win7以上，那么是在`c:\Users\用户名\.gradle`。

###2. 修改library目录下的`build.gradle`文件
首先在你的library项目中的`build.gradle`添加以下的构建脚本依赖：
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath "org.jfrog.buildinfo:build-info-extractor-gradle:3.1.1"
    }
}
```
然后在这个 `build.gradle`的底部添加以下代码：

    apply from: 'https://raw.githubusercontent.com/msdx/gradle-publish/master/bintray.gradle'

###3. 配置library目录下的`gradle.properties`
复制本项目下的 `gradle.properties`文件(注意不是第二步中修改的那个文件)到你的library目录下, 点 [Here](https://github.com/msdx/gradle-publish/blob/master/gradle.properties) 直接复制。
接下来对内容进行配置，下面是一个例子：
```
PROJ_GROUP=com.philcalvin
PROJ_VERSION=1.1.0
PROJ_NAME=iconbutton
PROJ_WEBSITEURL=https://github.com/pnc/IconButton
PROJ_ISSUETRACKERURL=
PROJ_VCSURL=git@github.com:pnc/IconButton.git
PROJ_DESCRIPTION=android icon button
PROJ_ARTIFACTID=com-phillipcalvin-iconbutton

DEVELOPER_ID=pnc
DEVELOPER_NAME=Phil Calvin
DEVELOPER_EMAIL=phil@philcalvin.com
```
上面的例子最终在Android Studio中的引用形式为：
```
dependencies {
    compile 'com.philcalvin:com-phillipcalvin-iconbutton:1.1.0'
}
```
可以发现，它的格式是 `PROJ_GROUP:PROJ_ARTIFACTID:PROJ_VERSION`组成。
###4. 执行发布命令
执行 `gradle bintrayUpload` 将库发布到 bintray.com.
```
gradle bintrayUpload
```
执行 `gradle artifactoryPublish` 可以发布版本到 oss.jfrog.org.
```
gradle artifactoryPublish
```
###5. 将库加入Jcenter
最后一步，需要登录bintray.com，将我们刚刚发布的库申请加入到jcenter，这样别人才能直接引用到，具体如何操作可以参考下面链接的博文。


*注:* 当前版本不会再在通过 Android Studio 运行项目时引发错误。

