# Hulma 3D (Android app + web app)

Photos in, 3D model out. Object scanning from photos, place scanning with the phone's depth sensor, and AR viewing.

The Android app is a **Trusted Web Activity**: an installable app with its own icon that runs the Hulma page inside Chrome. That's what lets web AR and the depth scanner work. A plain WebView app can't do AR.

## What's in here

| Folder / file | What it is |
|---|---|
| `web/` | The app itself (`index.html`), plus icons, `manifest.webmanifest` and `sw.js` for offline use |
| `app/` | The Android shell. `hulma-signing.keystore` is the fixed key, so new builds install over old ones |
| `.github/workflows/build.yml` | Builds the APK, publishes it as a release, and puts `web/` on GitHub Pages |
| `workflow-copy.yml.txt` | The same workflow, in case Windows drops the `.github` folder when you upload |
| `github-io-site/` | Two files for a *second* repo, only needed for full-screen mode (step 5) |

## One-time setup

1. Create a **public** repo called `hulma-3d` on your `gnotesamsung7-hue` account.
2. Unzip, open the `hulma-3d` folder, and drag **everything inside it** into **Add file → Upload files**. Commit.
3. Open the **Actions** tab. If it says **Get started** instead of showing a run, Windows dropped the `.github` folder:
   click **set up a workflow yourself**, name the file `build.yml`, paste in everything from `workflow-copy.yml.txt`, and commit.
4. Go to **Settings → Pages** and set **Source** to **GitHub Actions**. Then in **Actions**, re-run the workflow (or push any change).
   A run takes about 5 minutes. When it's green you have:
   - Web app: https://gnotesamsung7-hue.github.io/hulma-3d/
   - Android app: https://github.com/gnotesamsung7-hue/hulma-3d/releases/latest/download/hulma-3d.apk

   The QR codes in the app (bottom section, **Get the app**) point to those two links. The "latest" link always gives the newest build, so printed QR codes keep working.

5. **Optional: full-screen mode.** Without this, the app shows a slim Chrome address bar at the top. Everything still works.
   To remove it, Android needs proof that you own both the app and the website:
   1. Create a repo named exactly `gnotesamsung7-hue.github.io` (skip if you already have it).
   2. In that repo click **Add file → Create new file**, type `.nojekyll` as the name, leave it empty, commit.
   3. **Add file → Create new file** again, type `.well-known/assetlinks.json` as the name (the slash makes the folder),
      paste in the contents of `github-io-site/.well-known/assetlinks.json`, commit.
   4. In that repo, **Settings → Pages**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`.
   5. Check that https://gnotesamsung7-hue.github.io/.well-known/assetlinks.json opens and shows the text, then reopen the app.

## Installing the APK

1. Scan the Android QR code and open the link. `hulma-3d.apk` downloads.
2. Open it. Allow **Install unknown apps** for your browser if Android asks.
3. If Play Protect warns, tap **More details → Install anyway**.
4. Keep **Chrome** installed and updated. AR needs **Google Play Services for AR**.
5. The first launch needs internet. After that it opens offline.

## Updating

Change files in `web/` (usually just `index.html`) and commit. The workflow rebuilds the APK and the website.
Installed apps pick up a new `index.html` automatically, on the launch after next. You only need a new APK for icon or name changes.

## If the build fails

Open the failed run in **Actions**, click the red step, search the log for `What went wrong`, and send a screenshot of those lines.
