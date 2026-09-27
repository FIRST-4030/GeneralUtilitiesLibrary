# Adding the GeneralUtilities Library to a Project

In the build.gradle (for the TeamCode module *specifically*), add this block:

```groovy
repositories {  
    mavenCentral()  
    maven { url 'https://jitpack.io' }  
    google() // Needed for androidx  
}
```

...and then add the implementation for the GeneralUtilities library to the dependencies in the same file, making sure to specify the version number:

```groovy
dependencies {  
    implementation project(':FtcRobotController')  
  
    // Specify which version of the library you're using here
    implementation 'com.github.FIRST-4030:GeneralUtilitiesLibrary:1.0.6'  
}
```

That should be it!

# Configuring the GeneralUtilities Library Project so that JitPack Worked

There are a lot of things you seem to have to do before JitPack will play nice with a custom library, but it doesn't take very long. You don't need to
read this section to use the GeneralUtilies library in a project.

## Add Jitpack.io to the `repositories` Block in both build.gradle Files

Make sure this block is present in build.gradle (GeneralUtilities module):

```groovy
repositories {  
    mavenCentral()  
    maven { url "https://jitpack.io" }  
    google()  
}
```

...and make sure *this* block is present in build.gradle (GeneralUtiliesLibrary project):

```groovy
allprojects {  
    repositories {  
        mavenCentral()  
        maven { url "https://jitpack.io" }  
        google()  
    }  
}
```

## Changing the `implementation` Lines to `api` Lines

Edit the dependencies block in build.gradle (the GeneralUtilies module specifically).

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