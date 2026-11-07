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

## 2. Host on GitHub Pages
1. Create a GitHub repo and push `index.html`.
2. Repo **Settings → Pages → Deploy from branch → main / root**.
3. Share the `https://<user>.github.io/<repo>/` link.
