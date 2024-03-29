# Improve your multi-module app build configuration with convention plugins

## Sharing dependencies and versions:

A first step to simplify your build configuration in your project is to have a single source of truth for your
dependencies.
To share versions and plugins, you can use
Gradle's [version catalog API](https://docs.gradle.org/current/userguide/platforms.html).

In `settings.gradle.kts`

```kotlin
enableFeaturePreview("VERSION_CATALOGS")
```

Then you can create a version catalog file in `gradle/libs.versions.toml`
<details>
<summary>💻</summary>

```kotlin
[versions]
accompanist = "0.23.0"
androidxhilt = "1.0.0"
androidxlifecycle = "2.4.1"
androidxtest = "1.4.0"
compose = "1.1.0"
composeCompiler = "1.1.0"
coroutines = "1.6.0"
dagger = "2.40.5"
datastore = "1.0.0"
kotlin = "1.6.10"
ktlint = "0.41.0"
okhttp = "4.9.1"
paging = "3.0.0"
retrofit = "2.9.0"
sqldelight = "1.5.3"

[libraries]
android - gradle = { module = "com.android.tools.build:gradle", version = "7.1.1" }

androidx - activity - compose = "androidx.activity:activity-compose:1.4.0"
androidx - appcompat = "androidx.appcompat:appcompat:1.4.0"
androidx - archCoreTesting = "androidx.arch.core:core-testing:2.1.0"
androidx - datastore - preferences - core =
    { module = "androidx.datastore:datastore-preferences-core", version.ref = "datastore" }
androidx - datastore - preferences - android =
    { module = "androidx.datastore:datastore-preferences", version.ref = "datastore" }
androidx - core = "androidx.core:core-ktx:1.7.0"
androidx - splashscreen = "androidx.core:core-splashscreen:1.0.0-beta01"

androidx - hilt - compiler = { module = "androidx.hilt:hilt-compiler", version.ref = "androidxhilt" }
androidx - hilt - compose = "androidx.hilt:hilt-navigation-compose:1.0.0"
androidx - hilt - work = { module = "androidx.hilt:hilt-work", version.ref = "androidxhilt" }

androidx - lifecycle - runtime =
    { module = "androidx.lifecycle:lifecycle-runtime-ktx", version.ref = "androidxlifecycle" }
androidx - lifecycle - viewmodel - ktx =
    { module = "androidx.lifecycle:lifecycle-viewmodel-ktx", version.ref = "androidxlifecycle" }

androidx - navigation - compose = "androidx.navigation:navigation-compose:2.4.1"

androidx - test - core = { module = "androidx.test:core", version.ref = "androidxtest" }
androidx - test - junit = "androidx.test.ext:junit-ktx:1.1.3"
androidx - test - rules = { module = "androidx.test:rules", version.ref = "androidxtest" }
androidx - test - runner = { module = "androidx.test:runner", version.ref = "androidxtest" }

compose - animation - animation = { module = "androidx.compose.animation:animation", version.ref = "compose" }
compose - foundation - foundation = { module = "androidx.compose.foundation:foundation", version.ref = "compose" }
compose - foundation - layout = { module = "androidx.compose.foundation:foundation-layout", version.ref = "compose" }
compose - material - material = "androidx.compose.material3:material3:1.0.0-alpha05"
compose - ui - test = { module = "androidx.compose.ui:ui-test-junit4", version.ref = "compose" }
compose - ui - tooling = { module = "androidx.compose.ui:ui-tooling", version.ref = "compose" }
compose - ui - preview = { module = "androidx.compose.ui:ui-tooling-preview", version.ref = "compose" }
compose - ui - ui = { module = "androidx.compose.ui:ui", version.ref = "compose" }
compose - ui - util = { module = "androidx.compose.ui:ui-util", version.ref = "compose" }
compose - ui - viewbinding = { module = "androidx.compose.ui:ui-viewbinding", version.ref = "compose" }

dagger - compiler = { module = "com.google.dagger:dagger-compiler", version.ref = "dagger" }
dagger - dagger = { module = "com.google.dagger:dagger", version.ref = "dagger" }

desugarJdk = "com.android.tools:desugar_jdk_libs:1.1.5"

firebase - bom = "com.google.firebase:firebase-bom:29.0.4"
firebase - crashlytics - ktx = { module = "com.google.firebase:firebase-crashlytics-ktx" }
firebase - crashlytics - gradle = "com.google.firebase:firebase-crashlytics-gradle:2.8.1"
firebase - appcheck = "com.google.firebase:firebase-appcheck-safetynet:16.0.0-beta04"
firebase - database = { module = "com.google.firebase:firebase-database-ktx" }
firebase - links = { module = "com.google.firebase:firebase-dynamic-links-ktx" }

google - material = "com.google.android.material:material:1.5.0"
google - services = "com.google.gms:google-services:4.3.10"

hilt - android = { module = "com.google.dagger:hilt-android", version.ref = "dagger" }
hilt - compiler = { module = "com.google.dagger:hilt-android-compiler", version.ref = "dagger" }
hilt - core = { module = "com.google.dagger:hilt-core", version.ref = "dagger" }
hilt - gradle = { module = "com.google.dagger:hilt-android-gradle-plugin", version.ref = "dagger" }
hilt - testing = { module = "com.google.dagger:hilt-android-testing", version.ref = "dagger" }

javaxInject = "javax.inject:javax.inject:1"

junit = "junit:junit:4.13.2"

kotlin - coroutines - android =
    { module = "org.jetbrains.kotlinx:kotlinx-coroutines-android", version.ref = "coroutines" }
kotlin - coroutines - core = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-core", version.ref = "coroutines" }
kotlin - coroutines - test = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-test", version.ref = "coroutines" }
kotlin - coroutines - playServices =
    { module = "org.jetbrains.kotlinx:kotlinx-coroutines-play-services", version.ref = "coroutines" }
kotlin - extensions = { module = "org.jetbrains.kotlin:kotlin-android-extensions", version.ref = "kotlin" }
kotlin - gradle = { module = "org.jetbrains.kotlin:kotlin-gradle-plugin", version.ref = "kotlin" }
kotlin - stdlib = { module = "org.jetbrains.kotlin:kotlin-stdlib", version.ref = "kotlin" }

leakCanary = "com.squareup.leakcanary:leakcanary-android:2.8.1"

mockK = "io.mockk:mockk:1.12.0"
#molecule - gradle = "app.cash.molecule:molecule-gradle-plugin:0.1.0"
#molecule - runtime = "app.cash.molecule:molecule-runtime:0.1.0"

okhttp - loggingInterceptor = { module = "com.squareup.okhttp3:logging-interceptor", version.ref = "okhttp" }
okhttp - okhttp = { module = "com.squareup.okhttp3:okhttp", version.ref = "okhttp" }

retrofit - converter = { module = "com.squareup.retrofit2:converter-moshi", version.ref = "retrofit" }
retrofit - retrofit = { module = "com.squareup.retrofit2:retrofit", version.ref = "retrofit" }

robolectric = "org.robolectric:robolectric:4.7.3"

sqldelight - coroutines = { module = "com.squareup.sqldelight:coroutines-extensions-jvm", version.ref = "sqldelight" }
sqldelight - driver = { module = "com.squareup.sqldelight:android-driver", version.ref = "sqldelight" }
sqldelight - gradle = { module = "com.squareup.sqldelight:gradle-plugin", version.ref = "sqldelight" }

timber = "com.jakewharton.timber:timber:5.0.1"
```

</details>

Gradle will generate a `libs` property accessible from all your project modules.

- `libs.<library>` to get a dependency
- `libs.versions.<version>` to get a version

---
⏭️ [Now let's create our plugins project and included it in our app](3b-included-build.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)