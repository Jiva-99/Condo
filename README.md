# Condo shortlist

A single-page app for recording and comparing condos in Thailand: projects (buildings), units, photo links and a side-by-side comparison table.

- `index.html`: the whole app
- `firestore.rules`: security rules to paste into Firebase

Without Firebase set up, the app still works and saves in the browser only. With Firebase, you sign in with Google and your data syncs across devices, including offline use.

## 1. Put it on GitHub Pages

1. Create a repository (for example `condo-shortlist`) and upload `index.html`, `firestore.rules` and this README.
2. In the repo, go to **Settings → Pages**, set **Source** to "Deploy from a branch", pick `main` and `/ (root)`, and save.
3. After a minute the app is live at `https://<your-username>.github.io/condo-shortlist/`.

## 2. Create the Firebase project (free Spark plan)

1. Go to <https://console.firebase.google.com> and click **Create a project**. Google Analytics isn't needed.
2. In **Project overview**, click the **Web** icon (`</>`) to add a web app. Firebase Hosting isn't needed.
3. Copy the `firebaseConfig` values it shows and paste them into `FIREBASE_CONFIG` near the top of the `<script>` in `index.html`. These values are not secrets and are safe in a public repo; the security rules protect your data.

## 3. Turn on Google sign-in

1. **Build → Authentication → Get started**.
2. **Sign-in method → Google → Enable**, choose a support email, and save.
3. **Settings → Authorized domains → Add domain**, and add `<your-username>.github.io`.
   `localhost` is already there for local testing.

## 4. Create the database and set the rules

1. **Build → Firestore Database → Create database**.
2. Pick a location close to you, for example `asia-southeast1` (Singapore) or `australia-southeast1` (Sydney). This can't be changed later.
3. Start in **production mode**.
4. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.

This step is essential. The rules make sure each signed-in account can only see its own data. To stop anyone else from even creating data in your project, use the optional email lock inside `firestore.rules`.

## 5. Use it

Open the GitHub Pages URL and click **Sign in with Google**. The status next to it shows:

- **Synced**: everything is saved to Firebase.
- **Saving…**: changes are on their way.
- **Offline, changes kept on this device**: they upload automatically when you're back online.
- **Sync problem**: hover it for details. The usual cause is missing rules or an unauthorized domain.

If you used the app before signing in, the condos saved in that browser are uploaded to your account the first time you sign in (only if your account has no data yet).

When signed out, the app shows and saves browser-only data, which is separate from your account data.

## Backups

Use **Export JSON** at the bottom of the page to download everything. **Import JSON** adds records from an export; records with the same ID are updated, and nothing is deleted.

## Testing locally

Google sign-in doesn't work from a file opened directly (`file://`). Serve the folder instead:

```
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Data layout in Firestore

```
users/{your uid}/projects/{projectId}
users/{your uid}/units/{unitId}     (each unit has projectId)
```

## Updating the Firebase SDK

The SDK version is set in `FIREBASE_VERSION` in `index.html` (currently 12.15.0) and loaded from Google's CDN. Change the number to update.
