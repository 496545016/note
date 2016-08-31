



##Egret API
###egret.Shape
此类用于使用绘图应用程序编程接口 (API) 创建简单形状。
Shape 类含有 graphics 属性，通过该属性您可以访问各种矢量绘图方法。

    var bg:egret.Shape = new egret.Shape();
    bg.graphics.beginFill( 0x336699 );
    bg.graphics.drawRect( 0, 0, this.stage.stageWidth, this.stage.stageHeight );
    bg.graphics.endFill();
    super.addChild( bg );

* graphic 图形绘制
    - beginFill 设置填充颜色
    - drawRect 绘制矩形
    - endFill 用来结束绘制工作。
    - super.addChild 将某个显示对象添加到某个显示容器上
        这是Egret引擎操作显示列表的一个最常用的方法，这里使用 ```super``` 是由于所调用的方法 ```addChild``` 是当前类的父类定义的。根据个人习惯，这里完全可以用 ```this.addChild```

###egret.TextField
TextField是egret的文本渲染类，采用浏览器/设备的API进行渲染，在不同的浏览器/设备中由于字体渲染方式不一，可能会有渲染差异如果开发者希望所有平台完全无差异，请使用BitmapText

    var tx:egret.TextField = new egret.TextField();
    tx.text = "Hi，你好，我是duminghong";
    tx.size = 32;
    tx.x = 20;
    tx.y = 20;
    tx.textColor = 0xfefefe;
    tx.width = this.stage.stageWidth - 40;
    this.addChild( tx );

* text 设置文本
* size 设置文字大小
* x、y 设置文本对象的x和y坐标
* textColor 设置文本颜色
* width 设置文本的宽度
* this.addChild 将某个显示对象添加到某个显示容器上

###响应用户操作

    var tx:egret.TextField = new egret.TextField();
    tx.text = "Hi，你好，我是duminghong";
    tx.size = 32;
    tx.touchEnabled = true;
    tx.addEventListener( egret.TouchEvent.TOUCH_TAP, this.touchHandler, this );

* 第一行设置touchEnabled为true，意即允许该显示对象响应Touch事件，这是Egret中特别需要注意的问题。因为所有的显示对象，默认都是不响应Touch事件的，这是基于性能考虑，因为打开对这种事件的响应，是对性能有不可忽略的影响的。

* 第二行代码新增一个方法的引用，这就是事件处理函数，我们需要事件处理函数中对用户操作做出对应的反应。

在Main类中，加入如下代码：

    private touchHandler( evt:egret.TouchEvent ):void{
        var tx:egret.TextField = evt.currentTarget;
        tx.textColor = 0x00ff00;
    }

这里的事件处理函数是用一个类方法来实现，还有一种简写的方法，直接作为匿名函数传入：

    tx.addEventListener(egret.TouchEvent.TOUCH_TAP, function(evt: egret.TouchEvent): void {
        tx.textColor = 0x0000ff;
    }, this);

###资源加载
Egret中所有的资源都是动态加载的。

####资源加载清单
通常 Egret 中的资源加载配置文件位于项目目录的resource文件夹内，取名 ```default.res.json```

默认的 ```default.res.json``` 已经包含若干资源的配置：

    {
        "resources": [
            {
                "name": "bgImage",
                "type": "image",
                "url": "assets/bg.jpg"
            },
            {
                "name": "egretIcon",
                "type": "image",
                "url": "assets/egret_icon.png"
            },
            {
                "name": "description",
                "type": "json",
                "url": "config/description.json"
            }
        ],
        "groups": [
            {
                "name": "preload",
                "keys": "bgImage,egretIcon"
            }
        ]
    }

* resource 可以视为资源库，当前游戏使用到的资源都可以放到这里。其中以资源为单位分别列出。每一项资源单位都包含三个属性：
    - name：表示这个资源的唯一标识符。注意资源比较多的项目应确定一套命名规则，避免不同资源命名之间重复或太接近而易混淆。

    - type：表示资源类型。

        每个 ```resource``` 单位中的type，是Egret约定好的若干类型，最常用的有以下类型：

        + image：表示各种常见的图片类型，包括 ```PNG``` 和 ```JPG``` 格式，载入后将解析为 ```egret.Texture对象```；
        + text：表示文本类型，即文本文件，载入后将解析为 ```string对象```；
        + json：也是一种文本类型，不过内容是 ```json``` 格式的，载入后将直接解析为 ```json对象```；

