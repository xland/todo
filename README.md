- 我想做个浏览器
  - 我想把顶部的Tab页移动到左侧/右侧
    - 侧边栏底部有设置按钮
  - 顶部只留下地址栏（输入），LOGO（拖拽区域），窗口按钮
  - 我尝试了用Qt去做这个事情
    - Qt鼠标移入关闭按钮，再移入QWebEngine区域，鼠标光标在页面的Input控件上时Cursor样式不会改变
    - 这个问题应该与Qt窗口没有适应屏幕的缩放比例有关
    - 我怀疑这是Qt本身的问题，应该很难解决，所以我放弃了这个方案。
  - 我尝试用WebView2做这个事情
    - 第一个方案：用一个页面做窗口渲染，用户页面覆盖在这个窗口页面上
      - 我发现用户页面始终没办法放到窗口页面之上，也就是WebView2控件的Z序我时控制不了的
    - 第二个方案：窗口元素用TGUI框架做
      - TGUI用的是SFML，这个基于OpenGL的，兼容新有点问题，内存消耗页高，编译的应用倒是小，3.7M
    - 第三个方案：用Skia自己画界面元素
      - 太费劲，产物太大：4.9M左右，内存占用低：34M（我的屏幕），自由度非常高。



- WebView2
- 如何把自绘控件置于WebView2Ctrl之上？地址栏输入的时候是需要有下来菜单的(搞个TopMost的子窗口)
- Spy++ Edge的时候它也就两个创口

- WebUi与QuickJs的绑定可行吗？



- Qt版的浏览器
- Qt版的象棋训练程序
- Qt文件管理器(Everything)
- Qt日历


- 一个基于Markdown的编辑器
- QuickJS
- 





- Skia研究
  - 局部刷新的优越性，没有必要
  - SKSL必学，翻译官方SKSL文档
  - MarkDown示例研究
  - 更多的示例研究
- 基于Skia、Yoga和QuickJs的安装程序制作工具
- 基于Skia、Yoga的FileManager
  - 滚动条控件
- 基于Skia的有道翻译客户端
- 基于Skia的截图
  - 动态创建SurfaceFront？要做吗
  - Skia升级到
- 基于WebView2的浏览器
- 《Electron实战》第二版
- 掘金小册WebView2或Skia