# Improve your multi-module app build configuration with convention plugins
## A multi-module architecture

<div style="margin-left: auto;margin-right: auto; width: fit-content">

``` mermaid
flowchart TD
%% Nodes
    app(<small><i>application</i></small><br/><b>APP</b>)
    feature1(<small><i>android library</i></small><br/><b>FEATURE 1</b>)
    feature2(<small><i>android library</i></small><br/><b>FEATURE 2</b>)
    feature3(<small><i>android library</i></small><br/><b>FEATURE 3</b>)
    subgraph data1 [DATA 1]
        direction LR
        api1(<small><i>jvm</i></small><br/><b>API</b>)
        impl1(<small><i>android library</i></small><br/><b>IMPL</b>)
    end
    subgraph data2 [DATA 2]
        direction LR
        api2(<small><i>jvm</i></small><br/><b>API</b>)
        impl2(<small><i>android library</i></small><br/><b>IMPL</b>)
    end
    subgraph data3 [DATA 3]
        direction LR
        api3(<small><i>jvm</i></small><br/><b>API</b>)
        impl3(<small><i>jvm</i></small><br/><b>IMPL</b>)
    end
    subgraph data4 [DATA 4]
        direction LR
        api4(<small><i>jvm</i></small><br/><b>API</b>)
        impl4(<small><i>android library</i></small><br/><b>IMPL</b>)
    end

%% Links
    app --> feature1
    app --> feature2
    app --> feature3
    
   feature1 --> data1
   feature1 --> data2
   feature2 --> data2
   feature3 --> data2
   feature3 --> data3
   feature3 --> data4
   
%% Styles
   classDef application fill:#46f0b4,color:#000
   classDef android-library fill:#6df261,color:#000
   classDef jvm fill:#61cef2,color:#000
   
   class app application
   class feature1,feature2,feature3,impl1,impl2,impl4 android-library
   class api1,api2,api3,api4,impl3 jvm
```
</div>

- Each module has its own gradle configuration file (`build.gradle`)
- `com.android.library` and `com.android.applications` plugins should be configured the same way for each modules
- Feature modules will be configured the same way (with extra dependencies and plugins)
- **⌘+C ⌘+V** 🙈

---
⏭️ [Let's fix this ](2-objectives.md)

🐦 [@sjcqs](https://twitter.com/sjcqs)