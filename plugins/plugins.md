---
title: Android Plugins Infrastructure
description: When the NativeScript modules do not provide the native device or platform capability that you need, you can use NativeScript plugins.
position: 0
slug: android-plugins-infrastructure
---

Android Plugins Infrastructure
=========

Starting with NativeScript CLI 1.1.0, you can develop or use plugins in your NativeScript projects.

* [What Are NativeScript Plugins](#what-are-nativescript-plugins)
* [Create a Plugin](#create-a-plugin)
    * [Directory Structure](#directory-structure)
    * [Android plugin elements](#android-plugin-elements)
    * [Include.gradle Specification](#includegradle-specification)
* [Rules of thumb](#rules-of-thumb)
    * [What do I use?](#what-do-i-use) 

## What Are NativeScript Plugins

A NativeScript plugin is any npm package, published or not, that exposes a native API via JavaScript and consists of the following elements. The plugin must have the directory structure, described in the [Directory Structure](#directory-structure) section.

## Create a Plugin

If the NativeScript framework does not expose a native API that you need, you can develop a plugin which exposes the required functionality. When you develop a plugin, keep in mind the following requirements.

* The plugin must be a valid npm package.
* The plugin must expose a built-in native API or a native API available via custom native libraries.
* The plugin must be written in JavaScript or TypeScript and must comply with the CommonJS specification. If written in TypeScript, make sure to include the compiled `JavaScript` file in your plugin.
* The plugin directory structure must comply with the specification described below.
* The plugin must contain a valid `package.json` which complies with the specification described below.
* If the plugin requires any permissions, features or other configuration specifics, it must contain `AndroidManifest.xml` file which describes them.
* If the plugin depends on native libraries, it must contain a valid `include.gradle file`, which describes the dependencies.

### Directory Structure

This is what an Android NativeScript plugin can include.
```
my-plugin/
├── package.json
├── MyModule1/
│   ├── index1.js
│   └── package.json
├── MyModule2/
│   ├── index2.js
│   └── package.json
└── platforms/
    ├── android/
    │   └── AndroidManifest.xml
    │   └── include.gradle
    │   └── MyLibrary.aar
    │   └── MyLibrary.jar
    │   └── res/
```

### Android plugin elements

You can find more information on the common parts of the NativeScript plugins like the `package.json` and js modules [here]({% slug plugins-infrastructure %}).
* `platforms\android`: This directory contains any native Android libraries packaged as `*.jar` and `*.aar` packages. These native libraries can reside in the root of this directory or in a user-created sub-directory. During the plugin installation, the NativeScript CLI will configure the Android project in `platforms\android` to work with the plugin. 
* `platforms\android\AndroidManifest.xml`: This file describes any specific configuration changes required for your plugin to work. For example: required permissions. For more information about the format of `AndroidManifest.xml`, see [App Manifest](http://developer.android.com/guide/topics/manifest/manifest-intro.html).<br/>During build, gradle will merge the plugin `AndroidManifest.xml` with the `AndroidManifest.xml` for your project. The NativeScript CLI will not resolve any contradicting or duplicate entries during the merge. After the plugin is installed, you need to manually resolve such issues.
* `platforms\android\include.gradle`: This file modifies the native Android configuration of your NativeScript project such as native dependencies, build types and configurations. For more information about the format of `include.gradle`, see [include.gradle file](#includegradle-specification).
* `platforms\android\MyLibrary.aar` is an Android library. You can read more about the `.aar` format [here](http://tools.android.com/tech-docs/new-build-system/aar-format).
* `platforms\android\MyLibrary.jar` is an Android library. You can read more about the `.jar` format [here](https://en.wikipedia.org/wiki/JAR_(file_format))
* `platforms/android/res`:  This directory contains resources declared by the `AndroidManifest.xml` file. You can look at the folder structure [here](http://developer.android.com/guide/topics/resources/providing-resources.html#ResourceTypes).

### Include.gradle Specification

Every NativeScript plugin, which contains native Android dependencies, should also contain a valid `include.gradle` file in the root of its `platforms\android` directory. This `include.gradle` file must meet the following requirements.

* It must contain its own [configuration](http://developer.android.com/tools/building/configuring-gradle.html).
* It might contain native dependencies required to build the plugin properly.
* Any native dependencies should be available in [jcenter](https://bintray.com/bintray/jcenter) or from the Android SDK installed on your machine if you want it to work out of the box. You can see an example of a compile dependency [here](https://github.com/NativeScript/nativescript-facebook-plugin/blob/master/platforms/android/include.gradle).
* It can be used for any kind of native configuration. Find more information [here](http://developer.android.com/tools/building/configuring-gradle.html)

> **IMPORTANT:** If you don't have an `include.gradle` file, at build time, gradle will create a default one containing all default elements.

#### Include.gradle Example
```
//default elements
android { 
    productFlavors {
        "my-plugin" {
            dimension "my-plugin"
        }
    }
}

//optional elements
dependencies {
    compile "groupName:pluginName:ver"
}
```

## Rules of thumb

We are concentrating on the "_native_" part of the plugin. When we talk about "_native_" part of the plugin we mean the `platforms/android` folder and its content. 

#### What do I use?

When you want to create an Android NativeScript plugin and you want to add some "_native_" functionality there are two main options. Use a `.jar` file, or use a `.aar` file. Keep in mind that `.aar` files are the recomended library form for NativeScript plugins. When we use a library in the form of a `.jar` file we want some functionality that doesn’t need any resources, just native implementation of some logic we need. For example, if we need to make some complicated calculation and there is an SDK in the form of a `.jar` file and requires **no** UI elements, we could use that library. This would only provide a couple of classes with some logic in them and **should not** declare activities or any other types of application components (http://www.tutorialspoint.com/android/android_application_components.htm).

* In what cases should we prefer `.jar` files?
    * when we don’t have any need for using android application components
    * when we want to add classes with logic in them, we can use from our js user code.
    * when we don’t need resources connected to the `.jar` file like drawables, layouts, etc.

* In what cases should we prefer `.aar` files:
    * when we want to use some kind of an interactive SDK like facbook, dropbox, youtube, etc.
    * when we need to use application components like activities, services, etc.

> **IMPORTANT:**  The recommended way of using AAR files inside a NativeScript plugin is to add it as a dependency in the `include.gradle` file inside the `platforms/android` folder of the plugin.
