---
# title: Handle changes to a text field
title: 响应文本框内容的更改
# description: How to detect changes to a text field.
description: 如何检测文本框内容的更改。
tags: cookbook, 实用教程, 表格交互
keywords: 文本框,传值
js:
  - defer: true
    url: /assets/js/inject_dartpad.js
---

<?code-excerpt path-base="cookbook/forms/text_field_changes/"?>

In some cases, it's useful to run a callback function every time the text
in a text field changes. For example, you might want to build a search
screen with autocomplete functionality where you want to update the
results as the user types.

在某些情境中，我们可能需要在每次文本框的文本内容变化时都调用回调函数。
例如，当构建一个有自动填充功能的搜索页面时，
我们希望根据用户输入的内容来更新返回的结果。

How do you run a callback function every time the text changes?
With Flutter, you have two options:

那么如何每次在文本内容改变时调用回调函数呢？在Flutter中，我们提供了两种选择：

  1. Supply an `onChanged()` callback to a `TextField` or a `TextFormField`.

     给 `TextField` 或 `TextFormField` 绑定 `onChanged()` 回调

  2. Use a `TextEditingController`.

     使用 `TextEditingController`

## 1. Supply an `onChanged()` callback to a `TextField` or a `TextFormField`

## 1. 给 `TextField` 或 `TextFormField` 绑定 `onChanged()` 回调

The simplest approach is to supply an [`onChanged()`][] callback to a
[`TextField`][] or a [`TextFormField`][].
Whenever the text changes, the callback is invoked.

最简单的方法是给 [`TextField`][] 绑定 [`onChanged()`][] 回调。
每当文本内容改变时，回调函数会被触发。

In this example, print the current value and length of the text field 
to the console every time the text changes.

在下面的示例中，每次 text 的值改变，会在控制台中打印出当前文本框的值。

It's important to use [characters][] when dealing with user input,
as text may contain complex characters.
This ensures that every character is counted correctly
as they appear to the user.

在处理用户的输入内容时，很重要的一点是要使用 [characters][]，
因为用户的输入内容可能会包含复杂的字符。
characters 会保证每一个显示到用户字符都被计数到。

<?code-excerpt "lib/main.dart (TextField1)"?>
```dart
TextField(
  onChanged: (text) {
    print('First text field: $text (${text.characters.length})');
  },
),
```

## 2. Use a `TextEditingController`

## 2. 使用 `TextEditingController`

A more powerful, but more elaborate approach, is to supply a
[`TextEditingController`][] as the [`controller`][]
property of the `TextField` or a `TextFormField`.

另外一种更强大但是更复杂的方法是绑定 [`TextEditingController`][] 作为 `TextField` 和
 `TextFormField` 的 [`controller`][] 属性。

To be notified when the text changes, listen to the controller
using the [`addListener()`][] method using the following steps:

你可以通过如下步骤，使用 [`addListener()`][] 方法来监听控制，
实现在文本更改时收到通知：

  1. Create a `TextEditingController`.

     创建一个 `TextEditingController`

  2. Connect the `TextEditingController` to a text field.

     将 `TextEditingController` 绑定到 text field

  3. Create a function to print the latest value.

     创建一个函数来打印最新值

  4. Listen to the controller for changes.

     监听控制器的变化
    

### Create a `TextEditingController`

### 创建一个 `TextEditingController`

Create a `TextEditingController`:

创建一个 `TextEditingController`：


<?code-excerpt "lib/main_step1.dart (Step1)" remove="return Container();"?>
```dart
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller. Later, use it to retrieve the
  // current value of the TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is removed from the
    // widget tree.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

:::note

Remember to dispose of the `TextEditingController` when it's no
longer needed. This ensures that you discard any resources used
by the object.

请在 `TextEditingController` 使用完毕时将其 `dispose` ，
从而确保所有被这个对象所使用的资源被释放。

:::


### Connect the `TextEditingController` to a text field

### 给 text field 绑定 `TextEditingController`

Supply the `TextEditingController` to either a `TextField`
or a `TextFormField`. Once you wire these two classes together,
you can begin listening for changes to the text field.

`TextEditingController` 必须绑定到 `TextField` 或者是 `TextFormField` 才能被正常的使用。
一旦绑定，就能够开始监听文本框的变化。

<?code-excerpt "lib/main.dart (TextField2)"?>
```dart
TextField(controller: myController),
```

### Create a function to print the latest value

### 创建一个打印当前值的方法

You need a function to run every time the text changes.
Create a method in the `_MyCustomFormState` class that prints
out the current value of the text field.

现在，我们需要一个每当表单项变化都会运行的函数。
在下面的示例中，我们会在 `_MyCustomFormState` 类中创建一个方法，
实现打印出文本框当前值。


<?code-excerpt "lib/main.dart (printLatestValue)"?>
```dart
void _printLatestValue() {
  final text = myController.text;
  print('Second text field: $text (${text.characters.length})');
}
```

### Listen to the controller for changes

### 监听控制器的变化

Finally, listen to the `TextEditingController` and call the
`_printLatestValue()` method when the text changes. Use the
[`addListener()`][] method for this purpose.

最后，需要监听 `TextEditingController` 并且在 text 值变化时运行
`_printLatestValue()` 方法。
我们需要使用 [`addListener()`][] 方法来实现这个功能。

Begin listening for changes when the
`_MyCustomFormState` class is initialized,
and stop listening when the `_MyCustomFormState` is disposed.

下面的示例会在类 `_MyCustomFormState` 初始化的时候开始监听变化，dispose 时停止监听。

<?code-excerpt "lib/main.dart (init-state)"?>
```dart
@override
void initState() {
  super.initState();

  // Start listening to changes.
  myController.addListener(_printLatestValue);
}
```

<?code-excerpt "lib/main.dart (dispose)"?>
```dart
@override
void dispose() {
  // Clean up the controller when the widget is removed from the widget tree.
  // This also removes the _printLatestValue listener.
  myController.dispose();
  super.dispose();
}
```

## Interactive example

## 交互式样例

<?code-excerpt "lib/main.dart"?>
```dartpad title="Flutter text field change hands-on example in DartPad" run="true"
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: 'Retrieve Text Input',
      home: MyCustomForm(),
    );
  }
}

// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller and use it to retrieve the current value
  // of the TextField.
  final myController = TextEditingController();

  @override
  void initState() {
    super.initState();

    // Start listening to changes.
    myController.addListener(_printLatestValue);
  }

  @override
  void dispose() {
    // Clean up the controller when the widget is removed from the widget tree.
    // This also removes the _printLatestValue listener.
    myController.dispose();
    super.dispose();
  }

  void _printLatestValue() {
    final text = myController.text;
    print('Second text field: $text (${text.characters.length})');
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Retrieve Text Input')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              onChanged: (text) {
                print('First text field: $text (${text.characters.length})');
              },
            ),
            TextField(controller: myController),
          ],
        ),
      ),
    );
  }
}
```

[`addListener()`]: {{site.api}}/flutter/foundation/ChangeNotifier/addListener.html
[`controller`]: {{site.api}}/flutter/material/TextField/controller.html
[`onChanged()`]: {{site.api}}/flutter/material/TextField/onChanged.html
[`TextField`]: {{site.api}}/flutter/material/TextField-class.html
[`TextEditingController`]: {{site.api}}/flutter/widgets/TextEditingController-class.html
[`TextFormField`]: {{site.api}}/flutter/material/TextFormField-class.html
[characters]: {{site.pub}}/packages/characters
