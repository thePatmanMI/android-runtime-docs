---
nav-title: "Using Resource Libraries"
title: "Using Resource Libraries"
description: "NativeScript Android Runtime Requirements"
position: 2
---

> **IMPORTANT:** Use plugins to work with external libs. For more information about working with plugins, see [Plugins > Infrastructure]({% slug android-plugins-infrastructure %}).

# Overview

Often when you are developing an application you need to include a shared code or resources from a library. NativeScript for Android supports this scenario.

# Reusing Shared Libraries

For the purpose of this article we assume that you alredy setup an Android project.

```bash
tns create <project_name>
tns platform add android <project_name>
```

You can  add a reference to a shared library with the following command.

```bash
tns library add android <path_to_libraries_dir>
```

```
<project_name>
|--app/
|--platforms/
```
```
<project_name>
|--app/
|--lib/
   |--Android/
      |--<your_shared_lib>
      |--...
|--platforms/
```

The `lib` folder will contain a copy of your shared library.

# Generate Metadata

See [Generate Metadata](./jars.md#generate-metadata)

# See Also
* [Accessing APIs](../metadata/accessing-packages.md)
