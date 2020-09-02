# 学习笔记

## 2.1 项目结构概述

* 一个小程序项目通常包括：
  * JSON配置文件，配置小程序的页面文件路径、界面表现、网络超时时间、底部tab等
  * WXML模板文件，描述小程序的页面结构
  * WXSS样式文件，描述页面的样式
  * JavaScript文件，用于实现页面中的各种逻辑交互功能
  * 图片文件，用来作为小程序的logo、背景图片、按钮icon等
  * 其他类型，由于小程序项目有最大存储空间限制（几兆字节），通常不会直接将音频、视频放在项目中，而是在小程序运行时通过网络请求
* 目录结构：
  * 除了最外出目录中需有：app.js、app.json、app.wxss和project.config.json四个文件以外，其他目录结构和文件路径都可以由开发者自己设计
    * project.config.json文件是项目配置文件，其中保存了项目名称、AppID等配置信息。通常不要直接修改这个文件，通常在开发工具右上角“详情”中打开项目配置面板，来修改项目配置并自动更新到project.config.json
    * app.js描述小程序的全局逻辑，例如：保存小程序全局数据，在小程序启动或退出后执行某种操作
    * app.wxss描述小程序全局样式，在这个文件中设置的样式会在所有页面中都生效
    * app.json负责小程序的全局配置，可以设置各个页面路径、页面的窗口表现、网络超时时间等
  * 一个小程序页面由四个类型文件组成：JS、JSON、WXML、WXSS，分别负责页面逻辑、页面配置、页面结构、页面样式
    * JS文件只能描述页面自身的逻辑，例如：负责保存页面独有的数据，监听页面中的单击事件，在进入或退出页面时执行某种操作
    * WXSS文件设置的样式只会在该页面中生效
    * JSON文件只能对该页面进行配置，只能设置该页面的窗口表现
  * 编程中util这个词很常见，它是工具的意思。开发者习惯于创建一个这样的目录，用来定义一些会被重复多次用到的代码段
  * img目录，通常用来保存项目中需要用到的图片文件

## 2.2 JSON文件——小程序的配置

* 在JSON中可以有一下几种数据类型
  * number：整数、小数、正数、负数
  * boolean：true、false
  * string：单引号或双引号括起来的任意文本
  * null：只有一个取值null，表示什么都没有
  * Array：JSON中的数组可以包含任意数据类型，元素是有序的
  * Object：一组由key-value组成的无序集合。JSON中的对象，key是一个字符串类型的数据，value可以是任意类型的数据
  * HexColor：` # `号开头的六个十六进制数，每2个数一组，分别代表红、绿、蓝三种颜色的亮度
* 全局配置
  * 在小程序中，每一个配置文件都是一个JSON对象，对象中的每个属性表示一个配置项
  * 全局配置中所有的属性都由自己的默认配置，因此如果在配置文件中没有特别声明，这些属性都会使用默认配置
  * pages项：配置string[]类型，描述页面路径列表
  * window项：配置Object类型，描述全局的默认窗口表现
  * tabBar项：配置Object类型，描述底部tab栏的表现
  * networkTimeout项：配置Object类型，描述网络超时时间
  * debug项：配置boolean类型，描述是否开启debug模式，默认关闭
  * functionalPage项：配置boolean类型，描述是否启用插件功能页，默认关闭
  * subpackages项：配置Object[]类型，描述分包结构配置
  * workers项：配置string类型，描述Worker代码放置的目录，多线程相关
  * requiredBackgroundModes项：配置string[]类型，描述需要在后台使用的能力，如音乐播放
  * plugins项：配置Object类型，描述使用到的插件
  * preloadRule项：配置Object类型，描述分包预下载规则
  * resizable项：配置boolean类型，描述iPad上是否支持屏幕旋转，默认关闭
  * navigateToMiniProgramAppIdList项：配置string[]类型，描述可以跳转的小程序列表
  * usingComponents项，配置Object类型，描述全局自定义组件配置
  * permission项：配置Object类型，描述小程序接口权限设置
  * string[]类型表示每个元素都是string类型的数组
