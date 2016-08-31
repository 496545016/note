



##Egret Wing
###TypeScript
Egret 项目使用 TypeScirpt 语言来开发。TypeScript 是 JavaScript 的超集，具体内容可以参考 [TyptScript语言手册](http://bbs.egret.com/thread-1441-1-1.html)

###创建空项目
当大家熟悉 Egret 开发之后可以直接创建Egret游戏项目或者Egret EUI 项目等，里面包含了很多默认的配置。现在学习 Egret 第一步还是从一个完全空的项目来开始。

###使用 Egret Wing 创建项目
安装好Egret Wing 之后，打开 Wing 可以看到欢迎界面。在欢迎界面上面可以快速创建项目。

1. 新建Egret项目

    创建-新建Egret项目。当然也可以在文件菜单下的新建项目来创建我们的新项目。

2. 选择项目的基本配置

    选择建新项目之后会弹出新建项目的面板，在这里需要选择项目的基本配置。

    这里我们从一个游戏项目开始。游戏项目将会包含一套 HelloWorld 的模板，并包含我们常用的库几个库，```game``` , ```tween``` , ```res``` 。

    选择其他项目将会创建包含相应项目模板的项目，比如，EUI项目将会包含EUI项目模板。

    项目下面需要填写项目的名称和路径，这里大家应该都不陌生，项目名称是必填的字段，填写完成好后才能点击下一步。

    这里填入项目的名称 ```HelloWorld``` ,目录选择我的常用目录,当然也可以自定义。

3. 项目属性设置

    可以设置舞台的宽度和高度，这里的舞台是我们呈现游戏的地方，还可以选择舞台的背景颜色，可以根据项目的具体需要设定

    当我们的项目运行在不同的平台上面比如桌面浏览器和手机浏览器上面时，设备本身的宽度和高度是不同的。这时就需要缩放舞台来达到更好的用户体验。

    当我们的游戏运行在移动端的时候，会面临屏幕旋转的问题。这里我们旋转AUTO选项，舞台将随设备旋转而旋转。


####使用命令行创建项目

    $ egret create HelloWorld

创建上面的默认的游戏项目，如果有特殊需要可以加入参数 ```--type empty|game|gui|eui``` 来指定不同的项目。

如果需要设置项目的旋转和缩放模式，需要在项目内来修改

####项目结构

* Fighter 我们的项目名称

    - src 目录，存放我们的代码。我们编写的代码都放在src目录下面。

    - bin-debug 目录，项目编译和运行的debug目录，一般我们不要修改该目录下的内容。

    - libs 目录，这里面存放我们的库文件，包括 Egret 核心库和其他扩展库。当然以后添加了第三方库的话也会放在这里。

    - resource 目录，这里放置我们的资源文件，这里面有一个default.res.json 配置文件，用来配置资源。

    - template 目录，这里是项目调试过程中所需的目录，一般我们不需要修改该目录下的内容。
    - egretProperties.json 项目的配置文件，一般我们会用到里面的modules 字段来配置项目的模块。

    - index.html 项目访问的入口文件，我们可以在这里面配置项目的旋转缩放模式背景颜色等。

    - favicon.ico 一个ico。

####index.html
在 index.html 文件里可以完成很多配置。

* 打开文件，在第 ```<style>``` 里可以看到：

        background: #888888;

    这里可以设置舞台的背景颜色。

* 添加自定义的第三方库文件的引用

        <!--这个标签为不通过egret提供的第三方库的方式使用的 javascript 文件，请将这些文件放在libs下，但不要放在modules下面。-->
        <!--other_libs_files_start-->
        <!--other_libs_files_end-->

    第三方库文件放置在 ```<!--other_libs_files_start-->``` 和 ```<!--other_libs_files_end-->``` 之间。
    一般情况下这种第三方库是包含 ```.js``` 和 ```.d.ts``` 文件,不要包含 ```.ts``` 文件。

* 配置项目

        <div style="margin: auto;width: 100%;height: 100%;" class="egret-player"
             data-entry-class="Main"
             data-orientation="auto"
             data-scale-mode="showAll"
             data-resolution-mode="retina"
             data-frame-rate="30"
             data-content-width="480"
             data-content-height="800"
             data-show-paint-rect="false"
             data-multi-fingered="2"
             data-show-fps="false"
             data-show-log="false"
             data-log-filter=""
             data-show-fps-style="x:0,y:0,size:30,textColor:0x00c200,bgAlpha:0.9">
        </div>

    - ```data-entry-class="Main"``` 设置项目的入口文件，表示项目的入口类，默认为Main,如果需要自定义的话需要在项目中先创建类，然后在这里配置类的名字。

    - ```data-orientation="auto"``` 设置旋转模式。

    - ```data-scale-mode="showAll"``` 设置缩放模式。

    - ```data-frame-rate="30"``` 这里是运行的帧率。

    - ```data-content-width="480"``` 和 ```data-content-height="800"``` 用来设置舞台的设计宽和高

    - ```data-show-paint-rect="false"``` 设置显示脏矩形的重绘区域。

    - ```data-multi-fingered="2"``` 设置多指触摸

    - ```data-show-fps="false"``` ```data-show-log="false"``` 这里设置显示帧率和log，只有在调试时会显示，发布的版本会去掉。

    - ```data-log-filter=""``` 设置一个正则表达式过滤条件，日志文本匹配这个正则表达式的时候才显示这条日志。如 ```data-log-filter="^egret"``` 表示仅显示以 ```egret``` 开头的日志。

    - ```data-show-fps-style="x:0,y:0,size:30,textColor:0x00c200,bgAlpha:0.9"``` 这里设置fps面板的样式。目前支持默认的这几种设置，修改其值即可，比如修改面板位置可以设置 ```x``` 和 ```y``` ，改变大小可以设置 ```size``` ，改变文字颜色 ```textColor``` ，改变背景面板的透明度 ```bgAlpha```。

> 不要随意修改其他代码，包括其中的注释。


####egretProperties.json
这个文件里面进行项目配置，包括模块和第三方库的配置，发布和native相关配置。

我们比较常用的设置就是添加模块和第三方库。

具体的配置说明可以参考：[EgretProperties说明](http://edn.egret.com/cn/index.php/article/index/id/644)


###调试项目