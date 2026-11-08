# Hyrox Crew Schedule – setup

## 1. Firebase (shared database)
1. Go to https://console.firebase.google.com → **Add project**.
2. **Build → Firestore Database → Create database** (production mode).
3. **Rules** tab, paste, then **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /races/{id} {
         allow read: if true;
         allow create: if request.resource.data.keys().hasOnly(['name','race','date','time'])
                       && request.resource.data.name is string && request.resource.data.name.size() < 41
                       && request.resource.data.race is string && request.resource.data.race.size() < 81;
         allow delete: if true;
       }
     }
   }
   ```
   Anyone with the link can add/delete entries – fine for friends, not for the public.
4. **Project settings → Your apps → Web (`</>`)** → register app, copy the `firebaseConfig`
   values into `index.html`.

## 2. Host on Firebase Hosting
The site is a static single-page app served directly from the repository root; no build step is needed.

1. Install Node.js, then install and log in to the Firebase CLI:
   ```sh
   npm install -g firebase-tools
   firebase login
   ```
2. From the repository root, select the existing Firebase project. This creates a local `.firebaserc`:
   ```sh
   firebase use --add
   ```
   Choose the project associated with this app (its project ID may differ from its display name).
3. Deploy the site:
   ```sh
   firebase deploy --only hosting
   ```

The repository's `firebase.json` configures Hosting to serve `index.html` from the root. Firebase will provide the site URLs after deployment, typically `PROJECT_ID.web.app` and `PROJECT_ID.firebaseapp.com`.
