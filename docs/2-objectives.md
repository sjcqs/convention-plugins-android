# Improve your multi-module app build configuration with convention plugins

## Objectives

### Simplify our modules build configurations files

Let's take for example a feature module named **game** (`feature/game/build.gradle.kts`).

We want to go:

<table style="vertical-align: top;">
 <tr>
    <td>From this:</td>
    <td>To this:</td>
 </tr>
 <tr>
    <td>

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

        testInstrumentationRunner = // ...
    }


    buildTypes {
        release {
            isMinifyEnabled = false
        }
    }
    
    buildFeatures.compose = true
    composeOptions {
        kotlinCompilerExtensionVersion = // ...
    }
    
    compileOptions {
        sourceCompatibility = 11
        targetCompatibility = 11

        isCoreLibraryDesugaringEnabled = true
    }

    kotlinOptions {
        jvmTarget = config.jvm.kotlinJvm
        freeCompilerArgs = freeCompilerArgs + listOf(
            // ...
        )
    }
}

dependencies {
implementation(libs.kotlin.stdlib)

    coreLibraryDesugaring(libs.desugarJdk)
    
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

</td>
    <td style="vertical-align: top;">

``` kotlin
plugins {
    id("fr.sjcqs.android.feature")
    id("fr.sjcqs.android.compose.lib")
}

dependencies {
    implementation(projects.data.game.public)
    implementation(projects.data.settings.public)
}
```

</td>
 </tr>
</table>

---
⏭️ [Let's start with a quick win](3-version-catalog.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)