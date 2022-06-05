# Improve your multi-module app build configuration with convention plugins

## Why should my app code use multiple modules

- ~~\<insert big company name\> is doing it~~
- Reduces the build time: 
  - api/impl modules pattern (reuse build cache)
  - Only use the necessary Gradle plugins in your module (android plugins are expensive)
  - Parallel compilation
- Create several apps (demo apps, free vs pro, for your back-end folks working one specific features)

---
⏭️ [ Let's see a basic multi-module architecture](1-multi-module-schema.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)
