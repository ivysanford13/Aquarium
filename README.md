# Aquarium Log — self-hosted version

This is the GitHub Pages / Firebase version of your Aquarium Log app.
Each person who signs in gets their own private tanks and logs — nobody
can see anyone else's data, even though everyone loads the same page.

## 1. Create a Firebase project (free)

1. Go to https://console.firebase.google.com and click **Add project**.
   Name it anything (e.g. "aquarium-log"). You can skip Google Analytics.
2. In the left sidebar, go to **Build → Authentication → Get started**.
   Under **Sign-in method**, enable **Email/Password**.
3. In the left sidebar, go to **Build → Firestore Database → Create database**.
   Choose **production mode** and pick a location close to you.
4. Once created, go to **Rules** (a tab inside Firestore Database) and
   replace the contents with what's in `firestore.rules` in this folder,
   then click **Publish**. This is what actually keeps each person's data
   private — without it, anyone could read/write anyone's tanks.
5. Go to **Project settings** (gear icon, top left) → scroll to
   **Your apps** → click the **</>** (web) icon → register the app
   (any nickname is fine, no need for Firebase Hosting). It'll show you
   a `firebaseConfig` object with your keys.

## 2. Paste your config into the app

Open `index.html` and find this block near the top of the `<script type="module">`:

```js
const firebaseConfig = {
  apiKey: "PASTE_YOUR_API_KEY",
  authDomain: "PASTE_YOUR_PROJECT.firebaseapp.com",
  projectId: "PASTE_YOUR_PROJECT_ID",
  storageBucket: "PASTE_YOUR_PROJECT.appspot.com",
  messagingSenderId: "PASTE_YOUR_SENDER_ID",
  appId: "PASTE_YOUR_APP_ID",
};
```

Replace each value with what Firebase showed you in step 1.5. This
config is safe to be public (it's just which project to talk to) — the
Firestore rules are what actually enforce privacy, not this config.

## 3. Push to GitHub and turn on Pages

1. Create a new repo on GitHub (public or private both work with Pages
   on a paid plan; public repos get Pages free).
2. Upload `index.html` to the repo (rename this file from
   `aquarium-log-firebase.html` to `index.html` if it isn't already).
3. In the repo, go to **Settings → Pages**. Under **Source**, choose
   **Deploy from a branch**, pick `main` and `/ (root)`, then **Save**.
4. GitHub gives you a URL like `https://yourusername.github.io/reponame/`
   within a minute or two — that's your live, shareable app.

## How accounts work

- The first time someone visits and creates an account, they land on an
  empty log (no example tank — that was only for trying things out in
  the Claude version). They add their own tanks from there.
- Signing in on a different device pulls up the same account's data.
- Nobody sees anyone else's tanks unless you explicitly share your own
  login with them — there's no "shared household" mode in this version.
  If you want a couple of people to share one set of tanks, the simplest
  route is just sharing one login between them.
- "Forgot password?" on the sign-in screen sends a reset email via
  Firebase — no extra setup needed for that to work.

## Costs

Firebase's free tier ("Spark plan") comfortably covers personal use —
generous free reads/writes/storage per day, no credit card required
unless you specifically upgrade. GitHub Pages is always free for public
repos.