* groups 是预加载资源组。很多情况下，我们在某种游戏场合，需要同时加载若干资源，用以准备后续的游戏流程显示。们可以将若干项资源定义为一个资源组。需要时，只需加载这个资源组即可。

    每项是一个资源组。每一个资源组须包含两个属性：

    - name：表示资源组的组名

    - keys：表示这个资源组包含哪些资源，里面的逗号分隔的每一个字符串，都与 ```resource``` 下的资源 ```name``` 对应。

看看实际应用：

    {
        "resources": [
            {
                "name": "figure_01",
                "type": "image",
                "url": "assets/pic_1.png"
            },
            {
                "name": "figure_02",
                "type": "image",
                "url": "assets/pic_2.png"
            },
            {
                "name": "figure_03",
                "type": "image",
                "url": "assets/pic_3.png"
            },
            {
                "name": "figure_04",
                "type": "image",
                "url": "assets/pic_4.png"
            },
            {
                "name": "figure_05",
                "type": "image",
                "url": "assets/pic_5.png"
            },
            {
                "name": "figure_06",
                "type": "image",
                "url": "assets/pic_6.png"
            },
            {
                "name": "change",
                "type": "sound",
                "url": "assets/change.mp3"
            },
            {
                "name": "bgMusic",
                "type": "sound",
                "url": "assets/bg.mp3"
            }
        ],
        "groups": [
            {
                "name": "figure",
                "keys": "figure_01,figure_02,figure_03,figure_04,figure_05,figure_06,change,bgMusic"
            }
        ]
    }


###在程序中加载资源
在 ```Main.ts``` 中的开头部分，我们会发现大量使用RES开头的代码，RES就是专门用来加载资源的类，这些代码我们稍后再分析，首先我们完成把这些图片载入所需的步骤。

注意，在 ```onConfigComplete``` 的最后，有一行加载资源组的代码： ```RES.loadGroup("preload");```

很显然，```loadGroup``` 就是用来加载资源组的。由于我们将资源组命名为 ```figure```，因此这里代码中的 ```preload``` 需要改成 ```figure```。 资源加载结束后，我们需要判断所加载的资源是哪个资源组的，所以 ```onResourceLoadComplete``` 中的 ```preload``` 也需要改成 ```figure```。

完成这些改动后，Egret将会加载 ```figure``` 资源组，并且程序执行到 ```createGameScene``` 时，资源组已经加载完成。


###加载资源的代码分析
在进一步显示图片前，我们了解一下资源加载的代码。 再回过头看看加载资源的代码。加载资源的过程整体分为两部分，第一步首先加载资源配置清单，第二步就是加载资源。

在onAddToStage方法中，有代码：

    RES.addEventListener(RES.ResourceEvent.CONFIG_COMPLETE, this.onConfigComplete, this);
    RES.loadConfig("resource/default.res.json", "resource/");

这是专门用来加载资源配置的代码。 首先添加一个针对 ```CONFIG_COMPLETE``` 事件的侦听，然后执行加载。 配置加载完成时，即会执行 ```onConfigComplete``` 方法。

在onConfigComplete方法中，有如下 ：

    RES.removeEventListener(RES.ResourceEvent.CONFIG_COMPLETE, this.onConfigComplete, this);
    RES.addEventListener(RES.ResourceEvent.GROUP_COMPLETE, this.onResourceLoadComplete, this);
    RES.addEventListener(RES.ResourceEvent.GROUP_LOAD_ERROR, this.onResourceLoadError, this);
    RES.addEventListener(RES.ResourceEvent.GROUP_PROGRESS, this.onResourceProgress, this);
    RES.loadGroup("figure");

第一行移除了对 ```CONFIG_COMPLETE``` 事件的侦听，这是一个推荐做法，因为我们不再需要加载配置文件，该侦听也就没有作用了。及时移除事件侦听可以删除不必要的引用，使得不再需要使用的对象能被垃圾回收及时清理，避免内存泄露。

