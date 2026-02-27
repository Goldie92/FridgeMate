# FridgeMate — Setup Guide

A PWA for tracking what's in your fridge and freezer, with real-time cross-device sync via Firebase.

## Features
- **Fridge / Freezer** storage location toggle
- **Expiry warnings** — colour-coded (green / amber / red / expired)
- **Sort** by expiry, name, date added, or category
- **Quick edit & delete** on every item
- **Search** and **category filters**
- **Google Sign-In** with real-time Firestore sync
- **Offline-first** — works without internet, syncs when back online
- **Installable** as a home screen app on Android (PWA)

---

## Quick Start (offline only, no sync)

Just open `index.html` in a browser. Done. Data is stored locally in IndexedDB.

---

## Enable Cross-Device Sync (Firebase)

This takes about 5 minutes. You need a Google account.

### Step 1 — Create a Firebase project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **Add project**
3. Name it anything (e.g. "FridgeMate")
4. Disable Google Analytics (not needed) → **Create project**

### Step 2 — Add a web app

1. In your project, click the **</>** (web) icon to add an app
2. Name it "FridgeMate"
3. **Don't** tick "Firebase Hosting" (unless you want to host there)
4. Click **Register app**
5. You'll see a config block like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "fridgemate-xxxxx.firebaseapp.com",
  projectId: "fridgemate-xxxxx",
  storageBucket: "fridgemate-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

6. Copy these values into the `FIREBASE_CONFIG` object at the top of `index.html`

### Step 3 — Enable Google Sign-In

1. In Firebase Console → **Authentication** → **Get started**
2. Click **Google** under "Additional providers"
3. Toggle **Enable**
4. Select a support email → **Save**

### Step 4 — Create the Firestore database

1. In Firebase Console → **Firestore Database** → **Create database**
2. Choose **Start in production mode**
3. Select a region close to you (e.g. `europe-west2` for UK)
4. Click **Create**

### Step 5 — Set Firestore security rules

Go to **Firestore** → **Rules** tab and replace with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Each user can only read/write their own items
    match /users/{userId}/items/{itemId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**.

### Step 6 — Host it

You need to serve the files over HTTPS for the PWA and auth to work. Options:

**GitHub Pages (free, easiest):**
1. Create a new GitHub repo
2. Push all files to the `main` branch
3. Go to repo Settings → Pages → Source: `main` / root → Save
4. Your app is live at `https://yourusername.github.io/repo-name/`

**Firebase Hosting (free):**
1. Install Firebase CLI: `npm install -g firebase-tools`
2. Run `firebase init hosting` → select your project → set public dir to `.`
3. Run `firebase deploy`

**Netlify / Vercel (free):**
Just drag and drop the folder.

### Step 7 — Add authorized domain

1. Firebase Console → **Authentication** → **Settings** → **Authorized domains**
2. Add your hosting domain (e.g. `yourusername.github.io`)

---

## That's it!

Open the hosted URL on any device, sign in with Google, and your fridge inventory syncs everywhere in real time. The app also works offline and syncs changes when you reconnect.

---

## Data structure

Each item stored in Firestore:

```json
{
  "name": "Chicken breast",
  "quantity": 2,
  "useBy": "2026-03-05",
  "location": "fridge",
  "category": "meat",
  "addedAt": 1740500000000
}
```

Stored at: `users/{uid}/items/{itemId}`

---

## File overview

```
fridgemate/
├── index.html      ← The entire app (single file)
├── manifest.json   ← PWA manifest for installability
├── sw.js           ← Service worker for offline support
├── icon-192.png    ← App icon (192×192)
├── icon-512.png    ← App icon (512×512)
└── README.md       ← This file
```