* 低版本兼容问题：
  * 方法一：在小程序账号后台设置能让小程序运行的微信最低版本（基础库最低版本设置）。如果用户使用的微信低于该版本，打开小程序时会要求用户升级微信
  * 方法二：在小程序中通过逻辑判断用户的微信版本，对于低版本和高版本分别处理
* 个别全局配置属性详解：
  * pages属性：
    * 一个数组，数组每个元素都表示一个页面的路径。页面所在的目录名称可以和页面名字不同，但是一个页面的四种类型文件必须具有相同的文件名
    * pages属性中声明的第一个页面是小程序的首页
  * window属性：
    * navigationBarBackgroundColor：导航栏背景色
    * navigationBarTextStyle：导航栏标题文字颜色，仅支持black和white两个值
    * navigationBarTitleText：导航栏标题文字内容
    * navigationStyle：导航栏样式，仅支持default和custom两个值。defaut时为每个页面添加一个默认导航栏。custom时默认导航栏只会保留右上角的胶囊按钮，其余部分隐藏，且navigationBarBackgroundColor、navigationBarTextStyle、navigationBarTitleText三个属性失效，只能靠开发者通过编写WXML、WXSS去开发导航栏内容
    * backgroundColor：页面的背景颜色
    * backgroundTextStyle：下拉loading的样式，仅支持dark和light两个值
    * backgroundColorTop：顶部窗口的背景色，仅iOS支持
    * backgroundColorBottom：底部窗口的背景色，仅iOS支持
    * enablePullDownRefresh：是否开启下拉刷新
    * onReachBottomDistance：页面上拉触底事件触发时距页面底部距离，单位为px（像素）
    * pageOrientation：屏幕旋转设置，支持auto、portrait、landscape
  * tabBar属性：
    * color：tab上的文字默认的颜色
    * selectedColor：tab上文字选中时的颜色
    * backgroundColor：tab的背景颜色
    * borderStyle：tabBar上边框的颜色，仅支持black和white两个取值
    * list：tab的列表，最少两个、最多五个tab。list属性是一个数组，数组中每项元素时一个JSON对象，每个对象代表tab栏中的一个按钮
      * pagePath：tab按钮对应的页面路径，必须在pages中先定义
      * text：tab按钮文字
      * iconPath：图片路径，icon大小限制40KB，建议尺寸为81px*81px，不支持网络图片，当position为top时不显示icon
      * selectedIconPath：图片路径，icon大小限制40KB，建议尺寸为81px*81px，不支持网络图片，当position为top时不显示icon
    * position：tabBar的位置，仅支持bottom和top两个取值
    * custom：是否自定义tabBar
  * networkTimeout属性：
    * request：wx.request（HTTP请求）的超时时间
    * connectSocket：wx.connectSocket（WebSocket请求）的超时时间
    * uploadFile：wx.uploadFile（上传文件）的超时时间
    * downloadFile：wx.downloadFile（下载文件）的超时时间
  * debug属性：
    * 设置为true可以开启微信开发者工具的调试模式
  * 页面配置：
    * 小程序规定，在页面单独的配置文件中，只能设置app.json文件中window配置项的内容。页面配置中设置的内容会覆盖全局配置中window属性设置的内容
    * navigationBarBackgroundColor：导航栏背景色
    * navigationBarTextStyle：导航栏标题文字颜色，仅支持black和white两个值
    * navigationBarTitleText：导航栏标题文字内容
    * navigationStyle：导航栏样式，仅支持default和custom两个值。defaut时为每个页面添加一个默认导航栏。custom时默认导航栏只会保留右上角的胶囊按钮，其余部分隐藏，且navigationBarBackgroundColor、navigationBarTextStyle、navigationBarTitleText三个属性失效，只能靠开发者通过编写WXML、WXSS去开发导航栏内容
    * backgroundColor：页面的背景颜色
    * backgroundTextStyle：下拉loading的样式，仅支持dark和light两个值
    * backgroundColorTop：顶部窗口的背景色，仅iOS支持
    * backgroundColorBottom：底部窗口的背景色，仅iOS支持
    * enablePullDownRefresh：是否开启下拉刷新
    * onReachBottomDistance：页面上拉触底事件触发时距页面底部距离，单位为px（像素）
    * pageOrientation：屏幕旋转设置，支持auto、portrait、landscape
    * disableScroll：设置为true则页面整体不能上下滚动
    * disableSwipeBack：禁止页面右滑手势返回
    * usingComponent：页面自定义组件配置

