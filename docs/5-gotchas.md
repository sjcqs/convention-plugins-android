# Improve your multi-module app build configuration with convention plugins

## Gotchas & further thoughts

### Gotchas

#### Gradle scripts
It's possible to write gradle script in `src/main/java/<plugin-id>.build.gradle.kts`, Gradle will generate a script whose id is `<plugin-id>`

Don't do this ([#39](https://github.com/android/nowinandroid/issues/39) on [android/nowinandroid](https://github.com/android/nowinandroid))

|                      | scripts plugins | code plugins |
|:--------------------:|:---------------:|--------------|
|   Total Build Time   |     25.373s     | 11.205s      |
| Configuring Projects |     12.724s     | 0.765s       |

#### Dependencies (~interface segregation principle)
Limit the number of dependencies (and plugins) declared in your conventions plugins.  
(cf [feature convention plugin](../wordle-android/plugins/src/main/java/fr/sjcqs/AndroidFeaturePlugin.kt))

### Further thoughts

You could publishing those plugins to an internal repository should also improve configuration and build time. But it comes at a few costs:
- Setup your CI to publish those plugins
- Version the plugins
- Switching between the internal repository and the included build when working on the build config

Conventions plugins can be use in any multi-module projects using Gradle (not just Android apps.) 

### Notes
You don't have to follow this talk to the letter

- Speak with your team:
    - How many modules do you have ?
    - Is build configuration a pain point ?
    - Would you be able to maintain those plugins ? (and teach how ?)
- _I'm not perfect nor an expert on the subject. There might be things that could be done in a better way._

---
⏭️ [Finally some references and peoples to follow](6-references.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)