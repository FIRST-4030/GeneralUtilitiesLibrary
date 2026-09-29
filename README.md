# How to add the GeneralUtilitiesLibrary to a Project

In BOTH the build.gradle (for the TeamCode module *specifically*) file AND the build.dependencies.gradle file, make sure this code is present in the `repositories` block:

```groovy
repositories {  
    ...
    
    mavenCentral()  
    maven { url 'https://jitpack.io' }  
    
    ...
}
```

...and then add the implementation line for the GeneralUtilitiesLibrary to the `dependencies` block in build.gradle (in the TeamCode module), making sure to specify the version number:

```groovy
dependencies {  
    ...
  
    // Specify which version of the library you're using here
    implementation 'com.github.FIRST-4030:GeneralUtilitiesLibrary:1.0.6'  
    
    ...
}
```

That should be it! Now you can use the classes provided by GeneralUtilitesLibrary in your opmodes.

# Configuring the GeneralUtilitiesLibrary Project to Publish to JitPack properly

There are a lot of things you seem to have to do before JitPack will play nice with a custom library, but it doesn't take very long. This section could be a pretty good reference if we want to make another library. 

## Add JitPack.io to the `repositories` Block in both build.gradle Files

Make sure this block is present in build.gradle (GeneralUtilities module):

```groovy
repositories { 
    ...
    
    mavenCentral()  
    maven { url "https://jitpack.io" }  
    
    ...
}
```

...and make sure *this* block is present in build.gradle (GeneralUtilitiesLibrary project):

```groovy
allprojects {  
    repositories {  
        ...
        
        mavenCentral()  
        maven { url "https://jitpack.io" }  
        
        ...
    }  
}
```

## Adding the `maven-publish` Plugin to Gradle

Maven-publish is what allows the library to be built and hosted on JitPack. Without it JitPack will fail to build the project, and Android Studio won't be able to download the library and use it as a dependency.

Put this block at the top of build.gradle (specifically the GeneralUtilities module):

```groovy
plugins {
    id 'maven-publish'
}
```

## Changing the `implementation` Lines to `api` Lines

Edit the dependencies block in build.gradle (the GeneralUtilities module specifically).

Changing these lines to api seems to give better autocomplete for me.

Before:

```groovy
dependencies {  
    implementation 'org.firstinspires.ftc:Inspection:11.0.0'  
    implementation 'org.firstinspires.ftc:Blocks:11.0.0'  
    implementation 'org.firstinspires.ftc:RobotCore:11.0.0'  
    implementation 'org.firstinspires.ftc:RobotServer:11.0.0'  
    implementation 'org.firstinspires.ftc:OnBotJava:11.0.0'  
    implementation 'org.firstinspires.ftc:Hardware:11.0.0'  
    implementation 'org.firstinspires.ftc:FtcCommon:11.0.0'  
    implementation 'org.firstinspires.ftc:Vision:11.0.0'  
    implementation 'androidx.appcompat:appcompat:1.2.0'  
}
```

After:

```groovy
dependencies {  
    api 'org.firstinspires.ftc:Inspection:11.0.0'  
    api 'org.firstinspires.ftc:Blocks:11.0.0'  
    api 'org.firstinspires.ftc:RobotCore:11.0.0'  
    api 'org.firstinspires.ftc:RobotServer:11.0.0'  
    api 'org.firstinspires.ftc:OnBotJava:11.0.0'  
    api 'org.firstinspires.ftc:Hardware:11.0.0'  
    api 'org.firstinspires.ftc:FtcCommon:11.0.0'  
    api 'org.firstinspires.ftc:Vision:11.0.0'  
    api 'androidx.appcompat:appcompat:1.2.0'  
}
```

## Adding the `afterEvaluate` Block

In the same file as above (build.gradle in the GeneralUtilities module), add this block at the bottom of the file:

```groovy
afterEvaluate {  
    publishing {  
        publications {  
            release(MavenPublication) {  
                from components.release  
                groupId = 'com.github.FIRST-4030'  
                artifactId = 'GeneralUtilitiesLibrary'  
                version = '1.0.6' // This should match the tag version
            }  
        }
    }
}
```

## Finally: Creating a Git Tag for the Project Version

This final step lets us tag a specific version of the library to use in our project.

1. Pick a version number you're going to use for this version, and make sure to use that number in the afterEvaluate block (see above).
2. Commit all your changes in the library.
3. Run this command in the terminal: `git tag -a v{Your version number here} -m "Release version {Your version number here}"`
4. Push to GitHub in Android Studio, making sure to enable the "Push Tags" option at the bottom left of the popup.

Should be all set after that. Use that version number you picked when you use the library in a project.

---