## 2.3 WXML和WXSS文件——小程序的视图

* 每个小程序的页面都可以划分为视图层和逻辑层两部分
  * 视图层：负责描述页面的结构和样式。由WXML、WXSS两个文件共同实现
  * 逻辑层：负责保管页面中的数据和实现交互逻辑
* WXML（WeiXin Markup Language），微信标记语言
  * 只有` <!-- ...... --> `格式的注释
  * 组件是视图层的基本组成单元，每个页面都是由很多个组件组成的。一个组件由开标签、闭标签（有些开、闭标签合并成一个单标签）、属性（用于修饰组件，可以没有或有多个。属性值必须写在双引号内）和内容四部分组成
  * 属性值的引号中加入双大括号后，双大括号中的内容就会按照规则解析成number、string、boolean、Object、Array等类型的值
  * text组件
    * selectable属性：控制文本是否可选
    * space属性：控制是否显示连续空格
      * ensp：允许连续空格，每个空格占中文字符一半大小
      * emsp：允许连续空格，每个空格占中文字符一样大小
      * nbsp：允许连续空格，每个空格的大小与字体设置有关
    * decode属性：控制是否开启转义功能
* WXSS（WeiXin Style Sheets)，微信样式表
  * ` selector { property: value; } `。selector：选择器；每个样式都由一个属性（property）和一个值（value）组成，用英文冒号分开；属性值后用英文分号结尾。
  * 最简单的选择器是标签选择器，也就是用组件的名称作为选择器
  * class属性表示组件的“类”。值string类型，由开发者决定，通常写成有意义的英文单词（多个单词用减号分隔）
  * 类选择器使用class属性值选择页面中的组件，属性值前以一个点“.”号开始
  * 如果样式设置产生了冲突，根据选择器优先级匹配规则选择一个优先级最高的样式：
    * 页面WXSS中的类选择器
    * 全局WXSS中的类选择器
    * 页面WXSS中的标签选择器
    * 全局WXSS中的标签选择器
  * 优先级相同的两个选择器产生冲突，文件中靠后位置设置的样式会覆盖先设置的样式
  * 除标签选择器、类选择器，WXSS还支持ID选择器、属性选择器、后代选择器等
* 容器组件view：
  * view组件在开标签和闭标签中不仅可以加入字符串内容，还可以在内容中包含其他组件
  * 使用容器组件后，WXML代码中的组件开始有了层次关系。如果一个组件包含另外一个组件，通常称包含者是被包含者的“父元素”（或“父节点”），被包含者是包含者的“子元素”（或“子节点”）；如果一个组件包含另外多个组件，被包含的多个组件之间称为“兄弟元素”（或“兄弟节点”）
  * WXML代码中的层次关系对应着页面中的布局：
    * 如果把一个页面当做一个整体，可以对应到WXML中最外层的容器组件。通常一个页面在竖直方向上又可以划分成多个区域，这些区域对应了最外层容器组件的子元素
    * 每个区域又可以划分成更小的子区域，这些子区域可以是纵向排列的，也可以是横向排列的（划分方向由WXSS的样式决定）。这些子区域都对应了更里面一层子元素。
    * 不断划分页面区域，直到划分到不能在细分的文字、图片等基础内容，这就对应到WXML中的text组件、image组件上
