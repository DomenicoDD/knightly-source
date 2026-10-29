# Knightly app source

Knightly is an independent chess training app for iOS and Android. The public website is [trainchess.app](https://trainchess.app); support is support@trainchess.app.

This repository publishes the corresponding app/native-engine source snapshot for the first locally compiled iOS TestFlight candidate, version **1.0.0 (1)**. Download **Knightly-testflight-source-6fd9921.zip** from [Releases](../../releases). A source release does not mean Apple has approved the beta or that a public testing link is available.

## Source identity

- Source checkpoint: `6fd992111458d4be5c580dfb7f8690cde12ee8bd`.
- Archive: `Knightly-testflight-source-6fd9921.zip`, 65,943,628 bytes, 303 source/build-input files.
- SHA256: `4ba343f6e1d0e52ccd8090fb0969e3f7550ee9c731d33ee8b8d9abbb1ac9dcc4`.
- The archive includes app and engine source, bundled training data and evaluation networks, platform bridges, tests, package/native lockfiles and the generated iOS project. `SOURCE_MANIFEST.json` records individual file hashes.
- Credentials, signing keys/profiles, private backend implementation, raw QA records and operator documents are excluded. The development repository remains private.

## Build the iOS app

Extract the source archive. Use pnpm 10.33.0 and Xcode 27.1; run `pnpm install --frozen-lockfile` at the root, then install the CocoaPods dependencies in `ios/` using its checked-in `Podfile.lock`. Open `ios/Knightly.xcworkspace`, select the Knightly scheme and Release configuration, and supply your own Apple developer team/signing configuration for device distribution. The local Stockfish module and both NNUE networks are bundled. Public runtime configuration is documented in `.env.example`; a client build must never contain private backend credentials.

Core training and local progress work without an account. This initial beta does not enable public accounts, production ad units or verified paid cloud sync. Account/backend scripts and tests included as app-development references do not supply the excluded hosted service.

## License and notices

Copyright © 2026 Domenico Di Donna for original Knightly application code. That code is made available under **GPL-3.0-or-later**: you may use, modify and redistribute it under version 3 of the GNU General Public License, or any later version, without warranty. See [LICENSE](LICENSE). The owner approved this app-source publication on 30 October 2026.

Stockfish 17.1, the native integration and their upstream notices retain GPL-3.0-or-later. Third-party assets/software retain their own licenses and attributions recorded in the archive’s `THIRD_PARTY.md`, engine README and upstream source files. NNUE data retains its documented CC0 terms. The GPL grant does not imply ownership or relicensing of third-party components, names or trademarks.

The frozen archive was prepared before the owner’s source-publication approval. Its root `LICENSE` is the retained Expo starter MIT notice, reproduced here as [NOTICE-EXPO.txt](NOTICE-EXPO.txt); it does not replace the GPL grant for original Knightly application code or the engine. The archive’s historical “decision pending/unpublished” preparation note is superseded by this approved source release. Private backend implementation is not part of this source publication or license grant.
