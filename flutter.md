# Flutter <!-- .element: class="class="r-fit-text" -->



## 跨平台开发的三个时代

- Hybrid <!-- .element: class="fragment fade-up" -->
- React Native/Weex <!-- .element: class="fragment fade-up" -->
- Flutter <!-- .element: class="fragment fade-up" -->

Note:
1、Cordova/Ionic.

2、采用类 Web 标准进行开发，但在运行时把绘制和渲染交由原生系统接管的技术

3、自带渲染引擎，客户端仅提供一块画布即可获得从业务逻辑到功能呈现的多端高度一致的渲染体验



## Hybrid

![web容器](https://cdn.jsdelivr.net/gh/floiges/pics/img/web.webp)


## web 页面显示的基础流程

- 加载 -> 解析 -> 渲染

- 一边加载，一边解析，一边渲染 <!-- .element: class="fragment fade-up" -->

- 性能消耗要比原生开发增加 N 个数量级 <!-- .element: class="fragment fade-up" -->

Note:
1、浏览器控件加载 HTML5 页面的 HTML 主文档

2、加载过程中遇到外部 CSS 文件，浏览器另外发出一个请求，来获取 CSS 文件

3、遇到图片资源，浏览器也会另外发出一个请求，来获取图片资源。这是异步请求，并不会影响 HTML 文档的加载

4、加载过程中遇到 JavaScript 文件，由于 JavaScript 代码可能会修改 DOM 树，因此 HTML 文档会挂起渲染（加载解析渲染同步）的线程，直到 JavaScript 文件加载解析并执行完毕，才可以恢复 HTML 文档的渲染线程

5、JavaScript 代码中有用到 CSS 文件中的属性样式，于是阻塞，等待 CSS 加载完毕才能恢复执行



## React Native

![RN容器](https://cdn.jsdelivr.net/gh/floiges/pics/img/RN.webp)

Note:
1、JS 开发

2、原生接管 UI 绘制，Bridge 桥梁

3、非 web 标准，自定义 DSL，天猫 VitualView

4、框架本身需要处理大量平台相关的逻辑外，随着系统版本变化和 API 的变化，我们还需要处理不同平台的原生控件渲染能力差异，修复各类奇奇怪怪的 Bug。始终需要 Follow Native 的思维方式，就使得泛 Web 容器框架的跨平台特性被大打折扣



## Flutter

![Flutter容器](https://cdn.jsdelivr.net/gh/floiges/pics/img/Flutter.webp)

Note:
- Flutter 只关心如何向 GPU 提供视图数据，而 Skia 就是它向 GPU 提供视图数据的好帮手

- Skia 引擎会将使用 Dart（JIT、AOT） 构建的抽象的视图结构数据加工成 GPU 数据，交由 OpenGL 最终提供给 GPU 渲染

- Skia 保证了同一套代码调用在 Android 和 iOS 平台上的渲染效果是完全一致的


## 绘制流程

![Flutter绘制](https://cdn.jsdelivr.net/gh/floiges/pics/img/flutter-render.webp)


## Dart

- JIT 开发周期端，调试方便 (支持有状态的热重载)
- AOT 本地代码的执行更高效，代码性能和用户体验也更卓越

Note:
- 为什么不选择 JS？Dart 语言组在隔壁，好沟通


## Flutter 架构

![Flutter架构](https://cdn.jsdelivr.net/gh/floiges/pics/img/Flutter-arch.webp)



## 如何抉择

![distinct](https://cdn.jsdelivr.net/gh/floiges/pics/img/web-distinct.webp)

Note:
- Hybrid：跨平台开发历史最成功的例子
- RN/Flutter 两个框架都已达到了大面积商业应用的标准。
- 综合成熟度和生态，目前俩看 React Native 略胜 Flutter。
- 因此，如果是中短期项目的话，建议使用 React Native。
- 但作为技术选型，我们要看得更远一些。Flutter 的设计理念比较先进，解决方案也相对彻底，在渲染能力的一致性以及性能上，和 React Native 相比优势非常明显
- 状态管理
- 函数式编程
- Flex 布局
- 核心

