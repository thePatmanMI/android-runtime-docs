As a part of native Android development you often have to inherit from classes and/or implement interfaces. NativeScript suppport these scenarios as well.

## Classes

```Java
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
Here is how it is done in NativeScript for Android.

```JavaScript
var MyButton = new android.widget.Button.extend({
	setEnabled: function(enabled) {
	  	this.supper.setEnable(enabled);
	}
});
var btn = new MyButton(context);
```
## Interfaces
The next example shows how to implement an interface in Java and NativeScript. The main difference between inheriting classes and implementing interfaces in NativeScript is the used of `extend` keyword. Basically, you implement an interface by passing the *implementation* object to the interface constructor function. The syntaxt should be familiar to the Java developers who are used to anonymous classes.

```Java
button.setOnClickListener(new View.OnClickListener() {
  public void onClick(View v) {
    // Perform action on click
  }
});
```

```JavaScript
button.setOnClickListener(new android.view.View.OnClickListener({
  onClick: function() {
    // Perform action on click
  }
}));
```

## Remarks
The current NativeScript runtime allows implementing a single interface at once. This may change in future.
