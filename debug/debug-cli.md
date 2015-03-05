---
nav-title: "Debug"
title: "Debug"
description: "Debugging NativeScript for Android"
position: 1
---

# Overview

NativeScript for Android provides support for JavaScript debugging. The current implentation supports two major scenarios.
  * Start debugging - starts an application with debugger enabled
  * Attach/Detach debugger - attach/detach debuggger to a running application

Please note that the current implementation requires Chrome browser installed.  

# Start an application with debugger enabled

The following command with build, deploy and run the application with attached debugger. This will allow you to debug your application from the first JavaScript statement.

```bash
tns debug andorid --debug-brk
```

Behind the scenes `debug` command together with `--debug-brk` option will build and start the application, then it will find available port and enable V8 debugger on that port. Finally, it will start Node Inspector and launch Chrome browser.

![Image1](./debug-cli-screenshot.png)

# Attach/detach debugger

If you have a running application you can attach/detach debugger with the following commands.

```bash
tns debug andorid --start
```
```bash
tns debug andorid --stop
```

As in the previous scenario, `debug` command with configure V8 debugger port, forward the port, stars Node Inspector and launch Chrome browser.

# Miscellaneous

You can check whether a debugger is enabled with the following command.

```bash
tns debug andorid --get-port
```

It will return the current debugger port, 0 otherwise.

# Remarks

Current implementation has hard-coded 30 seconds timeout for establishing connection between the commnad line tool and the device/emulator.
