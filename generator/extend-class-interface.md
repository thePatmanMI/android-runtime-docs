---
nav-title: "Extending Classes And Interfaces"
title: "Extending Classes And Interfaces"
description: "NativeScript Android Runtime Extending Classes And Interfaces"
position: 3
---

As part of the native Android development you often have to inherit from classes and/or implement interfaces. NativeScript supports these scenarios as well.

# Classes

```java
public class MyButton extends android.widget.Button {
	public MyButton(Context context) {
		super(context);
	}
	
	@Override
	public void setEnabled(boolean enabled) {
		super.setEnabled(enabled);
	}
}
MyButton btn = new MyButton(context);
```

Here is how the above is done in NativeScript:

```javascript
var MyButton = android.widget.Button.extend({
	setEnabled: function(enabled) {
	  	this.supper.setEnable(enabled);
	}
});
var btn = new MyButton(context);
```

# Interfaces
The next example shows how to implement an interface in Java and NativeScript. The main difference between inheriting classes and implementing interfaces in NativeScript is the use of the `extend` keyword. Basically, you implement an interface by passing the *implementation* object to the interface constructor function. The syntax is identical to the [Java Anonymous Classes](http://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html).

```java
button.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		// Perform action on click
	}
});
```

```javascript
button.setOnClickListener(new android.view.View.OnClickListener({
	onClick: function() {
		// Perform action on click
	}
}));
```

# Remarks
The current NativeScript runtime allows implementing a single interface at once. This may change in future.

# See Also
* [How Extend Works](./how-extend-works.md)
* [Gotchas](./gotchas.md)