---
# title: Write your first Flutter app on the web
title: 编写你的第一个 Flutter 网页应用
# description: How to create a Flutter web app.
description: 如何创建一个 Flutter 网页应用。
# short-title: Write your first web app
short-title: 编写你的第一个网页应用
tags: Flutter安装,Flutter起步教程
keywords: Flutter Web应用,DartPad,在线教程,零基础
js:
  - defer: true
    url: /assets/js/inject_dartpad.js
---

<?code-excerpt path-base="get-started/codelab_web"?>

:::tip

This codelab walks you through writing
your first Flutter app on the web, specifically.
You might prefer to try
[another codelab][first_flutter_codelab]
that takes a more generic approach.
Note that the codelab on this page
does work on mobile and desktop
once you download and configure the appropriate tooling.

这个 codelab 将引导你初步体验 Flutter 网页应用开发。
当然你可能更想去尝试 [编写你的第一个 Flutter 应用][first_flutter_codelab]。
需要注意的是，在一切工具顺利安装的基础上，
本页面上的 codelab 将可以在移动端和桌面端的网页浏览器里运行。

:::

<img src="/assets/images/docs/get-started/sign-up.webp" alt="The web app that you'll be building." class='site-image-right'>

This is a guide to creating your first Flutter **web** app.
If you are familiar with object-oriented programming,
and concepts such as variables, loops, and conditionals,
you can complete this tutorial.
You don't need previous experience with Dart,
mobile, or web programming.

本教程可以帮助你你完成第一个 Flutter Web 应用，
如果你熟悉面对对象、变量、循环以及条件判断等概念，就可以完成本教程，
而无需要 Dart、移动开发和 Web 开发经验。

## What you'll build {:.no_toc}

## 内容概览 {:.no_toc}

You'll implement a simple web app that displays a sign in screen.
The screen contains three text fields:  first name,
last name, and username. As the user fills out the fields,
a progress bar animates along the top of the sign in area.
When all three fields are filled in, the progress bar displays
in green along the full width of the sign in area,
and the **Sign up** button becomes enabled.
Clicking the **Sign up** button causes a welcome screen
to animate in from the bottom of the screen.

你将实现一个只显示登录页面的简单 Web 应用，
这个页面包含了三个文本输入框：名字、姓氏和用户名。
当用户向输入框输入内容时，在登录区域顶部显示一个进度条动画效果。
当用户完成输入时，绿色的进度条将会跟随着充满整个登录区域的顶部，
而且 **Sign up** 按钮状态变成可点击，
点击 **Sign up** 按钮从屏幕下方弹出一个欢迎页面。

The animated GIF shows how the app works at the completion of this lab.

右侧的动图展示了完成该教程后程序的运行效果。

:::secondary 你将学到以下内容
<!-- What you'll learn -->

* How to write a Flutter app that looks natural on the web.

  如何使用 Flutter 构建一个原始的 Web 程序。

* Basic structure of a Flutter app.

  Flutter 程序的基本结构。

* How to implement a Tween animation.

  如何实现一个补间 (Tween) 动画。

* How to implement a stateful widget.

  如何实现一个有状态 (Stateful) widget 。

* How to use the debugger to set breakpoints.

  如何使用断点调试程序。

:::

:::secondary 你将用到
<!-- What you'll use -->

You need three pieces of software to complete this lab:

  我们需要下面三个软件来实现该教程：

  * [Flutter SDK][]
  * [Chrome browser][]

    [Chrome 浏览器][Chrome browser]

  * [Text editor or IDE][editor]

    [文本编辑器 或 IDE][editor]

While developing, run your web app in Chrome,
so you can debug with Dart DevTools 
(also call Flutter DevTools).

在开发过程中，你需要将你的应用在 Chrome 浏览器中运行，
以便使用 Dart DevTools 进行调试
（也可使用 Flutter DevTools）。

:::

## Step 0: Get the starter web app

## 第 0 步: 创建初始化 Web 应用

You'll start with a simple web app that we provide for you.

你将从我们为你提供的简单 Web 应用开始学习。

<ol>
<li>Enable web development.<br>
启用 Web 开发。<br>

At the command line, perform the following command to
make sure that you have Flutter installed correctly.

在命令行观察输出内容，你应该可以看到如下类似的内容，
说明 Flutter 安装的没问题：

```console
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, {{site.appnow.flutter}}, on macOS darwin-arm64, locale en)
[✓] Android toolchain - develop for Android devices (Android SDK version {{site.appnow.android_sdk}})
[✓] Xcode - develop for iOS and macOS (Xcode {{site.appnow.xcode}})
[✓] Chrome - develop for the web
[✓] Android Studio (version {{site.appnow.android_studio}})
[✓] VS Code (version {{site.appnow.vscode}})
[✓] Connected device (4 available)
[✓] HTTP Host Availability

• No issues found!
```

If you see "flutter: command not found",
then make sure that you have installed the
[Flutter SDK][] and that it's in your path.

如果你看到提示是 "flutter: command not found"，
那么就需要确保 [Flutter SDK][] 已经正确地安装，
并且在环境变量中做好了配置。

It's okay if the Android toolchain, Android Studio,
and the Xcode tools aren't installed,
since the app is intended for the web only.
If you later want this app to work on mobile,
you'll need to do additional installation and setup.

如上所示，显示我们缺少 Android 工具、Android Studio 和 Xcode，
如果我们只用于 Web 开发，这些都不是必要的。
后续如果你想用于移动端开发，你将需要安装配置这些工具。

