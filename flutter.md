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

加载 -> 解析 -> 渲染

一边加载，一边解析，一边渲染 <!-- .element: class="fragment fade-up" -->

性能消耗要比原生开发增加 N 个数量级 <!-- .element: class="fragment fade-up" -->

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


## Flutter 架构

![Flutter架构](https://cdn.jsdelivr.net/gh/floiges/pics/img/Flutter-arch.webp)

Note:
- Embedder 操作系统适配层

- Engine 层主要包含 Skia、Dart 和 Text，实现了 Flutter 的渲染引擎、文字排版、事件处理和 Dart 运行时等功能

- Framework 层则是一个用 Dart 实现的 UI SDK，包含了动画、图形绘制和手势识别等功能


## 绘制流程

![Flutter绘制](https://cdn.jsdelivr.net/gh/floiges/pics/img/flutter-render.webp)



## Dart

- JIT 开发周期端，调试方便 (支持有状态的热重载)
- AOT 本地代码的执行更高效，代码性能和用户体验也更卓越
- 类型安全，所有类型都是对象类型，都继承自顶层类型 Object
- num、bool、String、List、Map

Note:
- 为什么不选择 JS？Dart 语言组在隔壁，好沟通
- 继承、接口、mixin



## 类

```
class Point {
  num x, y;
  static num factor = 0;
  //语法糖，等同于在函数体内：this.x = x;this.y = y;
  Point(this.x,this.y);
  void printInfo() => print('($x, $y)');
  static void printZValue() => print('$factor');
}

var p = new Point(100,200); // new 关键字可以省略
p.printInfo();  // 输出(100, 200);
Point.factor = 10;
Point.printZValue(); // 输出10
```



## Dart 不支持多重继承

```
class Coordinate with Point {
}

var yyy = Coordinate();
print (yyy is Point); //true
print(yyy is Coordinate); //true
```



## 可选命名参数、可忽略参数

```
//要达到可选命名参数的用法，那就在定义函数的时候给参数加上 {}
//定义可选命名参数时增加默认值
void enableFlags({bool bold = true, bool hidden = false}) => print("$bold ,$hidden");
//可忽略的参数在函数定义时用[]符号指定
//定义可忽略参数时增加默认值
void enable2Flags(bool bold, [bool hidden = false]) => print("$bold ,$hidden");
//可选命名参数函数调用
enableFlags(); //true, false
//可忽略参数函数调用
enable2Flags(true, false); //true, false
enable2Flags(true); //true, false
```



## Future

```
Future<void> printOrderMessage() async {
  try {
    var order = await fetchUserOrder();
    print('Awaiting user order...');
    print(order);
  } catch (err) {
    print('Caught error: $err');
  }
}

Future<String> fetchUserOrder() {
  // Imagine that this function is more complex.
  var str = Future.delayed(
      Duration(seconds: 4),
      () => throw 'Cannot locate user order');
  return str;
}

Future<void> main() async {
  await printOrderMessage();
}
```



## 一切都是 Widget

Widget，Element 与 RenderObject

Note:
- Widget 不可变，是一份配置数据，可频繁删除、重建

- Element，类似 Virtual DOM，可以只将真正需要修改的部分同步到真实的 RenderObject 树中，最大程度降低对真实渲染视图的修改，提高渲染效率，而不是销毁整个渲染视图树重建

- RenderObject 负责渲染

- Flutter 通过控件树（Widget 树）中的每个控件（Widget）创建不同类型的渲染对象，组成渲染对象树

- JSX->虚拟DOM->浏览器DOM



## 例子


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

