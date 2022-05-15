# Improve your multi-module app build configuration with convention plugins
## Creating and including plugins in our app code

### Creating the [plugins](../wordle-android/plugins) module

- [plugins](../wordle-android/plugins) directory with [settings.gradle](../wordle-android/plugins/settings.gradle.kts)
  - Note: Sharing the `versionCatalog` (`create("libs") { from(files(...))}`)
- `includeBuild("plugins")` in [\<root\>/settings.gradle](../wordle-android/settings.gradle.kts)

### Register the conventions plugins
- Create a [build.gradle](../wordle-android/plugins/build.gradle.kts) file
- Define the plugins you are going to use in your project conventions plugins as `compileOnly` [dependencies](../wordle-android/gradle/libs.versions.toml)
``` kotlin
dependencies {
    compileOnly(libs.kotlin.gradle) // org.jetbrains.kotlin:kotlin-gradle-plugin

    compileOnly(libs.android.gradle) // com.android.tools.build:gradle
    compileOnly(libs.hilt.gradle) // com.google.dagger:hilt-android-gradle-plugin

}
```
- Register plugins
  - `kotlin-dsl` will apply `java-gradle-plugin`
``` kotlin
plugins {
  `kotlin-dsl` // will apply java-gradle-plugin
}

dependencies { ... }

// extension from java-gradle-plugin
gradlePlugin {
  plugins {
    register("<name>") {
      id = "<id>"
      implementationClass = "<implementation class>"
    }
    register("fr.sjcqs.android.app") {
        id = "fr.sjcqs.android.app"
        implementationClass = "fr.sjcqs.AndroidAppPlugin"
    }
    register("fr.sjcqs.android.feature") {
        id = "fr.sjcqs.android.feature"
        implementationClass = "fr.sjcqs.AndroidFeaturePlugin"
    }
    register("fr.sjcqs.android.lib") {
        id = "fr.sjcqs.android.lib"
        implementationClass = "fr.sjcqs.AndroidLibPlugin"
    }
    register("fr.sjcqs.android.wiring.lib") {
        id = "fr.sjcqs.android.wiring.lib"
        implementationClass = "fr.sjcqs.AndroidWiringLibPlugin"
    }
    register("fr.sjcqs.jvm.lib") {
        id = "fr.sjcqs.jvm.lib"
        implementationClass = "fr.sjcqs.JvmLibPlugin"
    }
  }
}
```

---
⏭️ [Now let's write our plugin](4-writing-plugin.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)