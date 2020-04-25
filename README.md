# ito React Native App

![Android Build](https://github.com/ito-org/react-native-app/workflows/Android/badge.svg)
![iOS](https://github.com/ito-org/react-native-app/workflows/iOS/badge.svg)
[![Android Downloads](https://img.shields.io/github/downloads/ito-org/react-native-app/total?color=%237dc6b6&logo=Android)](https://github.com/ito-org/react-native-app/releases/latest)
![License](https://img.shields.io/github/license/ito-org/react-native-app)

## Try it out

You can find Android alpha releases on our releases page. [Click here to find the latest release.](https://github.com/ito-org/react-native-app/releases/latest) For the app to recognize other devices, make sure to enable Bluetooth.

## Architecture

    +--------------------------------+
    |                                |
    |       react-native-app         |
    |                                |
    | +----------------------------+ |     +----------------+
    | |                            | |     |                |
    | | react-native-ito-bluetooth +------>+   api-backend  |
    | |          library           | |     |                |
    | +----------------------------+ |     +----------------+
    +--------------------------------+

- `react-native-app`: React Native app implementation
- [`react-native-ito-bluetooth`](https://github.com/ito-org/react-native-ito-bluetooth): React Native library, which contains native code for handling bluetooth and network communication with the backend. The library is used by the app as an NPM dependency. The exact version is pinned in `package-lock.json`.
- [`api-backend`](https://github.com/ito-org/api-backend): This is the hosted backend implementation. This is work in progress and will replace the old [`backend-sqlite`](https://github.com/ito-org/backend-sqlite) implementation, that is currently used by the app.

## Development

### User flow

Take a look at [the user flow on Figma](https://www.figma.com/file/fcDmzECUHFCrem9NBrzZSv/Ito-App?node-id=225%3A218). We have implemented the major part of those screen mockups.

Also [try our click dummy](https://www.figma.com/proto/fcDmzECUHFCrem9NBrzZSv/Ito-App?node-id=225%3A245&viewport=994%2C417%2C0.3995259702205658&scaling=scale-down) for a quick demonstration.

### Android

Select "React Native CLI Quickstart" and your OS on [Environment Setup React-Native](https://reactnative.dev/docs/environment-setup).

Don't forget to install the npm dependencies:

```bash
npm install
```

This will also trigger the `postinstall` script defined in `package.json`, which will run:

- `jetify` to do an [AndroidX migration for dependencies](https://github.com/mikehardy/jetifier#do-you-need-this)
- `react-native link` to [link library dependencies](https://reactnative.dev/docs/linking-libraries-ios)

To start the React Native Metro server use:

```bash
npm run start
```

To build the source and install use:

```bash
npm run android
```

To build an APK, run:

```sh
npm run build:android
```

The APK is generated to `android/app/build/outputs/apk/release/app-release.apk`

### iOS

To start the React Native Metro server use:

```bash
npm run start
```

To build the source and install use:

```bash
npm run ios
```

### Run the app with a locally modified library

Check out `react-native-ito-bluetooth` and execute:

```sh
npm link
```

Then, go to your local checkout of `react-native-app` and execute:

```sh
npm link react-native-ito-bluetooth
```

This will override `node_modules/react-native-ito-bluetooth` with a symbolic link to your local checkout of `react-native-ito-bluetooth`. Now your local checkout of the library will appear to be an installed dependency in the `node_modules` folder of the app and will be recognized as such during build.

Keep in mind, that after modifying library code, you might need to remove the `build` folders (e.g. `android/build`) in your `react-native-ito-bluetooth` checkout. This will force the app project to rebuild the library.

Now you can build and start the app and it will use your locally modified library code:

```sh
npm run start

npm run android
npm run ios
```

### Wire the app to a different server

Client/server communication is handled by the library, so you need to locally modify the library code (see previous section).

Check out the backend and run it locally (e.g. using `docker-compose`).

Find the `BASE_URL` environment variable in the code of `react-native-ito-bluetooth` and change it to your local server's url.

## Release process

The CI pipeline is implemented using GitHub Actions.

### Android

- Build (runs on every push):
  - install the Android SDK
  - build an APK
- Deploy (runs only on git tags):
  - create a [GitHub release](https://github.com/ito-org/react-native-app/releases)
  - Upload the APK as release asset

### iOS

[Not yet implemented](https://github.com/ito-org/react-native-app/issues/100)
