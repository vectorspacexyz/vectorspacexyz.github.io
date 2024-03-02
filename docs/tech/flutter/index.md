---
title: Flutter
comments: true
---
## Stateless Widgets
[https://www.youtube.com/watch?v=wE7khGHVkYY](https://www.youtube.com/watch?v=wE7khGHVkYY)

![1709375729.png](img/1709375729.png)

* Flutter apps have both an **Element tree** and a **Widget tree**.
* What we see on the screen is the Element tree. The Widget tree
  is the blueprint for the element tree.
* `runApp` takes the widget that's passed to it and makes it the root
  of the Widget tree.
* Once the widget is loaded to the root of the widget tree, Flutter
  framework calls the createElement method. This creates an instance
  of Element class that's gonna be the root of the element tree.
 
![1709376567.png](img/1709376567.png)

"A stateless widget is a widget that's composed of children. Which is
why it has a build method. 💡"

* The element tree then checks if it as any children, and calls the
  `build` method of the widget from which it was created.

Look at the widget-tree and the element-tree for the code below.

![1709381681.png](img/1709381681.png)

![1709381873.png](img/1709381873.png)

The widget at the root of the widget tree is the widget that you
defined, here `DogApp`.

## Dart classes
The fields within a dart class are non-nullable unless its stated explicity with
a `?`. The following code is fine:

```dart
class SomeOne {
  String? name;
  int? age;
}
```

But the moment you delete the `?` marks, LSP will complain its a non-nullable
field and it must be initialized. This is important to keep in mind as we model
stuff as dart classes. 😀

But when we declare fields as `final`, its not the null-safety rules, but by
virtue of being final that we gotta intialize the fields.

![1707993187.png](img/1707993187.png)

Since final fields need to be initialized before the class gets instantiated,
they need to initialized BEFORE the constructor gets called. This is where
initializer lists comes in:

![1707993436.png](img/1707993436.png)

But we've gotta a shorthand for initializer list:

### Upate 2024-03-02 10:24AM
I no longer think this can be considered an equivalent to initializer list. The
below code just forces you to create an object of the class specifiying
arguments, whereas with initializer list you can check whether arguments are
null (not passed to the constructor) and set them approrpriate values.

![1709356694.png](img/1709356694.png)

Yeah. So I no longer think below is a shorthand for initializer list:

![1707993690.png](img/1707993690.png)

With the above definition of constructor, we're gonna have to initialize the
class with positional parameter: `var thisOne = SomeOne('Vector', 29)`. This
isn't as verbose as I would like it to be. This is where named arguments
and `{}` come into play:

![1707994006.png](img/1707994006.png)

But unlike positional paramaters, the parameters within `{}` are optional, so
nullable. This is the errors are raised. This is were `required` keyword
comes to play. When named parameters (those within `{}`) refer to fields that
are non-nullable, you need to preprend required before `this`.

![1707994301.png](img/1707994301.png)

## Installation
I followed the manual method of installation : [link](https://docs.flutter.dev/get-started/install/linux#method-2-manual-installation)

Note my path in `.zshrc.local` or `.bashrc`/`.zshrc` in your system:

```bash
export PATH="$PATH:$HOME/bin:$HOME/flutter/bin:$HOME/.pub-cache/bin:$HOME/.local/bin"
```

## Upgrade
To upgrade, I just `cd $HOME/flutter` and `flutter upgrade`

* [meals app index](meals-app/index.md)