接着，加入了对资源组事件的侦听。

首先是对资源组加载完成的侦听，这是必须的，因为程序的流程需要从这里进行，即程序需要在某种资源加载完成后进行预期的后续流程。 另外，任何加载都需要稳定的网络，而网络出现各种中断是很常见的情况，所以需要添加对加载错误事件的侦听，以在这种情况作出相应的处理，通常是重新加载或者是提示用户检查网络。 可选的，可以加入对加载进度的侦听，通常是通过某种样式的进度条显示给用户当前进度，这在所加载的内容需要耗时较长时对于用户体验非常重要。

对于加载错误和进度的侦听处理，我们这里不做过多说明。

在加载完成的处理，即 ```onResourceLoadComplete``` 中，通过检查当前加载完成的资源组名称，来做对应的处理。确认当前加载的资源组是 ```figure``` 后，便进入程序的正式流程 ```createGameScene``` 中。


###显示图片
Bitmap 类表示用于显示位图图片的显示对象。利用 ```Bitmap()``` 构造函数，可以创建包含对 ```BitmapData``` 对象引用的 ```Bitmap``` 对象。

创建了 ```Bitmap``` 对象后，使用父级 ```DisplayObjectContainer``` 实例的 ```addChild()``` 或 ```addChildAt()``` 方法可以将位图放在显示列表中。

一个 ```Bitmap``` 对象可在若干 ```Bitmap``` 对象之中共享其 ```texture``` 引用，与缩放或旋转属性无关。由于能够创建引用相同 ```texture``` 对象的多个 ```Bitmap``` 对象，因此，多个显示对象可以使用相同的 ```texture``` 对象，而不会因为每个显示对象实例使用一个 ```texture``` 对象而产生额外内存开销。

    // 火炮兰
    var Pohwaran: egret.Bitmap = new egret.Bitmap(RES.getRes("figure_01"));
    Pohwaran.x = -40;
    Pohwaran.y = 20;
    this.addChild(Pohwaran);
    
    // 德玛 德玛西亚之力·盖伦
    var Garen: egret.Bitmap = new egret.Bitmap(RES.getRes("figure_02"));
    Garen.x = 40;
    Garen.y = 20;
    this.addChild(Garen);
    
    // 男枪 法外狂徒·格雷福斯
    var Graves : egret.Bitmap = new egret.Bitmap(RES.getRes("figure_03"));
    Graves .x = 120;
    Graves .y = 20;
    this.addChild(Graves);
    
    // 南小鸟
    var Minami: egret.Bitmap = new egret.Bitmap(RES.getRes("figure_04"));
    Minami.x = 200;
    Minami.y = 20;
    this.addChild(Minami);
    
    // 绫波丽
    var Ayanami: egret.Bitmap = new egret.Bitmap(RES.getRes("figure_05"));
    Ayanami.x = 280;
    Ayanami.y = 20;
    this.addChild(Ayanami);
    
    // 小南
    var Konan: egret.Bitmap = new egret.Bitmap(RES.getRes("figure_06"));
    Konan.x = 360;
    Konan.y = 20;
    this.addChild(Konan);

显示所需的图片，在Egret对应的类就是 ```Bitmap```。使用 ```Bitmap``` 创建一个图片时，在其构造函数中传入RES载入的资源，这里取得的是一个图片的资源，图片资源通过 ```getRes``` 获得的将是一个 ```Texture``` 对象。

###显示深度控制
####获取显示深度
    this.getChildIndex();

具体代码：

    console.log(
        "display indexes:",
        this.getChildIndex(bg),
        this.getChildIndex(Pohwaran),
        this.getChildIndex(Garen),
        this.getChildIndex(Graves),
        this.getChildIndex(Minami),
        this.getChildIndex(Ayanami),
        this.getChildIndex(Konan)
    );
    
    // display indexes: 0 1 2 3 4 5 6

####修改显示深度
    this.setChildIndex( <x>, <深度数值(最大值是显示列表长度-1)> );

