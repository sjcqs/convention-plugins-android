# Improve your multi-module app build configuration with convention plugins
## Gotchas & further thoughts

### Gotchas
- Using gradle scripts (`src/main/java/<plugin-id>.build.gradle.kts`) to generate plugins ([#39](https://github.com/android/nowinandroid/issues/39) on [android/nowinandroid](https://github.com/android/nowinandroid))
    <details>
    <summary>⏱</summary>

    |                      | scripts plugins | code plugins |
    |:--------------------:|:---------------:|--------------|
    |   Total Build Time   |     25.373s     | 11.205s      |
    | Configuring Projects |     12.724s     | 0.765s       |
    </details>

- Limit the number of dependencies (and plugins) declared in your conventions plugins. (~interface segregation principle) 

### Further thoughts
- Publishing those plugins to an internal repository should also improve configuration and build time.
  - Switching between the internal repository (when working on the app) and the included build (when working on the build config)
- You don't have to follow this talk to the letter:
    - Speak with your team:
        - How many modules do you have ?
        - Is build configuration a pain point ?
        - Would you be able to maintain those plugins ? (and teach how ?)
    - _I'm not perfect nor an expert on the subject. There might be things that could be done in a better way._
---
⏭️ [Finally some references and peoples to follow](6-references.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)