# Improve your multi-module app build configuration with convention plugins

## Creating and including plugins in our app code

### Creating the [plugins](../wordle-android/plugins) module

1. Create a [plugins](../wordle-android/plugins) directory with
   a [settings.gradle](../wordle-android/plugins/settings.gradle.kts)

```kotlin
enableFeaturePreview("VERSION_CATALOGS")

dependencyResolutionManagement {
    repositories {
        google()
        gradlePluginPortal()
        mavenCentral()
    }
}
```

2. Add `includeBuild("plugins")` in your app's root [settings.gradle](../wordle-android/settings.gradle.kts)

### Sharing dependencies and versions between the included build and the app:

```kotlin
enableFeaturePreview("VERSION_CATALOGS")

dependencyResolutionManagement {
    repositories {
        google()
        gradlePluginPortal()
        mavenCentral()
    }
    // Sharing the root project version catalog
    versionCatalogs {
        create("libs") {
            from(files("../gradle/libs.versions.toml"))
        }
    }
}
```

You will have to use the unsafe API to access you version catalog from the plugins code.

Here's some useful extensions, I used in the plugins:

[🔗 VersionCatalogExtensions.kt](../wordle-android/plugins/src/main/java/extensions/VersionCatalogExtension.kt)
<details>
<summary>💻</summary>

```kotlin
class AndroidFeaturePlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
            // ...

            dependencies.apply {
                // ...
                add("implementation", libs["kotlin.coroutines.android"])
                // ...
            }
        }
    }
}
```

```kotlin
class AndroidLibraryWithComposePlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
            // ...

            extensions.configure<LibraryExtension> {
                buildFeatures.compose = true
                composeOptions {
                    kotlinCompilerExtensionVersion = libs.requireVersion("composeCompiler")
                }
            }
        }
    }
}
```

```kotlin
internal val Project.libs: VersionCatalog
    get() = extensions.getByType<VersionCatalogsExtension>().named("libs")

/**
 * Usage: libs["<library>']
 */
internal operator fun VersionCatalog.get(
    name: String
): Provider<MinimalExternalModuleDependency> {
    val optionalDependency = findLibrary(name)
    if (optionalDependency.isEmpty) {
        error("$name is not a valid dependency, check your version catalog")
    }
    return optionalDependency.get()
}

internal fun VersionCatalog.requireVersion(alias: String): String {
    val optionalVersion = findVersion(alias)
    if (optionalVersion.isEmpty) {
        error("$alias is not a valid version, check your version catalog")
    }
    return optionalVersion.get().toString()
}
```

</details>

### Register the conventions plugins

1. Create a [build.gradle](../wordle-android/plugins/build.gradle.kts) file
2. Define the plugins you are going to use in your project conventions plugins
   as `compileOnly` [dependencies](../wordle-android/gradle/libs.versions.toml)

  <details>
  <summary>💻</summary>

  ``` kotlin
  dependencies {
      compileOnly(libs.kotlin.gradle) // org.jetbrains.kotlin:kotlin-gradle-plugin
  
      compileOnly(libs.android.gradle) // com.android.tools.build:gradle
      compileOnly(libs.hilt.gradle) // com.google.dagger:hilt-android-gradle-plugin
  
  }
  ```

  </details>

3. Register the plugins:

```kotlin
 plugins {
    `kotlin-dsl` // will apply `java-gradle-plugin`
}

dependencies { ... }

// java-gradle-plugin
gradlePlugin {
    plugins {
        register("<name>") {
            id = "<id>"
            implementationClass = "<implementation class>"
        }
// ...
    }
}
```

  <details>
  <summary>💻</summary>

  ``` kotlin
  plugins {
    `kotlin-dsl` // will apply `java-gradle-plugin`
  }
  
  dependencies { ... }
  
  // java-gradle-plugin
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

  </details>

---
⏭️ [Now let's write our plugins](4-writing-plugin.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)