具体代码：

    this.setChildIndex(Graves,this.getChildIndex(Garen));
    
    // display indexes: 0 1 3 2 4 5 6

显示深度的规则：

1. 某一个显示深度只能对应一个显示对象，一个显示对象也只能有一个显示深度。

2. 显示深度总是从零开始连续的，当某个深度位置的显示对象被设置为其他深度时，原来的深度会自动被紧邻的比其深度值大1位置的显示对象占据，后续深度位置的显示对象会依次往前排。

3. 某一容器内的显示列表的深度最大值是显示列表长度-1。

####交换显示深度
    this.swapChildren( <x>, <y> );

具体代码：

    this.swapChildren(Ayanami,Konan);
    
    // display indexes: 0 1 3 2 4 6 5

####不可逾越的显示深度最大值
    this.setChildIndex( captain, <比显示列表长度大就可以> );

具体看代码：

    this.setChildIndex(Graves,10);
    
    // display indexes: 0 1 2 6 3 5 4

会发现深度并没有变成10，而是自动取允许的最大值6。
> 这是引擎自动处理的，也算是一种容错功能吧。

###Tween动画效果
所谓Tween动画，就是设计某种属性（比如位置、透明度和缩放）的两个不同状态，然后在给定的时间内从一个状态平滑过渡到另外一个状态。

####认识锚点
锚点用另一个易于理解的词来说，就是定位点。因此锚点是只存在于显示对象的概念。并且锚点是对 __显示对象自身__ 设置的。

以显示对象本身的左上角作为原点的，取值就是具体的像素值。使用显示对象属性 ```anchorOffsetX``` 和 ```anchorOffsetY``` 来设置坐标值锚点。

    Konan.anchorOffsetX = 30;
    Konan.anchorOffsetY = 40;

设置锚点后，我们还需要根据锚点的偏移修改坐标值，以使绿巨人还保持原先的显示位置:

    Konan.x += 30;
    Konan.y += 40;


####设计并实现一组Tween动画
类内部建立记录次数变量

    private times:number;

点击次数控制的代码：

    this.times = -1;
        var self = this;
        this.stage.addEventListener(egret.TouchEvent.TOUCH_TAP,function() {
            switch(++self.times % 5) {
                case 0:
                    egret.Tween.get(Pohwaran).to({ x: Graves.x },300,egret.Ease.circIn);
                    egret.Tween.get(Graves).to({ x: Pohwaran.x },300,egret.Ease.circIn);
                    break;
                case 1:
                    egret.Tween.get(Garen).to({ alpha: .3 },300,egret.Ease.circIn).to({ alpha: 1 },300,egret.Ease.circIn);
                    break;
                case 2:
                    egret.Tween.get(Minami).to({ scaleX: .4,scaleY: .4 },500,egret.Ease.circIn).to({ scaleX: 1,scaleY: 1 },500,egret.Ease.circIn);
                    break;
                case 3:
                    egret.Tween.get(Ayanami).to({ y: Ayanami.y + 40 },500,egret.Ease.circIn).to({ y: Ayanami.y},500,egret.Ease.circIn);
                    break;
                case 4:
                    egret.Tween.get(Konan).to({ x: Konan.x + 40 },500,egret.Ease.circIn).to({ x: Konan.x },500,egret.Ease.circIn);
                    break;
            }
        },this);

* ```Tween.get``` 传入需要对其进行动画控制的对象，并返回一个 ```Tween``` 对象。

* ```to``` 设置 ```Tween``` 对象的动画。to方法包含三个参数:

    - 第一个参数是动画目标属性组，这个参数可以对目标对象本身的各项属性进行设定，就是动画结束时的状态，可以设定一个或多个属性。
        + ```x/y``` 位置
        + ```alpha``` 透明度
        + ```scaleX/scaleY``` 缩放因数

    - 第二个参数是动画时间，以毫秒计。

    - 第三个参数是补间方程，即对动画区间内每个时间点的属性值设定分布。在 ```egret.Ease``` 已经提供了丰富的补间方程，可以根据自己的喜好选择。

###加入声音
    var b_sound: egret.Sound = RES.getRes("bgMusic");
    
    var b_channel: egret.SoundChannel = b_sound.play(0,1);

