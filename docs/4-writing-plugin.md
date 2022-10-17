# Improve your multi-module app build configuration with convention plugins
## Writing one of our plugin

Regardless of the language you are using in your `build.gradle(.kts)` files (Kotlin vs Groovy).
You can write Gradle plugins in Groovy, Java or Kotlin.

The syntax is similar to the one you would use in a `build.gradle` file.

### Android library module

1. Creating the plugin class
    <details>
    <summary>💻</summary>
    
    ```kotlin
    package fr.sjcqs
    
    import org.gradle.api.Plugin
    import org.gradle.api.Project
    
    class AndroidLibPlugin : Plugin<Project> {
        override fun apply(target: Project) {
            // configuration ...
        }
    }
    ```
    
    </details>
2. Applying plugins
    <details>
    <summary>💻</summary>
    
    ```kotlin
    package fr.sjcqs
    
    import org.gradle.api.Plugin
    import org.gradle.api.Project
    import org.gradle.kotlin.dsl.apply
    
    class AndroidLibPlugin : Plugin<Project> {
        override fun apply(target: Project) {
            with(target) {
                with(pluginManager) {
                    apply("com.android.library")
                    apply("kotlin-android")
                }
    
                // ...
            }
        }
    }
    ```
    
    </details>
3. Configure the Android extension
    <details>
    <summary>💻</summary>
    
    ```kotlin
    import Config
    import com.android.build.gradle.LibraryExtension
    import com.android.build.gradle.LibraryPlugin
    import extensions.configureAndroidAndKotlin
    import extensions.disableDebugBuildType
    import extensions.get
    import extensions.libs
    import org.gradle.api.Plugin
    import org.gradle.api.Project
    import org.gradle.kotlin.dsl.apply
    import org.gradle.kotlin.dsl.configure
    import org.gradle.kotlin.dsl.dependencies
    
    class AndroidLibPlugin : Plugin<Project> {
        override fun apply(target: Project) {
            with(target) {
                with(pluginManager) {
                    // applying plugins
                }
    
                extensions.configure<LibraryExtension> {
                    configureAndroidAndKotlin(this)
    
                    defaultConfig.targetSdk = Config.android.targetSdk
                    buildTypes {
                        all { isMinifyEnabled = false }
                    }
    
                }
            }
        }
    }
    ```
    
    <details>
    <summary>configureAndroidAndKotlin</summary>
    Configure both the Android library and application plugins.
    
    ```kotlin
    internal fun Project.configureAndroidAndKotlin(extension: CommonExtension<*, *, *, *>) {
        with(extension) {
            compileSdk = Config.android.compileSdkVersion
    
            defaultConfig {
                minSdk = Config.android.minSdk
    
                testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
            }
    
            buildTypes {
                getByName("debug") {
                    isMinifyEnabled = false
                    matchingFallbacks.add("release")
                }
    
                getByName("release") {
                    isMinifyEnabled = true
                    proguardFiles(
                        getDefaultProguardFile("proguard-android-optimize.txt"),
                        "proguard-rules.pro"
                    )
                }
            }
    
            compileOptions {
                sourceCompatibility = Config.jvm.javaVersion
                targetCompatibility = Config.jvm.javaVersion
    
                isCoreLibraryDesugaringEnabled = true
            }
    
            kotlinOptions {
                jvmTarget = Config.jvm.kotlinJvm
                freeCompilerArgs = freeCompilerArgs + Config.jvm.freeCompilerArgs
            }
    
            packagingOptions.resources.excludes += "/META-INF/{AL2.0,LGPL2.1}"
        }
    
        dependencies.apply {
            add("coreLibraryDesugaring", libs["desugarJdk"])
        }
    }
    
    
    private fun CommonExtension<*, *, *, *>.kotlinOptions(block: KotlinJvmOptions.() -> Unit) {
        (this as ExtensionAware).extensions.configure("kotlinOptions", block)
    }
    ```
    
    </details>
    </details>
