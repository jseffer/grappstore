# Grappstore

Grappstore is an SCS-maintained fork of the GrapheneOS App Store for a curated
application catalog on SCS devices running GrapheneOS.

This repository contains the rebranded Android client. Its Base44 backend and
administration app are available as the separate GrAppStore project. The client
loads signed metadata from
`https://grappstore.sc-solutions.de/functions/repository-metadata`.
The long-term SCS repository public key and key version are embedded in the
client. New catalog apps and releases are signed with the same repository key;
they do not require a client update or a new key pair.

## Identity

- Application name: Grappstore
- Android application ID: `app.grappstore`
- Publisher: SCS

## Build

```sh
./gradlew build
```

The project requires the Android SDK versions configured in
`app/build.gradle.kts`. Release signing can be configured with an untracked
`keystore.properties` file; never commit signing keys or passwords.

## Installation and updates

Published APKs are attached to GitHub releases. Obtainium users can add
`https://github.com/jseffer/grappstore` as a GitHub source. The same releases
are imported into the GrAppStore catalog, allowing Grappstore to update itself.

Create a tag matching the Android version, for example `v36`, to run the signed
release workflow. It requires the GitHub Actions secrets
`ANDROID_KEYSTORE_BASE64`, `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_ALIAS`, and
`ANDROID_KEY_PASSWORD`. The same Android signing key must be used for every
release; replacing it would prevent installed copies from updating.

## Device integration

`app.grappstore` is a separate application identity and must be signed with an
SCS-controlled release key. It cannot update or inherit the trust of
`app.grapheneos.apps`. Privileged unattended installation requires explicit
integration of the Grappstore package and signing certificate into the device
OS; without that integration, Android installation confirmations apply.

For a regular, non-privileged installation, Grappstore opens Android's
per-source permission page before the first install. This operating-system
permission does not replace repository verification: metadata is accepted only
after a successful Ed25519/Signify signature check with the embedded SCS key.

If SCS distributes a modified operating-system build rather than the official
GrapheneOS release, it must be presented under distinct SCS branding as a
separate OS based on GrapheneOS.

## License and attribution

The original project is Copyright © 2021-2026 GrapheneOS. Grappstore
modifications are Copyright © 2026 SCS. The software remains available under
the MIT License in [LICENSE](LICENSE). GrapheneOS is a third-party project and
does not publish or endorse Grappstore.
