# Launcher3

​        This project aims to create a Launcher3 project that can be compiled and debugged in Android Studio. If there are other Android Apps that have the same requirements, it can be used as a reference.

 	Since Launcher3 uses some private APIs, such as `android.app.WallpaperColors`, these cause Android Studio to use the standard sdk to compile.

## 1.get-framework.jar
`framework.jar` can be obtained by compiling the official aosp project of android, and the compiling time is relatively long. The generated path is：

```java
out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes.jar
```

If you don't know how to compile the android source code, you can refer to [Google Official Build Compilation Environment Guidelines](https://source.android.com/source/initializing)，Of course, if you are not interested, you can use the one I uploaded `freeme-framework.jar`.

## 2. place framework.jar
Create a new `libs` directory. In this article, I renamed it to `freeme-framework.jar` and put it in this `libs` directory.

## 3. modify build.gradle

```groovy
dependencies {
    //provided It means that this library is only used at compile time, but will not be compiled into apk or aar in the end
    provided files('libs/freeme-framework.jar')
    
    ...
}

//Adding the configuration here is intended to adjust the loading of freeme-framework.jar first when compiling, so that the use of a private API in a public class will not report an error
//-Xbootclasspath/p is the priority addressing setting of java addressing
//The path followed by "Xbootclasspath/p" is a relative path relative to the root directory of the current Project
gradle.projectsEvaluated {
    tasks.withType(JavaCompile) {
        options.compilerArgs << '-Xbootclasspath/p:libs/freeme-framework.jar'
    }
}
```

## Finally, it can run smoothly

