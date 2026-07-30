# Vexli Public Downloads Agent Guide

## Purpose

This public repository contains official signed Vexli Android release assets.
It exists so users can download the app without exposing the private Flutter
source repository.

Related repositories:

- Flutter app: `/home/rseann/projects/flutter/hue`
- Nuxt frontend: `/home/rseann/projects/NuxtJs/hue-ui`
- Backend: `/home/rseann/projects/Misc/catalyst`

Do not add application source code, signing keys, provider details,
credentials, environment files, build logs containing secrets, or internal
infrastructure documentation to this repository.

## Stable Asset Contract

Every latest public release must contain:

- `vexli-android.apk`: signed universal APK and primary website download.
- `vexli-android-arm64.apk`: optional smaller 64-bit APK.
- `vexli-android-arm32.apk`: optional older-device APK.
- `SHA256SUMS.txt`: SHA-256 checksums for every APK.

The Nuxt website uses `releases/latest/download/<asset-name>`. Renaming an
asset breaks the public download page even when the release itself succeeds.

## Publishing Rules

- Build artifacts only from a committed and pushed Flutter source revision.
- Run Flutter tests and build verification before upload.
- Verify package name, version code, version name, and signer certificate with
  Android build tools.
- Increase the base build number for every full release.
- Use the production signing certificate. Never upload debug-signed packages.
- Publish a normal, non-draft, non-prerelease release when it should become
  the `latest` website download.
- Include concise user-facing notes; do not mention internal provider names.

Expected package identity:

- Application ID: `com.aerk.vexli`
- Signer SHA-256:
  `33cf703467e2b8ae46f97ad9c6e420c97434234bc6edbb5db093f6e103093c5c`

## Verification

After publishing:

```sh
gh release view --repo roywalk3r/vexli-downloads
curl -sSIL https://github.com/roywalk3r/vexli-downloads/releases/latest/download/vexli-android.apk
```

The redirect chain must finish with HTTP `200`, the Android package content
type, and the expected nonzero content length. Compare downloaded files with
`SHA256SUMS.txt`.

Then verify `https://www.vexli.online/download` points to this repository.

## OTA Clarification

This repository distributes full Android releases. Android installs a newer
properly signed APK over the existing app without clearing user data.

Shorebird patches are distributed by Shorebird, not through this repository.
The first Shorebird-enabled app must still be published here as a new full APK.
Native code, plugins, assets, and Flutter engine changes also require a new
full APK release here.
