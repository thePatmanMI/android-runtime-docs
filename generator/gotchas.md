---
nav-title: "Gotchas"
title: "Gotchas"
description: "NativeScript Android Runtime Known Limitations"
position: 4
---

## Overview

As explained in the previous page, everytime when you inherit from a class or implement an interface NativeScript runtime generates a new class. By default you don't have to specify a class name and NativeScript will pick one for you. The current naming convention generates a class name based on the following:
* The base class name
* The name of file in which you declare the type plus the row and column number

This has some important implications. Let's have a look the following code fragment.

```JavaScript
var MyButton = new android.widget.Button.extend({
    setEnabled: function(enabled) {
      // do something
    }
});
```
Because `extend` function generates a new class each time when is called, the second time you call it you will an error. In this example the error message reads the following.

*Extend name android/widget/Button-f--mainpage-l45-c23-- already used*

The error message gives us an important information. More preciessly, the file (*mainpage.js*), the line (*45*) and the column (*23*) number.

##Workaround

The full `extend` syntext is as follows.

```JavaScript
extend(<className>, <implementationObject>)
```
We can take advantage of the first parameter to specify a unique class name each time when `extend` is called. The refactored code should like something like the following.


```JavaScript
var className = getUniqueClassName(...);
var MyButton = new android.widget.Button.extend(className, {
    setEnabled: function(enabled) {
      // do something
    }
});
```

Another, even simpler, option is to guarantee that the desired `extend` function call is made only once per <file, line number, column number> combination.