4. Adding the dependencies
    <details>
    <summary>💻</summary>
    
    ```kotlin
    import Config
    import com.android.build.gradle.LibraryExtension
    import com.android.build.gradle.LibraryPlugin
    import extensions.configureAndroidAndKotlin
    import extensions.disableDebugBuildType
    import extensions.get
    import extensions.libs
    import org.gradle.api.Plugin
    import org.gradle.api.Project
    import org.gradle.kotlin.dsl.apply
    import org.gradle.kotlin.dsl.configure
    import org.gradle.kotlin.dsl.dependencies
    
    class AndroidLibPlugin : Plugin<Project> {
        override fun apply(target: Project) {
            with(target) {
                with(pluginManager) {
                    // applying plugins
                }
    
                extensions.configure<LibraryExtension> {
                    // configuring the library extension
                }
    
                // We don't have access to extensions like `implementation` and `compileOnly`  
                dependencies {
                    add("implementation", project(":tools:annotations")) 
    
                    add("compileOnly", libs["javaxInject"])
    
                    add("implementation", libs["kotlin.stdlib"])
                }
            }
        }
    }
    ```
    
    </details>
##### Usage
```kotlin
plugins {
    id("fr.sjcqs.android.lib")  // ⬅️
}

dependencies {
    implementation(projects.tools.logger.public)
    implementation(libs.timber)
}
```

### Feature module

<details>
<summary>💻</summary>

```kotlin
package fr.sjcqs

import extensions.get
import extensions.libs
import org.gradle.api.Plugin
import org.gradle.api.Project

class AndroidFeaturePlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
            pluginManager.apply {
                apply(AndroidLibPlugin::class.java)
                apply("kotlin-kapt")
            }

            dependencies.apply {
                add("implementation", project(":ui"))
                add("implementation", project(":tools:extensions:coroutines"))

                add("implementation", libs["androidx.lifecycle.viewmodel.ktx"])

                add("implementation", libs["kotlin.coroutines.core"])
                add("implementation", libs["kotlin.coroutines.android"])

                add("implementation", libs["hilt.android"])
                add("implementation", libs["androidx.hilt.compose"])
                add("kapt", libs["hilt.compiler"])
                add("kapt", libs["androidx.hilt.compiler"])

                add("testImplementation", libs["junit"])
                add("testImplementation", libs["robolectric"])
                add("testImplementation", libs["androidx.test.core"])
                add("testImplementation", libs["androidx.test.rules"])
            }
        }
    }
}
```
</details>

##### Usage

```kotlin
plugins {
    id("fr.sjcqs.android.feature") // ⬅️
    id("fr.sjcqs.android.compose.lib")
}

dependencies {
    implementation(projects.data.game.public)
    implementation(projects.data.settings.public)
}
```

### Enabling compose in module
<details>
<summary>💻</summary>

```kotlin
package fr.sjcqs

import com.android.build.gradle.LibraryExtension
import com.android.build.gradle.LibraryPlugin
import extensions.libs
import extensions.requireVersion
import org.gradle.api.Plugin
import org.gradle.api.Project
import org.gradle.kotlin.dsl.configure

class AndroidLibraryWithComposePlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
            pluginManager.apply("com.android.library")

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

</details>

##### Usage
```kotlin
plugins {
    id("fr.sjcqs.android.feature")
    id("fr.sjcqs.android.compose.lib") // ⬅️
}

dependencies {
    implementation(projects.data.game.public)
    implementation(projects.data.settings.public)
}
```

### Android wiring plugin (module with dependency injection)

<details>
<summary>💻</summary>

```kotlin
package fr.sjcqs

import extensions.get
import extensions.libs
import org.gradle.api.Plugin
import org.gradle.api.Project

class AndroidWiringLibPlugin : Plugin<Project> {
    override fun apply(target: Project) {
        with(target) {
            pluginManager.apply {
                apply(AndroidLibPlugin::class.java)
                apply("kotlin-kapt")
            }

            dependencies.apply {
                add("implementation", libs["hilt.core"])
                add("kapt", libs["hilt.compiler"])
            }
        }
    }

}
```
</details>

##### Usage

```kotlin
plugins {
    id("fr.sjcqs.android.wiring.lib") // ⬅️
}

dependencies {
api(projects.tools.logger.public)
api(projects.tools.logger.impl)
}
```

---
⏭️ [Some gotchas and further thoughts](5-gotchas.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)