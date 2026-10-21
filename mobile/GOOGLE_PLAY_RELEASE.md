# Google Play release

## Production identity

- App name: `FOUR³`
- Application ID: `com.kfbs.fourinrow`
- Minimum Android version: API 24 (Android 7.0)
- Compile/target SDK: API 36 (Android 16)
- Current version: `1.0.0+1`

The application ID is permanent after the first Play Console upload. Increase
the build number after every uploaded artifact, for example `1.0.0+2` in
`pubspec.yaml`.

## Upload signing key

The local release key is stored in files ignored by Git:

- `android/upload-keystore.jks`
- `android/key.properties`

Back up both files in an encrypted password manager or another secure location.
The keystore alias is `upload`.

Upload certificate fingerprints:

- SHA-1: `70:BC:30:E3:A6:CF:CB:BC:0E:2B:B7:1E:97:49:A1:D8:E4:D0:FE:9A`
- SHA-256: `DF:31:BE:91:C9:4B:11:2A:3B:C6:15:BE:63:D2:70:F3:D0:1D:E3:DB:20:BE:6A:8B:6B:0F:59:D3:37:40:1C:4B`

With Play App Signing enabled, Google uses a separate app-signing certificate
for installs. Use the app-signing SHA-256 shown by Play Console, not the upload
fingerprint above, in `https://4inrow.ru/.well-known/assetlinks.json`.

## Build

```sh
fvm flutter analyze
fvm flutter test
fvm flutter build appbundle --release
```

Upload `build/app/outputs/bundle/release/app-release.aab` to the internal test
track first.

## Play Console checklist

- Create the app as `FOUR³`, select the correct default language and reserve
  `com.kfbs.fourinrow` by uploading the AAB.
- Enable Play App Signing and keep the generated key as the upload key.
- Complete App access and provide a working review account because some online
  functionality requires authentication.
- Complete the Data safety form from actual production behavior. The app can
  process a username, email address, authentication data, game history,
  progress and rating. Re-check server logging and retention before submitting.
- Publish public HTTPS pages for the privacy policy, support contact and account
  deletion. Because the app supports registration, it also needs an in-app
  account-deletion/request path. These are release blockers until implemented.
- Complete content rating, target audience, ads and any applicable government
  app declarations using the product owner's answers.
- Add the store icon, feature graphic, phone screenshots, descriptions and
  release notes. Draft Russian copy is in `store/google-play/ru-RU.md`.
- After Play generates the app-signing certificate, publish/update
  `assetlinks.json` and test the `/auth/verify` App Link on a physical device.
- Roll out to internal testing, exercise registration, email verification,
  login/logout, AI/local/online games, reconnect, audio and background resume,
  then promote gradually.
