# Minimal Android app

Inspired by https://github.com/czak/minimal-android-project.
Used newer gradle standards and build tool versions to make it run well in modern Android Studio (August 2023).

## Build the app

```
./gradlew installRelease
```


## App signing

A keystore is available here for testing (`my.keystore`). It was created as described [here](https://stackoverflow.com/questions/10930331/how-to-sign-an-already-compiled-apk). There are 2 keys inside the keystore. The keystore password is `android`.

* `android` - valid into 2026
* `android-expired` - intentionally [expired](https://stackoverflow.com/questions/14052428/generate-an-expired-ssl-certificate-with-keytool) on Aug 19, 2023.


```
keytool -genkey -v -keystore my.keystore -keyalg RSA -keysize 2048 -validity 1000 -alias android-expired
keytool -genkey -v -keystore my.keystore -keyalg RSA -keysize 2048 -validity 1 -startdate -2d -alias android-expired
```

You can use it to sign a build release version of the APK like so:

```
zipalign -p 4 app/build/outputs/apk/release/app-release.apk app-aligned.apk
zipalign -c 4 app-aligned.apk                  // check
apksigner sign --ks-key-alias android --ks my.keystore app-aligned.apk
apksigner verify app-aligned.apk               // check
keytool -printcert -jarfile app-aligned.apk    // check
```

## APKs

A number of sample pre-built APKs that are useful for testing are provided the `/APKs` directory

* 