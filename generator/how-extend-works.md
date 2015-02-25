---
nav-title: "How Extend Works"
title: "How Extend Works"
description: "NativeScript Android Runtime Extend Workflow"
position: 2
---

# The Extend Function
Let's consider the following code snippet:

```javascript
var myObject = java.lang.Object.extend({
	hashCode: function() {
		return 0;
	}
})
```

At a low level, the Runtime adds the `extend` function on each Android [Class Proxy](../metadata/accessing-packages.md). The function is associated with a [V8 method callback](http://izs.me/v8-docs/namespacev8.html#a2084c6d4a8bbd7cb65af83251aa59d04), which, when triggered, will first validate the arguments passed and then the [Binding (Proxy) generator](../generator/overview.md) will attempt to create a new dynamic Android Type. This type will allow the JavaScript defined method overrides to be properly called.

> **Note:** There are several corner cases for generating such dynamic types, described in the [Gotchas](./gotchas.md) section.

# The Binding Proxy
The new dynamically generated type inherits the desired class (`java.lang.Object` in the above concrete example) and defines overrides 