成了一个 ```sound``` 对象并调用 ```sound``` 的 ```play``` 方法，其中的第一个参数 0 表示播放的开始时间，第二个参数表示播放次数，如果将第二个参数设置为负数将循环播放。

```play``` 方法返回了一个 ```SoundChannel``` 对象。通过操作 ```channnel``` 对象可以控制声音的音量大小停止播放等。

###常规网络通讯
在游戏开发项目中，数据的通讯无疑是不可或缺的因素。看看网络通讯的基本用法。

####URLRequest
```URLRequest``` 类封装了进行HTTP请求所需要的所有信息。 常用的HTTP请求有 ```GET/POST``` 两种类型。当进行HTTP请求时，可以直接在 ```URLRequest``` 实例上设置请求类型和实际数据。

HTTP请求首先需要URL，我们准备了一个专用于测试的URL，其返回当前浏览器的代理信息： ```http://httpbin.org/user-agent```

使用URLRequest类，就要创建其实例，通常在构造函数中传入URL即可：

    var urlreq:egret.URLRequest = new egret.URLRequest( "http://httpbin.org/user-agent" );

####URLLoader
```URLRequest``` 只是一个信息集合，实际通讯需要使用 ```URLLoader```。 ```URLLoader``` 必须使用一个 ```URLRequest``` 实例来发挥作用，并且为了得到返回结果，需要加一个事件监听，代码如下：

    var urlloader:egret.URLLoader = new egret.URLLoader();
    urlloader.addEventListener( egret.Event.COMPLETE, function( evt:egret.Event ):void{
        console.log(evt.target.data);
    }, this );
    urlloader.load( urlreq );


####使用WebSocket通讯
众所周知，WebSocket为Web应用提供了更高效的通讯方式。 本节介绍WebSocket的基本用法。

确保项目支持WebSocket
从Egret1.5.0开始，以官方扩展模块的形式支持WebSocket。在现有的Egret项目中，修改egretProperties.json中的”modules”，添加”socket”模块：

    {
        "name": "socket"
    }

> 注意 添加模块的时候要注意保证 json 的语法正确。

在项目所在目录内执行一次引擎编译：

    egret build -e

本步骤已经完成，现在项目中既可以使用WebSocket相关的API了。

#####WebSocket客户端用法
所有的通讯都是基于一个WebSocket实例，首先创建WebSocket对象。
首先看基本代码。

    private webSocket:egret.WebSocket;
        private createGameScene():void { this.webSocket = new egret.WebSocket();
        this.webSocket.addEventListener(egret.ProgressEvent.SOCKET_DATA, this.onReceiveMessage, this);
        this.webSocket.addEventListener(egret.Event.CONNECT, this.onSocketOpen, this);
        this.webSocket.connect("echo.websocket.org", 80);
    }

WebSocket对象主要有两个事件，一个是连接服务器成功，另一个是收到服务器数据。在正常的网络交互中，这两个事件都是要必须侦听的。

加入侦听事件后，即可连接服务器。注意像所有的通讯协议一样，服务器需要支持WebSocket协议，为便于测试，WebSocket官方提供了一个专用于测试的服务器 ```echo.websocket.org``` ，连接其80端口即可测试。

在连接成功后，即可发送消息，消息的具体格式都是根据情况自己定义的，通常是json格式，便于解析。当然可以自定义其他的字符串格式：

    private onSocketOpen():void {
        var cmd = "Hello Egret WebSocket";
        console.log("The connection is successful, send data: " + cmd);
        this.webSocket.writeUTF(cmd);
    }

服务器根据约定的格式给客户端发送消息，则会触发 ```SOCKET_DATA``` 事件，在其事件处理函数 ```onReceiveMessage``` 中即可读取消息，读取到字符串后，即可根据约定的格式解析：

    private onReceiveMessage(e:egret.Event):void {
        var msg = this.webSocket.readUTF();
        console.log("Receive data:" + msg);
    }

编译运行，没有错误的话，控制台将会输出如下log信息：

    The connection is successful, send data: Hello Egret WebSocket
    Receive data: Hello Egret WebSocket