</li>

<li>

List the devices.<br>
To ensure that web _is_ installed,
list the devices available.
You should see something like the following:

查询设备列表。<br>
通过查询设备列表来验证已支持 Web 开发。
你将看到如下的类似内容：

```console
$ flutter devices
4 connected devices:

sdk gphone64 arm64 (mobile) • emulator-5554                        •
android-arm64  • Android 13 (API 33) (emulator)
iPhone 14 Pro Max (mobile)  • 45A72BE1-2D4E-4202-9BB3-D6AE2601BEF8 • ios
• com.apple.CoreSimulator.SimRuntime.iOS-16-0 (simulator)
macOS (desktop)             • macos                                •
darwin-arm64   • macOS 12.6 21G115 darwin-arm64
Chrome (web)                • chrome                               •
web-javascript • Google Chrome 105.0.5195.125
```

The **Chrome** device automatically starts Chrome and enables the use
of the Flutter DevTools tooling.

**Chrome** 浏览器会自动启动并启用 Flutter 开发者工具。

</li>

<li>

The starting app is displayed in the following DartPad.

运行程序将在 DartPad 中显示。

<?code-excerpt "lib/starter.dart" remove="prefer_final_fields"?>
```dartpad title="Flutter beginning getting started hands-on example in DartPad" run="true"
import 'package:flutter/material.dart';

void main() => runApp(const SignUpApp());

class SignUpApp extends StatelessWidget {
  const SignUpApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(routes: {'/': (context) => const SignUpScreen()});
  }
}

class SignUpScreen extends StatelessWidget {
  const SignUpScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[200],
      body: const Center(
        child: SizedBox(width: 400, child: Card(child: SignUpForm())),
      ),
    );
  }
}

class SignUpForm extends StatefulWidget {
  const SignUpForm({super.key});

  @override
  State<SignUpForm> createState() => _SignUpFormState();
}

class _SignUpFormState extends State<SignUpForm> {
  final _firstNameTextController = TextEditingController();
  final _lastNameTextController = TextEditingController();
  final _usernameTextController = TextEditingController();

  double _formProgress = 0;

  @override
  Widget build(BuildContext context) {
    return Form(
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          LinearProgressIndicator(value: _formProgress),
          Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _firstNameTextController,
              decoration: const InputDecoration(hintText: 'First name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _lastNameTextController,
              decoration: const InputDecoration(hintText: 'Last name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _usernameTextController,
              decoration: const InputDecoration(hintText: 'Username'),
            ),
          ),
          TextButton(
            style: ButtonStyle(
              foregroundColor: WidgetStateProperty.resolveWith((states) {
                return states.contains(WidgetState.disabled)
                    ? null
                    : Colors.white;
              }),
              backgroundColor: WidgetStateProperty.resolveWith((states) {
                return states.contains(WidgetState.disabled)
                    ? null
                    : Colors.blue;
              }),
            ),
            onPressed: null,
            child: const Text('Sign up'),
          ),
        ],
      ),
    );
  }
}
```

:::important

This page uses an embedded version of [DartPad][]
to display examples and exercises.
If you see empty boxes instead of DartPads,
go to the [DartPad troubleshooting page][].

本页面使用了嵌入式 [DartPad] 来显示和练习示例。
如果你看到的是空白页面，请转到 [DartPad troubleshooting page][]

:::

</li>

<li>

Run the example.<br>
Click the **Run** button to run the example.
Note that you can type into the text fields,
but the **Sign up** button is disabled.

运行代码示例。<br>
点击 **Run** 按钮来运行示例代码。
你就可以在文本框中输入内容，但是 **Sign up** 按钮是禁用状态的。

</li>

<li>

Copy the code.<br>
Click the clipboard icon in the upper right of the
code pane to copy the Dart code to your clipboard.

复制代码。<br>
点击代码区域右上角的复制图标复制 Dart 代码。

</li>

<li>

Create a new Flutter project.<br>
From your IDE, editor, or at the command line,
[create a new Flutter project][] and name it `signin_example`.

创建一个新的 Flutter 工程。<br>
使用 IDE、编辑器或者命令行，创建一个名称为 `signin_example` 的新项目，
更多内容可以参考文档 [Flutter 开发体验初探][create a new Flutter project]。

</li>

<li>

Replace the contents of `lib/main.dart`
with the contents of the clipboard.

使用上面我们复制的内容替换 `lib/main.dart` 文件的内容。

</li>
</ol>

### Observations {:.no_toc}

### 观察和分析 {:.no_toc}

* The entire code for this example lives in the
  `lib/main.dart` file.

  完整的示例代码都位于 `lib/main.dart` 文件中。

* If you know Java, the Dart language should feel very familiar.

  如果你了解 Java ，那 Dart 也会给你一种熟悉的感觉。

* All of the app's UI is created in Dart code.
  For more information, see [Introduction to declarative UI][].

  应用程序的所有的 UI 的都是通过 Dart 构建的。
  你可以通过文档
  [声明式 UI 介绍][Introduction to declarative UI] 了解到更多的信息。

* The app's UI adheres to [Material Design][],
  a visual design language that runs on any device or platform.
  You can customize the Material Design widgets,
  but if you prefer something else,
  Flutter also offers the Cupertino widget library,
  which implements the current iOS design language.
  Or you can create your own custom widget library.

  应用的 UI 遵循 [Material Design][] 的设计规范，
  这是一种在任何设备和平台都可以运行的可视化设计语言。
  而且你也有其他选择，Flutter 也提供了一款 iOS 设计风格的 Cupertino widget 库。
  当然你也可以创建自己的自定义 widget 库。

