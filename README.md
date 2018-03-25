![FxA React Native SDK](fxa-react-native-sdk.jpg)


#### React Native bridge for [AppAuth-Android](https://github.com/openid/AppAuth-Android) SDK communicating with Firefox Accounts OAuth 2.0.

> Note: Only Android OS is supported at this time.

## Getting started

```sh
npm install vladikoff/fxa-react-native-app-auth --save
react-native link fxa-react-native-app-auth
```

To setup the Android project, you need to perform two steps:

1. [Install Android support libraries](#install-android-support-libraries)
2. [Add redirect scheme manifest placeholder](#add-redirect-scheme-manifest-placeholder)

#### Install Android support libraries

This library depends on the [AppAuth-Android](https://github.com/openid/AppAuth-android) project.
The native dependencies for Android are automatically installed by Gradle, but you need to add the
correct Android Support library version to your project:

1. Add the Google Maven repository in your `android/build.gradle` and update Gradle classpath to be at least `3.0.0`:
   ```
   repositories {
     google()
   }
   ```
   
   ```
    classpath 'com.android.tools.build:gradle:3.0.0'
   ```
2. Make sure the appcompat version in `android/app/build.gradle` matches the one expected by
   AppAuth. If you generated your project using `react-native init`, you may have an older version
   of the appcompat libraries and need to upgdrade:
   ```
   dependencies {
     compile "com.android.support:appcompat-v7:25.3.1"
   }
   ```
3. If necessary, update the `compileSdkVersion` to 25:
   ```
   android {
     compileSdkVersion 25
   }
   ```

4. If necessary, update the Gradle distribution (`gradle-wrapper.properties`):
   ```
   distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip
   ```


#### Add redirect scheme manifest placeholder

To [capture the authorization redirect](https://github.com/openid/AppAuth-android#capturing-the-authorization-redirect),
add the following property to the defaultConfig in `android/app/build.gradle`:

```
android {
  defaultConfig {
    manifestPlaceholders = [
      appAuthRedirectScheme: 'test-client'
    ]
  }
}
```

The scheme is the beginning of your OAuth Redirect URL, up to the scheme separator (`:`) character.

## Usage

```javascript
import { fxaAuth } from 'fxa-react-native-app-auth';

// example config
const config = {
  clientId: '22d74070a481bc73',
  redirectUrl: 'test-client://redirect',
  scopes: ['profile']
};

// use the client to make the auth request and receive the authState
try {
  const result = await fxaAuth(config);
  // result includes accessToken, accessTokenExpirationDate
} catch (error) {
  console.log(error);
}
```


## Contributors

This is a customized fork of [react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth).