* 弹性布局：
  * 为了实现页面的布局效果，仅仅通过WXML的描述是不够的，还需要通过WXSS的样式去控制布局
  * flex布局（弹性布局）的主要思想是给予容器控制内部元素排列方式的能力。任意一个容器都可以指定为flex布局，开启flex布局需要设置组件样式的display属性为flex
  * 采用flex布局的元素称为flex容器，简称容器。它的所有子元素自动成为容器成员。
  * 容器拥有两个隐形的轴
    * 主轴（main axis），默认水平方向
    * 交叉轴（cross axis），默认垂直方向
    * 子元素在主轴的方向上按顺序排列
  * flex布局在container中常用的样式属性：
    * flex-direction，决定了主轴的方向，即子元素排列方向
      * row（默认值）：主轴水平方向，子元素沿主轴从左至右排列
      * column：主轴为竖直方向，子元素沿主轴从上至下排列
      * row-reverse：主轴为水平方向，子元素沿主轴从右至左排列
      * column-reverse：主轴为竖直方向，子元素沿主轴从下至上排列
    * flex-wrap，决定是否换行及换行的方式（默认情况下子元素排列在主轴一条直线上）
      * nowrap（默认值）：自动缩小项目，不换行
      * wrap：换行，且第一行在上方，第二行在第一行下方
      * wrap-reverse：换行，第一行在下方，第二行在第一行上方
    * flex-direction与flex-wrap可以合并简写为：flex-flow
    * justify-content，决定了子元素在主轴上的对齐方式
      * flex-start（默认值）：左对齐（如果主轴是竖直方向则为顶端对齐）
      * flex-end：右对齐（如果主轴是竖直方向则为底部对齐）
      * center：居中对齐
      * space-between：两端对齐
      * space-around：沿轴线均匀分布
    * align-items，决定了子元素在交叉轴上的对齐方式
      * flex-start（默认值）：顶端对齐
      * flex-end：底部对齐
      * center：居中对齐
      * baseline：子元素第一行文字的底部对齐
      * stretch：子元素未设置高度时，子元素将和容器等高对齐
    * align-content，有多根主轴时子元素在交叉轴上的对齐方式（当flex布局设置可以换行时会出现多根主轴，每行子元素在各自的主轴上排列）
      * flex-start：顶端对齐
      * flex-end：底部对齐
      * center：居中对齐
      * space-between：两端对齐
      * space-around：沿轴线均匀分布
      * stretch：各行根据其flex-grow值伸展，以充分占据剩余空间
  * flex布局除了可以设置容器的样式外，还可以在子元素中设置样式，从而让子元素可以更灵活地调整布局
    * order：定义item的排列顺序
    * flex-grow：当有多余空间时，item会放大到充满空间。如果有多个item都设置了这个属性，则这些item以本属性设置的值作为比例放大
    * flex-shrink：当空间不足时，item缩小。如果有多个item都设置了这个属性，则这些item以本属性设置的值作为比例缩小
    * flex-basis：item在主轴上占据的空间
    * align-self：单个item在交叉轴上独特的对齐方式
    * flex-grow、flex-shrink、flex-basis三个属性可以合并简写为flex
* 盒模型
  * 它将WXML中每个组件都看做一个盒子，每个盒子都是由外边距（margin）、边框（border）、内边距（padding）和内容（content)组成。可以对四个方向分别设置，通过属性值后加上：-top、-bottom、-left、-right
  * view组件默认情况下没有外边距、边框和内边距
  * 默认情况下，组件的宽度、高度等于content区域的宽度、高度，这时如果设置padding会使组件所占的区域向外扩展。如果将组件的box-sizing属性设置为border-box，组件的宽度、高度等于content+padding+border，这时为组件增加padding会使content区域缩小
* 块级元素与行内元素
  * 行内（inline）元素，例如text组件，与其他行内元素在一行中并排显示
  * 块级（block）元素，例如view组件，独占一行，不能与其他任何元素并列
  * 行内元素不能设置宽高，默认的宽度就是文字的宽度
  * 块级元素能设置宽高，如果不设置宽高，那么宽度将默认变为父级的100%
  * 更改组件的块级属性或行内属性，可以通过修改组件的display样式属性进行设置
* 尺寸单位
  * 小程序引入了一种新的尺寸单位——rpx（responsive pixel），rpx这种尺寸单位可以根据屏幕宽度进行自适应
  * 小程序规定，所有手机屏幕的宽度都为750rpx
  * 尽管在做样式布局时使用rpx作为尺寸单位非常方便，但是设置文字大小时，通常使用pt（point）作为尺寸单位。pt是一种绝对长度单位，表示一个专用的印刷单位“点”，无论手机屏幕多宽，文字总是显示成相同的大小
