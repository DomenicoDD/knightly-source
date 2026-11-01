# Knightly app source

Knightly is an independent chess training app for iOS and Android. The public website is [trainchess.app](https://trainchess.app); support is support@trainchess.app.

This repository publishes corresponding app/native-engine source packages. Source availability does not mean a store has approved a beta, accepted public testers or released the app. The development/backend repository remains private.

## Source releases

| Platform / version | Source checkpoint | Source package |
| --- | --- | --- |
| iOS TestFlight 1.0.0 (5) | `23bbfd5a9c0e8616185ba379ee129cf46b67a36d` | [iOS build 5 source release](../../releases/tag/ios-1.0.0-5-source) |
| iOS TestFlight 1.0.0 (4) | `9dc9b7c703fd0c317823cc0511209d4c7b49a7b2` | [iOS build 4 source release](../../releases/tag/ios-1.0.0-4-source) |
| iOS TestFlight 1.0.0 (3) | `5dac296c065c08d4836112f9b8c9f92998059f63` | [iOS build 3 source release](../../releases/tag/ios-1.0.0-3-source) |
| iOS 1.0.0 (2) | `7f9249090eae9a4e2e486ddde8c5caf8eca05309` | [iOS build 2 source release](../../releases/tag/ios-1.0.0-2-source) |
| iOS TestFlight 1.0.0 (1) | `6fd992111458d4be5c580dfb7f8690cde12ee8bd` | [iOS source release](../../releases/tag/ios-1.0.0-1-source) |
| Android 1.0.0 (2) | `34a02bb8eb0cea5cd527a43db01213d3ec7075a7` | [Android source release](../../releases/tag/android-1.0.0-2-source) |

- iOS build 5: `Knightly-ios-build5-source-23bbfd5.zip`, 67,484,472 bytes, 304 files. SHA256 `1049d66c116b1cf51b3c587e77a3fcdebdc3584b86326ed6b7032cae461b1249`.
- iOS build 4: `Knightly-ios-build4-source-9dc9b7c.zip`, 65,933,993 bytes, 296 files. SHA256 `18c119ae66a049cc9c8fc266e1670e50172e1e491edebbf949ec1175b275e4ba`.
- iOS build 3: `Knightly-ios-build3-source-5dac296.zip`, 65,932,498 bytes, 294 files. SHA256 `29fac06f43c02b7e81222358eb3629db9ef3f577ca28c9b07fc50b7de46aadb9`.
- iOS build 2: `Knightly-ios-build2-source-7f92490.zip`, 65,929,883 bytes, 293 files. SHA256 `b7f8a41c91e358378b467725899659c77f31fab411faf6506441ebaf0d70877b`.
- iOS build 1: `Knightly-testflight-source-6fd9921.zip`, 65,943,628 bytes, 303 source/build-input files. SHA256 `4ba343f6e1d0e52ccd8090fb0969e3f7550ee9c731d33ee8b8d9abbb1ac9dcc4`. Existing release/archive preserved.
- Android: `Knightly-android-code2-source-34a02bb-r3.zip`, 66,133,909 bytes, 307 files. SHA256 `01202d67547c0de1e320cc27fe27d3d4cd2c528088530067d7a6ff23eff8eeb3`. The app runtime remains checkpoint `f7bdd6f`; the later source checkpoint includes the exact versioned Android build tooling.

Each archive includes app/engine source, bundled training data and evaluation networks, platform bridges, tests, package lockfile and its generated native project. SOURCE_MANIFEST.json records individual input hashes. Credentials, signing keys/profiles, private backend implementation, raw QA and operator documents are excluded. The Android and iOS build 2/3/4/5 BUILD-ENV.example files contain only the public client SDK/build settings already embedded in that app.

## Build Android 1.0.0 (2)

Use Node 22, pnpm 10.33.0, JDK 17, Android SDK/platform 37, Build Tools 37.0.0, NDK 27.1.12297006 and CMake 3.22.1. Target SDK is 36, minimum SDK 24. Extract the Android package, run `pnpm install --frozen-lockfile`, set JAVA_HOME/ANDROID_HOME and copy BUILD-ENV.example to .env. Reuse the included Android project for this reproduction.

```sh
cd android
./gradlew :app:bundleRelease -I ../scripts/android-store.gradle \
  -PknightlyVersionCode=2 -PreactNativeArchitectures=arm64-v8a,x86_64 \
  -Pandroid.enableMinifyInReleaseBuilds=true --console=plain --max-workers=2
```

This produces an unsigned bundle without a signing key. Use your own signing setup for installation/distribution; do not upload another code-2 binary to Google Play. A separate isolated restore/build passes all three TypeScript targets, 129 app tests and 12 native-tool checks. Both DEX files, JavaScript and both evaluation networks are byte-identical to the frozen signed code-2 bundle. Thirty native libraries are byte-identical; the other ten differ only in their ELF build-ID notes. All forty native libraries' remaining bytes match. Debug-symbol metadata and the upload signature differ, so a whole-AAB bit-identical rebuild is not claimed. ANDROID-CODE2-SOURCE.json records the source/package identities.

## Build iOS 1.0.0 (5)

Use the build 5 archive with the same toolchain and unsigned-archive command described below for build 2. The supplied project identifies build 5. It includes PR #2: one continuous phone session scroll, pinned Hint/Their plan/Undo/Flip controls, shared chips/confirmation controls and phone bottom sheets. The existing ad-break fix is retained.

This TestFlight build continues to use visibly marked Google demo test ads. BUILD-ENV.example records that flag; production unit fields, debug geography and diagnostics stay off. The exact signed IPA SHA256 is `0e960cf78cd9798ddd890472e583cc1222e957c761df4ea0b653f6d4743af317`.

The local arm64 iPhoneOS archive and signed export pass. All 57 first-party JavaScript inputs match frozen source `23bbfd5`; both networks, privacy metadata, 50 attribution entries and the existing signing profile are verified. Source ZIP CRC, all 303 manifest inputs and private-value scans pass; an anonymous download matches its published SHA256. The merged private project passes 162 app tests, three TypeScript targets, 20 native-tool checks and 43 account/website tests. An independent native rebuild from this public source ZIP has not been performed; whole-IPA bit reproducibility is not claimed. Apple accepts the upload and reports build 5 as VALID on 1 November 2026. Device gameplay, production ads and paid cloud sync require their own runtime verification.

## Build iOS 1.0.0 (4)

Use the build 4 archive with the same toolchain and unsigned-archive command described below for build 2. The supplied native project identifies build 4. This update removes overlapping native training/ad-loading modals, waits for the training screen to leave before presenting an SDK ad, clears loading after failures and skips interstitials when no unit is configured.

**This TestFlight build uses visibly marked Google demo test ads.** Its BUILD-ENV.example sets the test-ad flag on; production unit fields, debug geography and diagnostics remain off. This is not evidence of production ad serving. The exact signed IPA SHA256 is `153b0cf822bdc214fe4985cf14d75c757ffc9b8b2505d52f8f2d6eb2a8d85be9`.

An actual unauthenticated download of the published ZIP now passes an independent frozen dependency install, all three TypeScript targets, 162 app tests, 12 included native-tool checks and the real Stockfish host integration tests. The supplied native project compiles an unsigned arm64 iPhoneOS 1.0.0 (4) archive. All 2,963 mapped JavaScript source inputs match the distributed build in content and order, including all 56 first-party inputs. Both evaluation networks, privacy metadata and 50 attribution entries match. Normal CocoaPods installation refreshes only ExpoModulesCore's path-dependent checksum; the complete pinned dependency graph and supplied source code remain unchanged. Rebuilt Hermes bytecode and the native executable differ, so whole-IPA bit reproducibility is not claimed. Archive CRC, every supplied-file hash and private-pattern/known-private-value scans pass. The published ZIP and distributed signed IPA are unchanged.

On 31 October 2026, Fastlane uploads the exact signed build 4. Apple processing/export compliance pass, and both internal and open-beta states report Testing. The public invitation remains [TestFlight](https://testflight.apple.com/join/WWR36spt). The reported build 3 freeze still requires confirmation on the owner's iPhone after updating; current-build iPhone gameplay and paid cloud sync are separate verification gates.

## Build iOS 1.0.0 (3)

Use the build 3 archive with the same toolchain and unsigned-archive command described below for build 2. The supplied native project identifies build 3. This update includes saved-game capacity checks and recovery of an earned unlock after storage failure. Its signed IPA SHA256 is `a3d2573540cb97667b4dea7f50b55f6da959e5800ea8b3a36cf4d13b46f03b4d`.

The local iPhoneOS archive and signed export pass; all 55 first-party JavaScript bundle inputs match the frozen Git source. Both evaluation networks, privacy/attribution metadata, existing provisioning profile and distribution certificate are verified. The prepared source passes 158 app tests, three TypeScript targets and 12 included native-tool checks using the existing installed dependencies. ZIP CRC, every supplied-file hash and private-pattern/known-private-value scans pass. Release-packaging-only credential fixtures are excluded. An independent native rebuild of this build 3 ZIP has not been performed; whole-IPA bit reproducibility is not claimed. On 31 October 2026, the exact signed build 3 is uploaded through Fastlane, passes Apple processing/export compliance and reports Testing in both the internal group and Knightly Open Beta. The public invitation is [TestFlight](https://testflight.apple.com/join/WWR36spt). Current-build iPhone gameplay and paid cloud sync still require their own runtime verification.

## Build iOS 1.0.0 (2)

Extract the build 2 archive. Use Node 22, pnpm 10.33.0 and Xcode 27.1 / iPhoneOS SDK 27.1. Run `pnpm install --frozen-lockfile`, copy BUILD-ENV.example to .env, then run `pod install` inside ios. Reuse the supplied native project. An unsigned archive can be built with:

```sh
xcodebuild -workspace ios/Knightly.xcworkspace -scheme Knightly \
  -configuration Release -sdk iphoneos -destination 'generic/platform=iOS' \
  -archivePath build/Knightly.xcarchive CODE_SIGNING_ALLOWED=NO \
  ONLY_ACTIVE_ARCH=YES ARCHS=arm64 archive
```

An independent restore, all three TypeScript targets, 153 app tests, 12 native-tool checks and the arm64 iPhoneOS archive pass. All 2,962 source-map inputs, including 55 first-party files, match the signed build's source content. Both NNUE networks, privacy metadata and 50 attribution entries match. CocoaPods refreshes ExpoModulesCore's absolute-path-dependent checksum after relocation; the complete pinned dependency graph remains unchanged. Rebuilt Hermes bytecode and native executable are not byte-identical; whole-IPA bit reproducibility is not claimed. This source release does not establish Apple processing or TestFlight availability for build 2.

## Build iOS 1.0.0 (1)

Extract the iOS source archive. Use pnpm 10.33.0 and Xcode 27.1; run `pnpm install --frozen-lockfile` at the root, then install CocoaPods dependencies in ios/ using its included Podfile.lock. Open ios/Knightly.xcworkspace, select Knightly/Release and supply your own Apple developer team/signing configuration. The local Stockfish module and both NNUE networks are bundled. Public runtime configuration is documented in .env.example; client builds must never contain private backend credentials.

Core training and local progress work without an account. These beta environments do not enable public accounts, production ad units or verified paid cloud sync. Hosted-account, website and separate platform commands in the original package.json are development references; excluded infrastructure is not supplied by the source packages.

## License and notices

Copyright © 2026 Domenico Di Donna for original Knightly application code. That code is available under **GPL-3.0-or-later**: you may use, modify and redistribute it under version 3 of the GNU General Public License, or any later version, without warranty. See [LICENSE](LICENSE). The owner approved original app/engine source publication on 30 October 2026.

Stockfish 17.1, the native integration and upstream notices retain GPL-3.0-or-later. Third-party assets/software retain their own licenses and attributions in THIRD_PARTY.md, the engine README and upstream files. NNUE data retains its documented CC0 terms. This grant does not imply ownership or relicensing of third-party components, names or trademarks. Private backend implementation is outside these app-source packages and this grant.

The iOS build 1 archive predates the owner's approval and retains the Expo starter MIT notice as its root LICENSE. That notice is reproduced separately as [NOTICE-EXPO.txt](NOTICE-EXPO.txt); it does not replace the GPL grant for original Knightly code or the engine. Its historical preparation-status note is superseded by the approved iOS release. The Android and iOS build 2/3/4 archives have GPL text at LICENSE and preserve the Expo notice separately at NOTICE-EXPO.txt.