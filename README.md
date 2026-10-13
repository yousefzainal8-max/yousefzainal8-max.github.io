# APMT PORT - GC DASHBOARD
Live shift cargo dashboard. Anyone with the link can add/update data and everyone sees it within seconds.
لوحة متابعة الشحن المباشرة: أي شخص يملك الرابط يستطيع التحديث ويظهر عند الجميع خلال ثوانٍ.

## Files / الملفات
| File | Purpose |
|---|---|
| `index.html` | The whole website (put in repository root) |
| `firestore.rules` | Database rules to paste into Firebase |
| `.nojekyll` | Optional (GitHub's web upload may refuse dot-files; the site works without it) |
| `README.md` | These instructions |

## Step 1 – Firebase database (free) / قاعدة البيانات
1. https://console.firebase.google.com -> **Add project** (any name, Analytics off).
2. **Build -> Firestore Database -> Create database** -> start in **production mode** -> choose a region -> Enable.
3. **Firestore -> Rules** -> delete everything, paste the content of `firestore.rules` -> **Publish**.
4. **Project settings (gear icon) -> General -> Your apps -> Web `</>`** -> give any nickname -> Register app.
5. Copy the `firebaseConfig` values shown.

## Step 2 – Paste the config / لصق الإعدادات
**Easy way (no code editing) / الطريقة السهلة:** open the site, click **🔧 Database setup** in the header, paste the `firebaseConfig` block, press **Save & test**, then **Download configured index.html** and upload that file to GitHub (replace the old `index.html`). Now every device is connected.
اضغط 🔧 Database setup في أعلى الموقع، الصق إعدادات Firebase، ثم نزّل index.html المعدّل وارفعه إلى GitHub بدل القديم.

**Manual way / الطريقة اليدوية:**
Open `index.html`, find `window.FIREBASE_CONFIG` near the top and fill:
```js
window.FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef"
};
```
(If `apiKey` stays empty the site works in offline mode: each device keeps its own data.)

## Step 3 – Publish on GitHub Pages / النشر
1. github.com -> **New repository** (e.g. `gc-dashboard`, Public).
2. **Add file -> Upload files**: upload `index.html`, `.nojekyll`, `README.md`, `firestore.rules` to the **root** -> Commit.
3. **Settings -> Pages -> Source: Deploy from a branch -> Branch: `main`, folder `/ (root)` -> Save**.
4. After ~1 minute: `https://<your-username>.github.io/gc-dashboard/` is live. Share this link.

## Step 4 – Check it works / التأكد
Open the link on two devices. The header must show **✅ connected – live for everyone**.
Add a shift entry on one device -> it appears on the other in a few seconds.
A red message at the top tells you what is wrong (config missing, rules, database not created, scripts blocked). Hard-refresh with Ctrl+F5 after fixing.

## Using it
- **Dashboard** (first page), **Shift Entry** (second), **Reports Archive** (last).
- **TV Mode**: press the TV button or the `T` key. **📡 Remote** changes Period / Page / TV Mode for all open screens.
- Edit, Delete, Remote and Generate 24H Report ask for password `GC123`.

## Change the password / تغيير كلمة المرور
Run `printf 'NEWPASS' | sha256sum` (or use any SHA-256 tool) and replace the value of `PWH` in `index.html`.
The password is checked in the browser, so it is only a light safeguard.

## Security note
`firestore.rules` lets anyone with the link read and write the shared data (this is what makes "anyone can update" work).
For stricter control, enable Firebase Authentication and change the rules to `allow read, write: if request.auth != null;`.

## Realtime Database version / نسخة Realtime Database
This package is already set to the **Realtime Database** URL:
`https://apmt-port---gc-dashboard-default-rtdb.asia-southeast1.firebasedatabase.app`
1. Firebase Console -> **Build -> Realtime Database -> Rules** -> paste `database.rules.json` -> **Publish**.
   (If you started in "test mode", the default rules expire after 30 days, so publish these rules.)
2. Upload `index.html` to GitHub and open the site: the header must show **✅ connected – live for everyone**.
You do NOT need Firestore for this version. If you prefer Firestore, use `firestore.rules` and fill `apiKey`/`projectId` instead.

## Connection test / اختبار الاتصال
Open `test-connection.html` (upload it to GitHub next to index.html, then open `https://<you>.github.io/<repo>/test-connection.html`) and press **Run connection test**. It checks: library loads -> write -> read, and tells you exactly what to fix. When it says **ONLINE MODE WORKS**, the dashboard is online.