* 微信团队在微信开发者工具中提供了一个样式补全功能，如果开启会自动检测并补全样式，尽可能保证WXSS的兼容显示。开启功能修改的是project.config.json中setting属性的postcss设置项，通过开发者工具的“详情”、“上传代码时样式自动补全”选项修改

## 2.4 JS文件——小程序的逻辑

* 小程序的app.js文件实现了小程序App的注册，主要逻辑是调用App函数：` App({}) `，并传入一个对象作为参数
* 传入App函数的对象可以添加生命周期回调函数、错误监听函数、全局变量等属性
  * `onLaunch(options){}`，生命周期函数，小程序打开时执行一次
  * `onShow(options){}`，生命周期函数，小程序打开时和每次小程序切换到前台都会执行一次
  * `onHide(){}`，生命周期函数，每次小程序切换到后台都会执行一次
  * `onError(msg){}`，错误监听函数，每次小程序JS代码报错都会调用一次
* 在JavaScript中，函数本质上也是一种对象，因此可以在Object中写成key-value的形式
```js
{
  key: function 函数名 (参数) {

  }
}
```
* 由于在对象中可以通过key找到value，因此可以将对象中函数的名称去掉
```js
{
  key: function (参数) {

  }
}
```
* 在对象中，函数定义还能进一步简化，将function关键字去掉，将key和value合并
```js
{
  key (参数) {

  }
}
```
* 在App中，除了生命周期和错误监听函数，还可以自定义一些全局的变量或者其他自定义函数。在App函数中，用this关键词可以访问这些自定义的函数和变量
```js
App({
  test: null,
  myFunc() {
    this.text = 123
  },
  onLaunch(options) {
    this.myFunc()
  }
})
```
* 小程序在每个页面的JS文件中实现了页面的注册逻辑，通过调用` Page({}) `
* 传入的对象包括
  * `data: {}`，用来定义页面的初始数据
  * `onLoad(option){}`，生命周期函数，页面打开时执行一次
  * `onShow(){}`，生命周期函数，页面打开时和每次页面切换到前台都会执行一次
  * `onReady(){}`，生命周期函数，页面初次渲染完毕时执行一次
  * `onHide(){}`，生命周期函数，每次页面切换到后台都会执行一次
  * `onUnload(){}`，生命周期函数，页面退出时执行一次
  * `onPullDownRefresh(){}`，监听页面下拉动作，页面下拉时执行一次
  * `onReachBottom(){}`，页面上拉触底事件的处理函数
  * `onShareAppMessage(){}`，设置用户转发页面时的转发标题、路径和预览图片
  * `onPageScroll(){}`，页面滚动事件的处理函数
  * `onResize(){}`，屏幕旋转事件的处理函数
  * `onTabItemTap(item){}`，如果JSON中开启了tab页，单击页面中的tab会触发本函数
* 同样可以在Page注册中自定义一些函数和变量，使用方法与App注册中的自定义函数和变量相同
* Page注册中的data属性，不是用户自定义的属性，而是小程序规定的属性。data属性是一个Object变量，这里的数据在WXML中使用双大括号语法绑定到视图中
* 修改data中的数据，只需要在Page注册的函数中调用this.setData函数，并传入新的对象作为参数。不能直接通过赋值的方式修改data属性中的值。更新后的值将更新页面显示
* WXML组件中增加一个bindtap属性，属性值写成JS中的事件出来函数名字，就可以将事件处理函数与组件的单击事件关联起来
* 如果组件中有以“data-"开头的属性，那么这些属性会通过事件处理函数的参数传入event.target.dataset属性中，key为”data-"后面的字符串，value为属性的值
* 在App注册的函数与Page注册的函数中，可以使用微信提供的小程序API实现很多有用的功能，如获取用户信息、本地存储、实现支付等
* 小程序API本质上是小程序提供的一系列函数，这些函数位于wx对象中。在App注册的函数与Page注册的函数中可以直接获取wx对象
