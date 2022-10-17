# Improve your multi-module app build configuration with convention plugins
## References

Those references should not be blindly followed. Some of them are from people working in large companies 
with multiples people working on a single app and even people dedicated to build configuration.

Take what you need, depending on the scale of your app and your team. 

~~Don't be a fanboy 🤩~~

### Herding elephants 🐘
Some feedbacks and best practices on the Android's build configuration by [Tony Robalik](https://twitter.com/autonomousapps) who is working on build and tooling at Square

🔗 https://developer.squareup.com/blog/herding-elephants/

### Slack gradle plugins 👀
Slack [started open sourcing](https://slack.engineering/developing-in-the-open/) part of their build tools on Github. 

🔗 https://github.com/slackhq/slack-gradle-plugin

### Improve Build Times in Less Time by Zac Sweers, Slack 🎬
[A talk from Android Makers 2022](https://www.youtube.com/watch?v=CkKtCuqqxHs) with some best practices to improve
build time. 

### Now in Android project
The Android team recently open-sourced an app with some good practices to build applications. 

🔗 https://github.com/android/nowinandroid

### Proton core projects
Proton mail source code is open source. They have shared libraries in [ProtonMail/protoncore_android](https://github.com/ProtonMail/protoncore_android)

In [plugins/core/src/main/kotlin/me/proton/core/gradle](https://github.com/ProtonMail/protoncore_android/tree/main/plugins/core/src/main/kotlin/me/proton/core/gradle), they 
are defining their convention plugins. 

_IMHO: it's a bit overkill the way they did it, but it's still a good reference_

### People 👨‍💻
🐦 [Tony Robalik](https://twitter.com/autonomousapps) from Square

🐦 [Zac Sweers](https://twitter.com/ZacSweers) from Slack

### Feedbacks 👂

[Here:](https://forms.gle/fa6fKB66KG2VQV4C6)

<img src="feedback.png" alt="https://forms.gle/n7YT55VdoVTeq8JR6" title="Feedbacks"/>

### Any Questions ❓

---
🐦 [@sjcqs](https://twitter.com/sjcqs) (_feel free to reach out_)