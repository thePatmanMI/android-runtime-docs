---
nav-title: "Simple Jar Libs"
title: "Simple Jar Libs"
description: "NativeScript Android Runtime Requirements"
position: 1
---

# Overview

Often Android projects use third-party libraries in order to provide and/or consume application functionality. NativeScript for Android supports this scenario as well. In this article you will learn how to add JAR libraries to your NativeScript for Android project.

# Adding JAR libraries

One of the most common formats in which third-party libraries for Android are distributed is an ordinary JAR file. Adding one or more JAR files to your NativeScript for Android project is done through the `tns` command tool.

For the purpose of this article, we assume that you have already setup an Android project.

```bash
tns create <project_name>
tns platform add android <project_name>
```

You can add one or more JAR files to your NativeScripr for Android project with the following command:

```bash
tns library add android <path_to_dir>
```

This command will read all JAR files from <path_to_dir> folder and will add references to them in your project. As a result, this will create a new folder `lib` which is sibling to the already existing `app` and `platforms`.

```
<project_name>
|--app
|--platforms
```
```
<project_name>
|--app
|--lib
   |--Android
|--platforms
```

The newly created `lib` folder will contain a copy of the third-party libraries. The libraries in this folder will also be referenced from your project.

# Generate metadata

Once yoy added third-party libraries to your project, you have to generate metadata. Generating metadata is part from build command.

```bash
tns build android
```

The metadata is the key piece that allows you to instantiate and use the Java classes defined in the third-party libraries. It contains information what packages, types and methods are defined in the JAR libraries. The metadata files are located in `<project_name>/platforms/android/assets/metadata/` folder.

Metadata alone is not enought to use third-party libraries. Your libraries must become part of the `*.apk` file that you will deploy. To make this possilbe the `build` command copies the JAR files in `<project_name>/platforms/android/libs/` folder.

# See Also
* [Accessing API's](../metadata/accessing-packages.md)
