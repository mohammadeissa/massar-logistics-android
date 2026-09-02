# Massar Logistics — Android direct download

Public distribution site for the **Massar Logistics** Android app
(`com.massarlogistics.app`), installed by drivers straight from a link
("sideload") instead of Google Play.

- Download page (GitHub Pages): https://mohammadeissa.github.io/massar-logistics-android/
- Stable APK link, always the newest release:
  https://github.com/mohammadeissa/massar-logistics-android/releases/latest/download/massar-logistics.apk
- `version.json` — read by the app on every launch/resume. A higher `versionCode`
  shows the in-app update banner; an installed build below `minVersionCode` gets a
  full-screen "update required" gate.

## Cutting a release

Everything is driven from the main Torok repository, never by hand here:

```bash
# in the Torok checkout, branch with the Capacitor app
npm run android:release:apk -- --bump 1.1 --notes "What changed" --notes-ar "اللي اتغير"
```

That builds the signed APK, uploads it as the release asset `massar-logistics.apk`
on a `v<versionName>` tag, and commits the new `version.json` here. Nothing but
`version.json` (and this page) should ever be committed to this repository — APKs
live on Releases so the git history stays small.

## Signing

Every APK is signed with the company keystore (`~/.massar-android/massar-upload.keystore`
on the build machine, backed up outside it). Android only installs an update over an
app signed with the same key, so a build signed with any other key cannot upgrade an
existing install. When the app later goes on Google Play, that same key must be
uploaded as the Play App Signing key so Play updates replace the direct installs in place.

`version.json` fields: `versionCode`, `versionName`, `apkUrl`, `apkUrlPinned`,
`minVersionCode` (optional), `notes` / `notesAr` (optional), `sha256`, `sizeBytes`,
`publishedAt`.
