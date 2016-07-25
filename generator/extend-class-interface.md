---
nav-title: "Extending Classes And Interfaces"
title: "Extending Classes And Interfaces"
description: "NativeScript Android Runtime Extending Classes And Interfaces"
position: 2
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
var constructorCalled = false;
var MyButton = android.widget.Button.extend({
		//constructor
		init: function() {
			constructorCalled = true;
		},
		
		setEnabled: function(enabled) {
			this.super.setEnable(enabled);
		}
	});
	var btn = new MyButton(context);
	//constructorCalled is true here
```

> **Note:** In the above setEnabled function the `this` keyword points to the JavaScript object that proxies the extended native instance. The `this.super` property provides access to the base class method implementation.

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

## Implementing multiple interfaces in NativeScript 

Suppose you have the following classes:

```java
public interface Printer {
	void print(String content);
	void print(String content, int offset);
}

public interface Copier {
	String copy(String content);
}

public interface Writer {
	void write(Object[] arr);
	void writeLine(Object[] arr)
}
```

Implementing the interfaces is as easy in Java as writing:

```java
public class MyVersatileCopywriter implements Printer, Copier, Writer {
	public void print(String content) {
		...
	}

	public void print(String content, int offset) { ... }

	...
}
```

That can be achieved in NativeScript by extending any valid Java object and declaring an **interfaces** array in the implementation:

Javascript - attach `interfaces` array to implementation object of the extend call

```javascript
var MyVersatileCopyWriter = java.lang.Object.extend({
	interfaces: [com.a.b.Printer, com.a.b.Copier, com.a.b.Writer],
	print: function() { ... },
	copy: function() { ... },
	write: function() { ... },
	writeLine: function() { ... }
});
```

Typescript - note how you **decorate** the class instead of declaring a static interface field 

```typescript
@Interfaces([com.a.bPrinter, com.a.b.Copier, com.a.b.Writer])
class MyVersatileCopyWriter extends java.lang.Object {
	print() { ... }
	copy() { ... }
	write() { ... }
	writeLine() { ... }
}
```

# See Also
* [How Extend Works](./how-extend-works.md)
* [Gotchas](./gotchas.md)