* In Flutter, almost everything is a [Widget][].
  Even the app itself is a widget.
  The app's UI can be described as a widget tree.

  在 Flutter 的世界，万物皆 [Widget][]，甚至连应用本身都是 widget。
  应用的 UI 可以看作为 widget 树。

## Step 1: Show the Welcome screen

## 第 1 步：显示欢迎页面

The `SignUpForm` class is a stateful widget.
This simply means that the widget stores information
that can change, such as user input, or data from a feed.
Since widgets themselves are immutable
(can't be modified once created),
Flutter stores state information in a companion class,
called the `State` class. In this lab,
all of your edits will be made to the private
`_SignUpFormState` class.

`SignUpForm` 类是一个 Stateful widget。这代表着 widget 的存储信息可动态改变，
例如用户输入，或者传递的数据。由于 widget 本身是不可变的（一旦创建不可修改），
所有 Flutter 的状态信息存储在一种叫 `State` 的附加类中。
在这个代码示例中，所有的编辑将在一个 `_SignUpFormState` 的私有类中实现。

:::tip 有趣的事
<!-- Fun fact -->

The Dart compiler enforces privacy for any identifier
prefixed with an underscore. For more information,
see the [Effective Dart Style Guide][].

Dart 编译器会将任何带有下划线前缀标识的视为私有。可查阅
[Dart 文档 —— 高效 Dart 语言指南：代码风格][Effective Dart Style Guide]
获取更多信息。

:::

First, in your `lib/main.dart` file,
add the following class definition for the
`WelcomeScreen` widget after the `SignUpScreen` class:

首先，在 `lib/main.dart` 文件中，在 `SignUpScreen` 类后面
添加下面 `WelcomeScreen` widget 的定义类：

<?code-excerpt "lib/step1.dart (welcome-screen)"?>
```dart
class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Welcome!',
          style: Theme.of(context).textTheme.displayMedium,
        ),
      ),
    );
  }
}
```

Next, you will enable the button to display the screen
and create a method to display it.

接下来，你需要创建一个显示方法，然后使用按钮通过方法控制页面的显示。

<ol>

<li>

Locate the `build()` method for the
`_SignUpFormState` class. This is the part of the code
that builds the SignUp button.
Notice how the button is defined:
It's a `TextButton` with a blue background,
white text that says **Sign up** and, when pressed,
does nothing.

找到 `_SignUpFormState` 类的 `build()` 方法。
这部分代码是用来构建注册按钮的。注意，按钮是如何定义：
它是一个背景为蓝色， **Sign up** 文本为白色的 `TextButton` 按钮，
当我们点击它时，并未执行任何操作。

</li>

<li>

Update the `onPressed` property.<br>
Change the `onPressed` property to call the (non-existent)
method that will display the welcome screen.

修改按钮的 `onPressed` 属性。<br>
将按钮的 `onPressed` 属性改为调用显示欢迎页面的方法（该方法在下一步创建）。

Change `onPressed: null` to the following:

将 `onPressed: null` 改为以下内容：

<?code-excerpt "lib/step1.dart (on-pressed)"?>
```dart
onPressed: _showWelcomeScreen,
```

</li>

<li>

Add the `_showWelcomeScreen` method.<br>
Fix the error reported by the analyzer that `_showWelcomeScreen`
is not defined. Directly above the `build()` method,
add the following function:

新增 `_showWelcomeScreen` 方法。<br>
修复上述代码导致的编译器提示错误：
`_showWelcomeScreen` is not defined.
（未定义 `_showWelcomeScreen`）。
在 `build()` 方法上方添加下面的方法：

<?code-excerpt "lib/step1.dart (show-welcome-screen)"?>
```dart
void _showWelcomeScreen() {
  Navigator.of(context).pushNamed('/welcome');
}
```

</li>

<li>

Add the `/welcome` route.<br>
Create the connection to show the new screen.
In the `build()` method for `SignUpApp`,
add the following route below `'/'`:

添加 `/welcome` 页面路由。<br>
为新的页面添加跳转路由。在 `SignUpApp` 类的 `build()` 方法中，
在 `'/'` 下面添加如下路由：

<?code-excerpt "lib/step1.dart (welcome-route)"?>
```dart
'/welcome': (context) => const WelcomeScreen(),
```

</li>

<li>

Run the app.<br>
The **Sign up** button should now be enabled.
Click it to bring up the welcome screen.
Note how it animates in from the bottom.
You get that behavior for free.

运行该应用程序。<br>
**Sign up** 按钮现在应该可以点击了。
单击注册按钮跳转到欢迎页面。
注意，欢迎页面显示是有一个从底部弹出的动画。
你可以很简单的实现它。

</li>

</ol>

### Observations {:.no_toc}

### 观察和分析 {:.no_toc}

* The `_showWelcomeScreen()` function is used in the `build()`
  method as a callback function. Callback functions are often
  used in Dart code and, in this case, this means
  "call this method when the button is pressed".

  `_showWelcomeScreen()` 函数被当成回调函数在 `build()` 方法中被调用。
  在 Dart 中你会经常使用回调函数，在这里意味着“点击按钮时调用该方法”。

* The `const` keyword in front of the constructor is very
  important. When Flutter encounters a constant widget, it
  short-circuits most of the rebuilding work under the hood
  making the rendering more efficient.

  构造函数前面的 `const` 关键字至关重要，
  当 Flutter 遇到一个静态 widget 时，
  它就会缩短引擎下的大部分重建工作，从而提高渲染效率。

* Flutter has only one `Navigator` object.
  This widget manages Flutter's screens
  (also called _routes_ or _pages_) inside a stack.
  The screen at the top of the stack is the view that
  is currently displayed. Pushing a new screen to this
  stack switches the display to that new screen.
  This is why the `_showWelcomeScreen` function pushes
  the `WelcomeScreen` onto the Navigator's stack.
  The user clicks the button and, voila,
  the welcome screen appears. Likewise,
  calling `pop()` on the `Navigator` returns to the
  previous screen. Because Flutter's navigation is
  integrated into the browser's navigation,
  this happens implicitly when clicking the browser's
  back arrow button.

  Flutter 中仅存在一个 `Navigator` 对象。
  这个 widget 用来管理 Flutter 堆栈中的页面
  （也可以被称为路由 (**routes**) 或者页面管理器 (**pages**)）。
  当前显示的页面是堆栈中最上面的页面，
  通过往堆栈中 push 新的页面来切换新的页面。
  这也是 `_showWelcomeScreen` 函数向
  `Navigator` 堆栈中添加 `WelcomeScreen` 页面的原因。
  用户点击按钮，然后出现欢迎页面。
  同样，可以通过调用 `Navigator` 的 `pop()` 方法来返回上一个页面。
  因为 Flutter 的 navigation 已经集成到浏览器的导航中，
  所以当点击浏览器的返回箭头也会返回到上一个页面。

## Step 2: Enable sign in progress tracking

## 第 2 步： 实现输入进度监听

This sign in screen has three fields.
Next, you will enable the ability to track the
user's progress on filling in the form fields,
and update the app's UI when the form is complete.

在这个页面有三个文本框。
下一步，我们将实现监听用户输入表单的进度，
并且在表单完成后更新应用的 UI 。

:::note

This example does **not** validate the accuracy of the user input.
That is something you can add later using form validation, if you like.

这个简单的示例并未对用户的输入进行准确性验证。
如果需要，你可以后面自己添加表单验证。

:::

<ol>
<li>

Add a method to update `_formProgress`.
In the `_SignUpFormState` class, add a new method called
`_updateFormProgress()`:

添加一个用于更新进度 `_formProgress` 属性的方法。
在 `_SignUpFormState` 类，添加一个名为 `_updateFormProgress()` 的新方法：

<?code-excerpt "lib/step2.dart (update-form-progress)"?>
```dart
void _updateFormProgress() {
  var progress = 0.0;
  final controllers = [
    _firstNameTextController,
    _lastNameTextController,
    _usernameTextController,
  ];

  for (final controller in controllers) {
    if (controller.value.text.isNotEmpty) {
      progress += 1 / controllers.length;
    }
  }

  setState(() {
    _formProgress = progress;
  });
}
```

This method updates the `_formProgress` field based on
the number of non-empty text fields.

这个方法根据非空输入框的数量来更新 `_formProgress` 属性。

</li>

<li>

Call `_updateFormProgress` when the form changes.<br>
In the `build()` method of the `_SignUpFormState` class,
add a callback to the `Form` widget's `onChanged` argument.
Add the code below marked as NEW:

表单改变时调用 `_updateFormProgress` 方法。<br>
在 `_SignUpFormState` 类的 `build()` 方法中，
为 `Form` widget 的 `onChanged` 参数添加回调函数。
注意注释为 NEW 的那行新添加的代码：

<?code-excerpt "lib/step2.dart (on-changed)"?>
```dart
return Form(
  onChanged: _updateFormProgress, // NEW
  child: Column(
```

</li>

<li>

Update the `onPressed` property (again).<br>
In `step 1`, you modified the `onPressed` property for the
**Sign up** button to display the welcome screen.
Now, update that button to display the welcome
screen only when the form is completely filled in:

再次更改按钮的 `onPressed` 属性。<br>
还记得我们在第一步中，我们通过修改 `onPressed` 属性实现了
点击 **Sign up** 按钮跳转到欢迎页面吗？
现在，将它改成只有完成表单输入时才可以点击按钮跳转到欢迎页面。

<?code-excerpt "lib/step2.dart (on-pressed)"?>
```dart
TextButton(
  style: ButtonStyle(
    foregroundColor: WidgetStateProperty.resolveWith((states) {
      return states.contains(WidgetState.disabled)
          ? null
          : Colors.white;
    }),
    backgroundColor: WidgetStateProperty.resolveWith((states) {
      return states.contains(WidgetState.disabled)
          ? null
          : Colors.blue;
    }),
  ),
  onPressed:
      _formProgress == 1 ? _showWelcomeScreen : null, // UPDATED
  child: const Text('Sign up'),
),
```

</li>

<li>

Run the app.<br>
The **Sign up** button is initially disabled,
but becomes enabled when all three text fields contain
(any) text.

运行应用。<br>
刚打开页面时 **Sign up** 按钮是禁用状态，
当为三个字段输入内容（任意内容）时将会变成可点击状态。

</li>
</ol>

### Observations {:.no_toc}

### 观察和分析 {:.no_toc}

* Calling a widget's `setState()` method tells Flutter that the
  widget needs to be updated on screen.
  The framework then disposes of the previous immutable widget
  (and its children), creates a new one
  (with its accompanying child widget tree),
  and renders it to screen. For this to work seamlessly,
  Flutter needs to be fast.
  The new widget tree must be created and rendered to screen
  in less than 1/60th of a second to create a smooth visual
  transition—especially for an animation.
  Luckily Flutter _is_ fast.

  调用 widget 的 `setState()` 方法通知 Flutter 页面上的 widget 需要重新构建。
  框架将销毁之前的不可变 widget （上面说过 widget 一旦创建不可更改）（包含它的子级 widget） ，
  然后创建一个新的 widget （包含他的子级 widget 树）并将新的 widget 渲染到页面上。
  为了使应用运行顺畅， Flutter 需要快速的销毁和创建 widget。
  新创建的 widget 必须在不到 1/60 秒的时间渲染到页面上，才能创建一个流畅的动画效果。
  幸运的是 Flutter 就是**这么快**。当然如果你愿意的话，也可以使用文本编辑器。

* The `progress` field is defined as a floating value,
  and is updated in the `_updateFormProgress` method.
  When all three fields are filled in, `_formProgress` is set to 1.0.
  When `_formProgress` is set to 1.0, the `onPressed` callback is set to the
  `_showWelcomeScreen` method. Now that its `onPressed` argument is non-null, the button is enabled.
  Like most Material Design buttons in Flutter,
  [TextButton][]s are disabled by default if their `onPressed` and `onLongPress` callbacks are null.
  `_showWelcomeScreen` method. The button is enabled when it's `onPressed`
  argument is non-null.

  `progress` 属性定义为浮点值，并在 `_updateFormProgress` 方法中更新。
  当三个输入框都被输入后， `_formProgress` 设置为 1.0 。
  当 `_formProgress` 设置为 1.0 后，
  `onPressed` 的回调函数将设置为 `_showWelcomeScreen` 方法。
  当 `onPressed` 参数变为非空时按钮将会变成可点击。
  所有的 [TextButton][] 在 `onPressed` 和 `onLongPress` 回调为空时，默认也是无法点击的，
  与 Flutter 中其他 Material Design 的按钮一致。

* Notice that the `_updateFormProgress` passes a function to `setState()`.
  This is called an anonymous
  function and has the following syntax:

  请注意, `_updateFormProgress` 是通过传递一个函数调用 `setState()` 。
  这种被称为匿名函数,语法如下所示:

  ```dart
  methodName(() {...});
  ```
  
  Where `methodName` is a named function that takes an anonymous
  callback function as an argument.

  名为 `methodName` 的函数把匿名回调函数作为参数。

* The Dart syntax in the last step that displays the
  welcome screen is:

  最后一步显示欢迎页面的 Dart 语法如下所示:

  <?code-excerpt "lib/step2.dart (ternary)" replace="/, \/\/ UPDATED//g"?>
  ```dart
  _formProgress == 1 ? _showWelcomeScreen : null
  ```
  This is a Dart conditional assignment and has the syntax:
  `condition ? expression1 : expression2`.
  If the expression `_formProgress == 1` is true, the entire expression results
  in the value on the left hand side of the `:`, which is the
  `_showWelcomeScreen` method in this case.

  Dart 三目运算语法如下: `condition ? expression1 : expression2` 。
  如果 `_formProgress == 1` 是正确的，则会取 `:` 左侧的值，在这个示例中会取
  `_showWelcomeScreen` 方法。

## Step 2.5: Launch Dart DevTools

## 第 2.5 步：启动 Dart 开发者工具

How do you debug a Flutter web app?
It's not too different from debugging any Flutter app.
You want to use [Dart DevTools][]!
(Not to be confused with Chrome DevTools.)

如何调试 Flutter Web 应用？所有的 Flutter 应用调试方法没有很大的区别。
你应该使用 [Dart DevTools][]！（不要和 Chrome 开发者工具搞混淆了）

Our app currently has no bugs, but let's check it out anyway.
The following instructions for launching DevTools applies to any workflow,
but there is a shortcut if you're using IntelliJ.
See the tip at the end of this section for more information.

虽然我们的应用现在没有 bug ，但是我们依然来验证一下。
下面的指引讲明了 DevTools 使用的场景，
如果你使用的是 IntelliJ 编辑器则会有更好的方式。
可以通过查看文档末尾的提示信息获取更多的信息。

<ol>
<li>

Run the app.<br>
If your app isn't currently running, launch it.
Select the **Chrome** device from the pull down
and launch it from your IDE or,
from the command line, use `flutter run -d chrome`.

运行应用。<br>
如果应用未启动，启动应用。
从下拉选项中选择 **Chrome** 设备然后使用 IDE 启动，
或者在命令行中使用 `flutter run -d chrome`。

</li>

<li>

Get the web socket info for DevTools.<br>
At the command line, or in the IDE,
you should see a message stating something like the following:

获取开发者工具 （DevTools） 的 socket 信息。<br>
在命令行或者 IDE 中你应该可以看下如下所示内容的信息：

```console
Launching lib/main.dart on Chrome in debug mode...
Building application for the web...                                11.7s
Attempting to connect to browser instance..
Debug service listening on <b>ws://127.0.0.1:54998/pJqWWxNv92s=</b>
```

Copy the address of the debug service, shown in bold.
You will need that to launch DevTools.

复制粗体显示的调试服务的地址，你可以用这个地址启动 DevTools 。

</li>

<li>

Ensure that the Dart and Flutter plugins are installed.<br>
If you are using an IDE,
make sure you have the Flutter and Dart plugins set up,
as described in the [VS Code][] and
[Android Studio and IntelliJ][] pages.
If you are working at the command line,
launch the DevTools server as explained in the
[DevTools command line][] page.

确认开发工具已被安装。<br>
你是否 [已经安装 DevTools 了呢][DevTools installed]？
如果你使用的是编辑器 (IDE) ，先确认已经用 [VS Code][] 和
 [Android Studio and IntelliJ][] 文档描述的方式安装 Flutter
和 Dart 插件。如果你使用的是命令行的方式，用 [DevTools command line][]
文档说明的方式启动开发者工具服务 （DevTools server）。

</li>

<li>

Connect to DevTools.<br>
When DevTools launches, you should see something
like the following:

连接到 DevTools。<br>
当 DevTools 启动时，你应该会看到如下类似的内容：

```console
Serving DevTools at http://127.0.0.1:9100
```

Go to this URL in a Chrome browser. You should see the DevTools
launch screen. It should look like the following:

在 Chrome 浏览器中打开上面 URL，你应该可以看到 DevTools 运行页面。
如下所示：

![Screenshot of the DevTools launch screen](/assets/images/docs/get-started/devtools-launch-screen.png){:width="100%"}

</li>

<li>

Connect to running app.<br>
Under **Connect to a running site**,
paste the web socket (ws) location that you copied in step 2,
and click **Connect**. You should now see Dart DevTools
running successfully in your Chrome browser:

连接到运行的应用。<br>
在 **Connect to a running site** 下面粘贴你在上面第 2 步中复制的 Web Socket (ws) 地址，
然后点击 **Connect**。现在你应该可以看到 Dart DevTools 成功地运行在你的 Chrome 浏览器中，
如下所示：

![Screenshot of DevTools running screen](/assets/images/docs/get-started/devtools-running.png){:width="100%"}

Congratulations, you are now running Dart DevTools!

恭喜，你已经成功运行 Dart 开发者工具！

</li>
</ol>

:::note

This is not the only way to launch DevTools.
If you are using IntelliJ,
you can open DevTools by going to
**Flutter Inspector** -> **More Actions** -> **Open DevTools**:

这不是启动开发者工具的唯一方式。
如果你使用的是 IntelliJ 编辑器，
你可以通过 **Flutter Inspector** -> **More Actions** -> **Open DevTools**
的方式启动开发者工具，如下所示：

![Screenshot of Flutter inspector with DevTools menu](/assets/images/docs/get-started/intellij-devtools.png){:width="100%"}

:::

<ol>
<li>

Set a breakpoint.<br>
Now that you have DevTools running,
select the **Debugger** tab in the blue bar along the top.
The debugger pane appears and, in the lower left,
see a list of libraries used in the example.
Select `lib/main.dart` to display your Dart code
in the center pane.

设置断点。<br>
现在你以前启动了开发者工具，在上面的蓝色工具栏中选择 **Debugger** 选项。
在左下角出现调试面板，可以查看示例中使用的类库。
选择 `lib/main.dart` 将在页面中间显示 Dart 代码。

![Screenshot of the DevTools debugger](/assets/images/docs/get-started/devtools-debugging.png){:width="100%"}

</li>

<li>

Set a breakpoint.<br>
In the Dart code,
scroll down to where `progress` is updated:

设置断点。<br>
在 Dart 代码中，向下拉找到被修改的 `progress`，如下所示：

<?code-excerpt "lib/step2.dart (for-loop)"?>
```dart
for (final controller in controllers) {
  if (controller.value.text.isNotEmpty) {
    progress += 1 / controllers.length;
  }
}
```

Place a breakpoint on the line with the for loop by clicking to the
left of the line number. The breakpoint now appears
in the **Breakpoints** section to the left of the window.

在 for 循环行的行数前面单击设置断点。
这个断点将显示在窗口左侧的 **Breakpoints** 栏中。

</li>

<li>

Trigger the breakpoint.<br>
In the running app, click one of the text fields to gain focus.
The app hits the breakpoint and pauses.
In the DevTools screen, you can see on the left
the value of `progress`, which is 0. This is to be expected,
since none of the fields are filled in.
Step through the for loop to see
the program execution.

触发断点。<br>
在正在运行的应用中，点击任意一个输入框获取焦点。
应用会遇到断点并暂停。
在开发者工具页面，你可以在左侧看到 `progress` 的值是 0 。
这是正常的，因为你没有输入任何内容，
遍历 for 循环观察应用的运行。

</li>

<li>

Resume the app.<br>
Resume the app by clicking the green **Resume**
button in the DevTools window.

恢复应用程序。<br>
在开发者工具窗口点击绿色的 **Resume** 按钮来恢复应用程序。

</li>

<li>

Delete the breakpoint.<br>
Delete the breakpoint by clicking it again, and resume the app.

删除断点。<br>
再次点击断点来删除断点和恢复程序。

</li>
</ol>

This gives you a tiny glimpse of what is possible using DevTools,
but there is lots more! For more information,
see the [DevTools documentation][].

这里只是粗略的介绍开发者工具的使用方式，还有更多没有讲到。
请参考 [DevTools 文档][DevTools documentation] 学习更多的内容。


## Step 3: Add animation for sign in progress

## 第3步：为输入进度添加动画效果

It's time to add animation! In this final step,
you'll create the animation for the
`LinearProgressIndicator` at the top of the sign in
area. The animation has the following behavior:

是时候添加动画效果了！在最后一步，我们将在登录区域上方创建一个进度条动画，
特效如下所述：

* When the app starts,
  a tiny red bar appears across the top of the sign in area.

  刚启动时，登录区域的顶部显示一条红色的进度条。

* When one text field contains text,
  the red bar turns orange and animates 0.15
  of the way across the sign in area.

  当一个文本框被键入内容时，进度条从红色变成橙色，
  并且进度条前进到距登录区域顶部 1/3 的位置。

* When two text fields contain text,
  the orange bar turns yellow and animates half
  of the way across the sign in area.

  当第二个文本框被键入内容时，进度条从橙色变为黄色，
  并且进度条前进到距登录区域顶部 2/3 的位置。

* When all three text fields contain text,
  the orange bar turns green and animates all the
  way across the sign in area.
  Also, the **Sign up** button becomes enabled.

  当三个文本框全部被输入内容时，进度条从橙色变成绿色，并且逐渐充满整个登录区域顶部。
  除此之外， **Sign up** 按钮的状态也变成可点击。

<ol>
<li>

Add an `AnimatedProgressIndicator`.<br>
At the bottom of the file, add this widget:

添加进度条动画效果 (`AnimatedProgressIndicator`)<br>
在文件的下面，添加下面的 widget：

<?code-excerpt "lib/step3.dart (animated-progress-indicator)"?>
```dart
class AnimatedProgressIndicator extends StatefulWidget {
  final double value;

  const AnimatedProgressIndicator({super.key, required this.value});

  @override
  State<AnimatedProgressIndicator> createState() {
    return _AnimatedProgressIndicatorState();
  }
}

class _AnimatedProgressIndicatorState extends State<AnimatedProgressIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Color?> _colorAnimation;
  late Animation<double> _curveAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );

    final colorTween = TweenSequence([
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.red, end: Colors.orange),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.orange, end: Colors.yellow),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.yellow, end: Colors.green),
        weight: 1,
      ),
    ]);

    _colorAnimation = _controller.drive(colorTween);
    _curveAnimation = _controller.drive(CurveTween(curve: Curves.easeIn));
  }

  @override
  void didUpdateWidget(AnimatedProgressIndicator oldWidget) {
    super.didUpdateWidget(oldWidget);
    _controller.animateTo(widget.value);
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder:
          (context, child) => LinearProgressIndicator(
            value: _curveAnimation.value,
            valueColor: _colorAnimation,
            backgroundColor: _colorAnimation.value?.withValues(alpha: 0.4),
          ),
    );
  }
}
```

The [`didUpdateWidget`][] function updates
the `AnimatedProgressIndicatorState` whenever
`AnimatedProgressIndicator` changes.

[`didUpdateWidget`][] 方法会在 `AnimatedProgressIndicator` 变化时更新
`AnimatedProgressIndicatorState`。

</li>

<li>

Use the new `AnimatedProgressIndicator`.<br>
Then, replace the `LinearProgressIndicator` in the `Form`
with this new `AnimatedProgressIndicator`:

使用新的进度条。<br>
然后，使用新的 `AnimatedProgressIndicator` widget 替换表单中的 `LinearProgressIndicator`
 widget，如下所示：

<?code-excerpt "lib/step3.dart (use-animated-progress-indicator)"?>
```dart
child: Column(
  mainAxisSize: MainAxisSize.min,
  children: [
    AnimatedProgressIndicator(value: _formProgress), // NEW
    Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
    Padding(
```

This widget uses an `AnimatedBuilder` to animate the
progress indicator to the latest value.

该 widget 使用 `AnimatedBuilder` 为最新值实现了进度的动画显示。

</li>

<li>

Run the app.<br>
Type anything into the three fields to verify that
the animation works, and that clicking the
**Sign up** button brings up the **Welcome** screen.

运行应用。<br>
在三个输入框中输入任意值来验证动画效果是否正常显示，
然后点击 **Sign up** 按钮将弹出欢迎页面。

</li>
</ol>

### Complete sample

### 完整的示例

<?code-excerpt "lib/main.dart"?>
```dartpad title="Flutter complete getting started hands-on example in DartPad" run="true"
import 'package:flutter/material.dart';

void main() => runApp(const SignUpApp());

class SignUpApp extends StatelessWidget {
  const SignUpApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      routes: {
        '/': (context) => const SignUpScreen(),
        '/welcome': (context) => const WelcomeScreen(),
      },
    );
  }
}

class SignUpScreen extends StatelessWidget {
  const SignUpScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[200],
      body: const Center(
        child: SizedBox(width: 400, child: Card(child: SignUpForm())),
      ),
    );
  }
}

class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text(
          'Welcome!',
          style: Theme.of(context).textTheme.displayMedium,
        ),
      ),
    );
  }
}

class SignUpForm extends StatefulWidget {
  const SignUpForm({super.key});

  @override
  State<SignUpForm> createState() => _SignUpFormState();
}

class _SignUpFormState extends State<SignUpForm> {
  final _firstNameTextController = TextEditingController();
  final _lastNameTextController = TextEditingController();
  final _usernameTextController = TextEditingController();

  double _formProgress = 0;

  void _updateFormProgress() {
    var progress = 0.0;
    final controllers = [
      _firstNameTextController,
      _lastNameTextController,
      _usernameTextController,
    ];

    for (final controller in controllers) {
      if (controller.value.text.isNotEmpty) {
        progress += 1 / controllers.length;
      }
    }

    setState(() {
      _formProgress = progress;
    });
  }

  void _showWelcomeScreen() {
    Navigator.of(context).pushNamed('/welcome');
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      onChanged: _updateFormProgress,
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          AnimatedProgressIndicator(value: _formProgress),
          Text('Sign up', style: Theme.of(context).textTheme.headlineMedium),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _firstNameTextController,
              decoration: const InputDecoration(hintText: 'First name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _lastNameTextController,
              decoration: const InputDecoration(hintText: 'Last name'),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8),
            child: TextFormField(
              controller: _usernameTextController,
              decoration: const InputDecoration(hintText: 'Username'),
            ),
          ),
          TextButton(
            style: ButtonStyle(
              foregroundColor: WidgetStateProperty.resolveWith((states) {
                return states.contains(WidgetState.disabled)
                    ? null
                    : Colors.white;
              }),
              backgroundColor: WidgetStateProperty.resolveWith((states) {
                return states.contains(WidgetState.disabled)
                    ? null
                    : Colors.blue;
              }),
            ),
            onPressed: _formProgress == 1 ? _showWelcomeScreen : null,
            child: const Text('Sign up'),
          ),
        ],
      ),
    );
  }
}

class AnimatedProgressIndicator extends StatefulWidget {
  final double value;

  const AnimatedProgressIndicator({super.key, required this.value});

  @override
  State<AnimatedProgressIndicator> createState() {
    return _AnimatedProgressIndicatorState();
  }
}

class _AnimatedProgressIndicatorState extends State<AnimatedProgressIndicator>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<Color?> _colorAnimation;
  late Animation<double> _curveAnimation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(milliseconds: 1200),
      vsync: this,
    );

    final colorTween = TweenSequence([
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.red, end: Colors.orange),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.orange, end: Colors.yellow),
        weight: 1,
      ),
      TweenSequenceItem(
        tween: ColorTween(begin: Colors.yellow, end: Colors.green),
        weight: 1,
      ),
    ]);

    _colorAnimation = _controller.drive(colorTween);
    _curveAnimation = _controller.drive(CurveTween(curve: Curves.easeIn));
  }

  @override
  void didUpdateWidget(AnimatedProgressIndicator oldWidget) {
    super.didUpdateWidget(oldWidget);
    _controller.animateTo(widget.value);
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder:
          (context, child) => LinearProgressIndicator(
            value: _curveAnimation.value,
            valueColor: _colorAnimation,
            backgroundColor: _colorAnimation.value?.withValues(alpha: 0.4),
          ),
    );
  }
}
```

### Observations {:.no_toc}

### 观察和分析 {:.no_toc}

* You can use an `AnimationController` to run any animation.

  你可以使用 `AnimationController` 控制任何动画效果。

* `AnimatedBuilder` rebuilds the widget tree when the value
  of an `Animation` changes.

  当 `Animation` 的值改变时 `AnimatedBuilder` 将重新构建 widget 树。

* Using a `Tween`, you can interpolate between almost any value,
  in this case, `Color`.

  使用动画 `Tween` ，你还可以使用很多值，像这个示例中的 `Color`。

## What next?

## 下一步，我们该做什么？

Congratulations!
You have created your first web app using Flutter!

恭喜！
你已经使用 Flutter 创建了第一个 Web 应用！

If you'd like to continue playing with this example,
perhaps you could add form validation.
For advice on how to do this,
see the [Building a form with validation][]
recipe in the [Flutter cookbook][].

如果你想继续完善这个示例，或许你可以添加表单验证。
如何继续的建议，请参考 [Flutter cookbook][] 中的
[Building a form with validation][]

For more information on Flutter web apps,
Dart DevTools, or Flutter animations, see the following:

有关 Web 应用、Dart 开发者工具以及 Flutter 动画的更多信息，
请参考下面文档：

* [Animation docs][]
* [Dart DevTools][]
* [Implicit animations][] codelab
* [Web samples][]

[Android Studio and IntelliJ]: /tools/devtools/android-studio
[Animation docs]: /ui/animations
[Building a form with validation]: /cookbook/forms/validation
[Chrome browser]: https://www.google.com/chrome/?brand=CHBD&gclid=CjwKCAiAws7uBRAkEiwAMlbZjlVMZCxJDGAHjoSpoI_3z_HczSbgbMka5c9Z521R89cDoBM3zAluJRoCdCEQAvD_BwE&gclsrc=aw.ds
[create a new Flutter project]: /reference/create-new-app
[Dart DevTools]: /tools/devtools
[DartPad]: {{site.dartpad}}
[DevTools command line]: /tools/devtools/cli
[DevTools documentation]: /tools/devtools
[DevTools installed]: /tools/devtools#start
[DartPad troubleshooting page]: {{site.dart-site}}/tools/dartpad/troubleshoot
[`didUpdateWidget`]: {{site.api}}/flutter/widgets/State/didUpdateWidget.html
[editor]: /get-started/editor
[Effective Dart Style Guide]: {{site.dart-site}}/guides/language/effective-dart/style#dont-use-a-leading-underscore-for-identifiers-that-arent-private
[Flutter cookbook]: /cookbook
[Flutter SDK]: /get-started/install
[Implicit animations]: /codelabs/implicit-animations
[Introduction to declarative UI]: /get-started/flutter-for/declarative
[Material Design]: {{site.material}}/get-started
[TextButton]: {{site.api}}/flutter/material/TextButton-class.html
[VS Code]: /tools/devtools/vscode
[Web samples]: {{site.repo.samples}}/tree/main/web
[Widget]: {{site.api}}/flutter/widgets/Widget-class.html
[first_flutter_codelab]: /get-started/codelab
