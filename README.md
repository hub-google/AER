# Ancient Empires Reloaded — Google Play APK reference

Target: [Google Play package net.toyknight.aeii.android](https://play.google.com/store/apps/details?id=net.toyknight.aeii.android).

## Download status (2026-11-17)

**The current Google Play APK has NOT been downloaded or added to this repository.** The workflow alone is not a downloaded APK.

- Latest observed Google Play version: **5.2.1**, versionCode **5210**, updated 2026-11-15 ([Google Play data tracker](https://chrome-stats.com/d/net.toyknight.aeii.android)).
- Public APK mirrors checked only offered **4.2.6.3** (2025, versionCode 1063). This is **not** the requested latest Play version.
- [Ubuntu attempt](https://github.com/hub-google/AER/actions/runs/44049708953): failed because Aurora anonymous token dispenser returned HTTP **403** for all device profiles.
- [macOS attempt](https://github.com/hub-google/AER/actions/runs/44051401829): also failed with the same **403**.
- No binary hash or signing certificate has been verified, because **no APK file was returned**.

## Acquisition workflow

[Fetch Google Play APK](.github/workflows/fetch-google-play-apk.yml) attempts to download directly from Google Play using `goopdl`, checks the package ID and rejects older versions, and commits only if an APK was received and verification passed. No schedule is configured.

**Blocker:** Google's Play delivery needs a working authentication route. Aurora's public anonymous dispenser currently rejects the GitHub-hosted downloaders with HTTP 403. Re-running unchanged will not fix the problem. An authorized Google Play session or developer-provided APK for the *current version* is required.

Never commit Google login credentials, session tokens, or secret values to this repository.

## Licensing

The [historical AER source project](https://github.com/AncientTree/project_aeii) publishes GPL-3.0 code and says its bundled assets can be used; this does not automatically establish identical redistribution terms for newer Play builds. Check redistribution rights before publicly publishing an APK.

## Fallback: authorized Play session (when anonymous download returns HTTP 403)

The [workflow](.github/workflows/fetch-google-play-apk.yml) now supports a Google Play authorization session through **GitHub Actions secrets** without storing credentials in source code.

1. Use an **isolated throwaway Google account**, not your main account. The unofficial Google Play client may trigger Google account restrictions.
2. On your own trusted machine, use the [official goopdl authentication guide](https://github.com/Villoh/goopdl/blob/main/docs/authentication.md) and `goopdl auth login --print` to obtain an account-specific AAS token. Never send account credentials or tokens via a public message.
3. In [AER repository Actions secrets](https://github.com/hub-google/AER/settings/secrets/actions), create both `GOOPDL_ACCOUNT_EMAIL` and `GOOPDL_AAS_TOKEN`. The token must begin with `aas_et/`.
4. From [Actions](https://github.com/hub-google/AER/actions/workflows/fetch-google-play-apk.yml), choose **Run workflow**. If it downloads an APK with package `net.toyknight.aeii.android` and versionCode >= 5210, the workflow verifies the APK and adds binary, signature metadata, and SHA-256 to `google-play/`.

This is a contingency, **not a completed download**. Credentials cannot be supplied by the GitHub connector; do not share them in this chat. The downloader's account authentication is unofficial, can fail or trigger Google Play restrictions. There are no further periodic retry workflows configured.
