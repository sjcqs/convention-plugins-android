# Improve your multi-module app build configuration with convention plugins
## Gotchas & further thoughts

### Gotchas
- Using gradle scripts (`src/main/java/<plugin-id>.build.gradle.kts`) to generate plugins (cf [issue on android/nowinandroid](https://github.com/android/nowinandroid/issues/39) with some profiling I made)
- Every tip from this talk [🎬 Improve Build Times in Less Time by Zac Sweers, Slack](https://www.youtube.com/watch?v=CkKtCuqqxHs)

### Further thoughts
- Publishing those plugins to an internal repository should also improve configuration and build time.
  - Switch between the internal repository (when working on the app) and the included build (when working on the build config)
- You don't have to follow this talk to the letter:
    - Speak with your team:
        - How many modules do you have ?
        - Is build configuration a pain point ?
        - Would you be able to maintain those plugins ? (and teach how ?)
    - Your app architecture should scale with your team and its needs and pain points.
    - I'm not perfect nor an expert on the subject. There might be things that could be done in a better way.
---
⏭️ [Finally some references and peoples to follow](6-references.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)