As explained in the previous page, everytime when you inherit from a class or implement an interface NativeScript runtime generates a new class. By default you don't have to specify a class name and NativeScript will pick one for you. The current naming convention generates a class name based on the following:
* The base class name
* The name of file in which you declare the type plus the row and column number

This has some important implications...
