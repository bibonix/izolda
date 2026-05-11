<img src="http://www.zold.io/logo.svg" width="92px" height="92px"/>

[![Donate via Zerocracy](https://www.0crat.com/contrib-badge/CAZPZR9FS.svg)](https://www.0crat.com/contrib/CAZPZR9FS)

[![EO principles respected here](http://www.elegantobjects.org/badge.svg)](http://www.elegantobjects.org)
[![Managed by Zerocracy](https://www.0crat.com/badge/CAZPZR9FS.svg)](https://www.0crat.com/p/CAZPZR9FS)

[![Hits-of-Code](https://hitsofcode.com/github/zold-io/izolda)](https://hitsofcode.com/github/zold-io/izolda)

This is iOS mobile wallet for ZLD coins.

## Architecture

The app targets [iOS](https://developer.apple.com/ios/) natively in [Swift](https://swift.org) rather than through a cross-platform runtime such as [React Native](https://reactnative.dev) or [Flutter](https://flutter.dev), because Zold transaction signing requires direct access to [Keychain Services](https://developer.apple.com/documentation/security/keychain_services) and the [Secure Enclave](https://support.apple.com/guide/security/secure-enclave-sec59b0b31ff/web) for private-key storage; a cross-platform abstraction layer cannot guarantee that key material stays inside the hardware security boundary.

Wallet state — the ledger of signed transactions and the current balance — is persisted locally via [Core Data](https://developer.apple.com/documentation/coredata) (backed by [SQLite](https://www.sqlite.org)) rather than synchronised with a remote server, because the [Zold protocol](https://github.com/zold-io/zold) requires the wallet file to live on the owner's device: each entry is signed with the owner's [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) private key, and delegating custody of that key to a backend would break the protocol's trust model. The Core Data model in `izolda.xcdatamodel` is currently empty, marking the boundary where the Zold ledger schema must be introduced before the Send and Receive flows can store real data.

The UI is structured as a two-scene [UIKit](https://developer.apple.com/documentation/uikit) storyboard (`Main.storyboard`): an Entrance screen that holds the app logo and a "Start" button, and a Transactions screen that displays the balance and exposes "Send", "Receive", and "Migrate" actions. Using [Interface Builder](https://developer.apple.com/xcode/interface-builder/) for navigation keeps the segue graph visible without reading code, a practical tradeoff given that the app has only two screens and the storyboard XML diff noise is not yet a real cost.

The single `ViewController` is today an empty `UIViewController` subclass — all three action buttons are wired in the storyboard but backed by no logic. A programmer adding the first real flow must keep the controller thin: Zold node communication (reading and writing wallet files over the [Zold HTTP API](https://github.com/zold-io/zold)) belongs in a dedicated service layer that the controller calls, not inside the controller itself, so that the networking code can be tested independently of [UIKit](https://developer.apple.com/documentation/uikit).

## How to Contribute

I have no idea as of yet...
