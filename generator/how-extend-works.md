---
nav-title: "How Extend Works"
title: "How Extend Works"
description: "NativeScript Android Runtime Extend Workflow"
position: 3
---

# The Extend Function
Let's consider the following code snippet:

```javascript
var myViewGroup = android.view.ViewGroup.extend({
	hashCode: function() {
		return 0;
	}
})
```

At a low level, the Runtime adds the `extend` function on each Android [Class Proxy](../metadata/accessing-packages.md). The function is associated with a [V8 method callback](http://izs.me/v8-docs/namespacev8.html#a2084c6d4a8bbd7cb65af83251aa59d04), which, when triggered, will first validate the arguments passed and then the [Binding (Proxy) generator](../generator/overview.md) will attempt to create a new dynamic Android Type. This type will allow the JavaScript defined method overrides to be properly called. 

# Extend Function Syntax
The full `extend` syntax is as follows:

```javascript
extend(<className>, <implementationObject>);
```

### The *className* Parameter
This parameter is **optional** and specifies the name for the new class. In case not specified (which is the most common scenario), the Runtime will attempt to generate an implicit unique class name, based on the following criteria:

* The base class name
* The name of the file where the type is declared
* The line and the column at which the `extend` function is called

> Based on the these criteria, a typical implicit class name will look like `java_lang_Object_myFile_l10_c20`. There are several corner cases, as described in the [Gotchas](./gotchas.md) section, where generating implicit class names may lead to errors.

### The *implementationObject* Parameter
This is a JavaScript [Object Literal](http://www.w3schools.com/js/js_objects.asp) that provides the methods to be overridden in the newly generated class. If we consider the example at the top, the implementation object is:

```javascript
var implObject = {
	hashCode: function() {
		return 0;
	}
}
```

# The New Class (Type)
The new type inherits the specified class (`java.lang.Object` in this concrete example) and defines overrides as specified by the JavaScript object literal, passed to the `extend` function:

```java
public class java_lang_Object_myFile_l10_c20 {
	@Override
	public int hashCode () {
		return excuteJavaScriptMethod(this, “hashCode”, null);
	}	
}
```

# See Also
* [Gotchas](./gotchas.md)