# Improve your multi-module app build configuration with convention plugins
## Objectives

### Simplify our modules build configurations files
Let's take for example: `feature/game/build.gradle.kts`
- From: 
``` kotlin
plugins {
    id("config")
    id("com.android.library")
    id("kotlin-android")
    id("kotlin-kapt")
}

android {
    compileSdk = 32

    defaultConfig {
        minSdk = 26
        targetSdk = 32

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }


    buildTypes {
        release {
            isMinifyEnabled = false
        }
    }
    
    buildFeatures.compose = true
    composeOptions {
        kotlinCompilerExtensionVersion = libs.versions.composeCompiler.get()
    }
    
    compileOptions {
        sourceCompatibility = 11
        targetCompatibility = 11

        isCoreLibraryDesugaringEnabled = true
    }

    kotlinOptions {
        jvmTarget = config.jvm.kotlinJvm
        freeCompilerArgs = freeCompilerArgs + listOf(
            "-Xopt-in=kotlinx.coroutines.ExperimentalCoroutinesApi",
            "-Xopt-in=kotlinx.coroutines.FlowPreview",
            "-Xopt-in=kotlin.RequiresOptIn"
        )
    }
}

dependencies {
    implementation(libs["kotlin.stdlib"])

    coreLibraryDesugaring(libs["desugarJdk"])
    
    implementation(projects.data.game.public)
    implementation(projects.data.settings.public)
    implementation(project.ui))
    implementation(project.tools.extensions.coroutines))
    implementation(project.tools.annotations))
    implementation(project.tools.logger.public))

    implementation(libs.androidx.lifecycle.viewmodel.ktx)

    implementation(libs.kotlin.coroutines.core)
    implementation(libs.kotlin.coroutines.android)

    implementation(libs.hilt.android)
    implementation(libs.androidx.hilt.compose)
    kapt(libs.hilt.compiler)
    kapt(libs.androidx.hilt.compiler)

    testImplementation(libs.junit)
    testImplementation(libs.robolectric)
    testImplementation(libs.androidx.test.core)
    testImplementation(libs.androidx.test.rules)
}
```
- [To](../wordle-android/feature/game/build.gradle.kts):
``` kotlin
plugins {
    id("fr.sjcqs.android.feature")
}

feature {
    compose.set(true)
}

dependencies {
    implementation(projects.data.game.public)
    implementation(projects.data.settings.public)
}
```

### Allow configuration of our convention plugins

For example here, the `feature` extension allows to enable `compose` or not. 
``` kotlin
feature {
    compose.set(true)
}
```

In our plugin if it's enable it will simply configure our module with
``` kotlin
buildFeatures.compose = true
composeOptions {
    kotlinCompilerExtensionVersion = libs.versions.composeCompiler.get()
}
```

### Notes: 
`libs.<library>` or `libs.versions.<version>` is Gradle's [version catalog API](https://docs.gradle.org/current/userguide/platforms.html)

---
⏭️ [Let's start by creating our plugins project and included it in our app](3-include-build.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)