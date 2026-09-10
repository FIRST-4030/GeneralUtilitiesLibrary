# GeneralUtilities Library

This module contains reusable FTC utilities extracted from TeamCode.

## GitHub/JitPack dependency

After a version tag such as `1.0.0` is pushed to GitHub, another FTC project can use:

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.FIRST-4030:GeneralUtilities:1.0.0'
}
```

The library uses the FTC SDK 11.0.0 APIs and keeps the original Java package names, so existing
imports such as `org.firstinspires.ftc.teamcode.Utilities.PIDController` continue to work.
