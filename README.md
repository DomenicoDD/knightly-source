# Knightly app source

Knightly is an independent chess training app for iOS and Android. The public website is [trainchess.app](https://trainchess.app); support is support@trainchess.app.

This repository publishes corresponding app/native-engine source packages. Source availability does not mean a store has approved a beta, accepted public testers or released the app. The development/backend repository remains private.

## Source releases

| Platform / version | Source checkpoint | Source package |
| --- | --- | --- |
| iOS TestFlight 1.0.0 (1) | `6fd992111458d4be5c580dfb7f8690cde12ee8bd` | [iOS source release](../../releases/tag/ios-1.0.0-1-source) |
| Android 1.0.0 (2) | `34a02bb8eb0cea5cd527a43db01213d3ec7075a7` | [Android source release](../../releases/tag/android-1.0.0-2-source) |

- iOS: `Knightly-testflight-source-6fd9921.zip`, 65,943,628 bytes, 303 source/build-input files. SHA256 `4ba343f6e1d0e52ccd8090fb0969e3f7550ee9c731d33ee8b8d9abbb1ac9dcc4`. Existing release/archive preserved.
- Android: `Knightly-android-code2-source-34a02bb-r3.zip`, 66,133,909 bytes, 307 files. SHA256 `01202d67547c0de1e320cc27fe27d3d4cd2c528088530067d7a6ff23eff8eeb3`. The app runtime remains checkpoint `f7bdd6f`; the later source checkpoint includes the exact versioned Android build tooling.

Each archive includes app/engine source, bundled training data and evaluation networks, platform bridges, tests, package lockfile and its generated native project. SOURCE_MANIFEST.json records individual input hashes. Credentials, signing keys/profiles, private backend implementation, raw QA and operator documents are excluded. Android BUILD-ENV.example contains only the public client SDK/build settings already embedded in that app.

## Build Android 1.0.0 (2)

Use Node 22, pnpm 10.33.0, JDK 17, Android SDK/platform 37, Build Tools 37.0.0, NDK 27.1.12297006 and CMake 3.22.1. Target SDK is 36, minimum SDK 24. Extract the Android package, run `pnpm install --frozen-lockfile`, set JAVA_HOME/ANDROID_HOME and copy BUILD-ENV.example to .env. Reuse the included Android project for this reproduction.

```sh
cd android
./gradlew :app:bundleRelease -I ../scripts/android-store.gradle \
  -PknightlyVersionCode=2 -PreactNativeArchitectures=arm64-v8a,x86_64 \
  -Pandroid.enableMinifyInReleaseBuilds=true --console=plain --max-workers=2
```

This produces an unsigned bundle without a signing key. Use your own signing setup for installation/distribution; do not upload another code-2 binary to Google Play. A separate isolated restore/build passes all three TypeScript targets, 129 app tests and 12 native-tool checks. Both DEX files, JavaScript and both evaluation networks are byte-identical to the frozen signed code-2 bundle. Thirty native libraries are byte-identical; the other ten differ only in their ELF build-ID notes. All forty native libraries' remaining bytes match. Debug-symbol metadata and the upload signature differ, so a whole-AAB bit-identical rebuild is not claimed. ANDROID-CODE2-SOURCE.json records the source/package identities.

## Build iOS 1.0.0 (1)

Extract the iOS source archive. Use pnpm 10.33.0 and Xcode 27.1; run `pnpm install --frozen-lockfile` at the root, then install CocoaPods dependencies in ios/ using its included Podfile.lock. Open ios/Knightly.xcworkspace, select Knightly/Release and supply your own Apple developer team/signing configuration. The local Stockfish module and both NNUE networks are bundled. Public runtime configuration is documented in .env.example; client builds must never contain private backend credentials.

Core training and local progress work without an account. These beta environments do not enable public accounts, production ad units or verified paid cloud sync. Hosted-account, website and separate platform commands in the original package.json are development references; excluded infrastructure is not supplied by the source packages.

## License and notices

Copyright © 2026 Domenico Di Donna for original Knightly application code. That code is available under **GPL-3.0-or-later**: you may use, modify and redistribute it under version 3 of the GNU General Public License, or any later version, without warranty. See [LICENSE](LICENSE). The owner approved original app/engine source publication on 30 October 2026.

Stockfish 17.1, the native integration and upstream notices retain GPL-3.0-or-later. Third-party assets/software retain their own licenses and attributions in THIRD_PARTY.md, the engine README and upstream files. NNUE data retains its documented CC0 terms. This grant does not imply ownership or relicensing of third-party components, names or trademarks. Private backend implementation is outside these app-source packages and this grant.

The frozen iOS archive predates the owner's approval and retains the Expo starter MIT notice as its root LICENSE. That notice is reproduced separately as [NOTICE-EXPO.txt](NOTICE-EXPO.txt); it does not replace the GPL grant for original Knightly code or the engine. Its historical preparation-status note is superseded by the approved iOS release. The Android archive has GPL text at LICENSE and preserves the Expo notice separately at NOTICE-EXPO.txt.
