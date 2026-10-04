# JMA Motor Service Finance

A mobile-first finance dashboard inspired by the supplied JMA reference: navy background, green Income +, red Expenses −, current balance, Day/Week/Month summaries and Home/History/Settings. The car emblem is a lightweight vector recreation, not the original photographic logo.

## Publish on GitHub Pages

1. Extract the ZIP. Upload the **contents** of the `jma-finance` folder to a GitHub repository, keeping `icons/` intact. `index.html` must be at the publishing root.
2. In the repository, open **Settings → Pages**. Choose **Deploy from a branch**, select your branch (usually `main`) and **/(root)**, then Save.
3. Open the HTTPS Pages address shown by GitHub after deployment. Project subpaths are supported; there are no absolute asset URLs or build steps.
4. Create and confirm a 4 digit PIN. No default PIN is supplied.

## Use

- Tap Income + or Expenses −, enter a euro amount, description and date, then Save. Amounts are stored as integer cents. Dates cannot be in the future.
- Top cards and current balance show all saved transactions. Day is today; Week starts Monday; Month starts on the first. Period summaries run through today in the device’s local timezone.
- Use History to filter and delete transactions. Deletion asks for confirmation.
- The eye button hides displayed amounts. The top-left button locks the app; the top-right button opens Settings.
- Change the PIN in Settings by entering the current PIN and confirming a new one.
- Export a JSON backup from Settings and restore it on another installation if needed. Restore replaces records after confirmation. Backups do not contain the PIN.

## Install and use offline

On iPhone/iPad, open in Safari and select **Share → Add to Home Screen**. On supported Android and desktop browsers, use **Install app / Add to Home Screen**, or the Install button in Settings when offered. Installation availability depends on the browser. Visit online once and allow the service worker to finish before going offline. The manifest includes 192px, 512px and maskable PNG icons.

For a local preview, serve this folder on localhost (for example `npx --yes http-server .`). Open the printed localhost address. Do not open `index.html` directly as a file: secure-context PIN cryptography and service workers require HTTPS or localhost.

## Storage and PIN limitations

All records live in localStorage for the current browser and origin; there is no account, server or cross-device sync. Clearing site data, using private browsing or changing browser/origin may remove or separate records. Installed app storage may differ from browser storage on some platforms. Export backups regularly.

The PIN is stored as a salted PBKDF2-SHA-256 hash (150,000 iterations). The app locks when the page becomes hidden and on every reload. Five failed unlock attempts cause a 30-second in-session pause. This is a convenience screen lock, **not encryption or strong access control**: someone with device/browser developer access can read the records or bypass the lock. Backups are plaintext. Use device security for sensitive financial data.

There is no PIN recovery service. If the PIN is forgotten, resetting site storage removes both the PIN and records; a previously exported backup can then be restored after creating a new PIN. Do not clear storage without considering data loss.

## Files and updates

`index.html`, `style.css`, `app.js`, `manifest.json`, `service-worker.js`, and `icons/` are the complete app. No libraries, analytics, APIs or remote fonts are used. Deploy updated files together and increment the service worker cache name when releasing changes. Close all app tabs and reopen online to activate a waiting worker.

Use a separate Pages origin for unrelated apps when practical: localStorage is shared by origin rather than repository path. This project uses JMA-specific storage keys.

## Clean-screen update
The app has no simulated battery, clock, status bar, screenshot background or phone frame. The installed PWA requests fullscreen where supported; the operating system may still show its own status bar, particularly on iOS. New and changed PINs must contain exactly four digits. Older 4–8 digit PINs remain accepted only to allow existing users to sign in and change their PIN without losing records.
