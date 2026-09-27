# Bahagi — Salary & Budget Ledger (multi-user)

A salary and budget ledger with account sign-up, admin approval, and private
per-user data — backed by Firebase (Authentication + Firestore).

## How the split works

Offering and savings are each calculated directly from **net profit**
(salary − expenses):

1. **Net profit** = total salary − total expenses.
2. **Offering — 10%** of net profit.
3. **Savings — 20%** of net profit (independent of the offering).
4. **Final balance** = net profit − offering − savings.

If net profit is zero or negative, no offering or savings are taken.

## How accounts work

- Anyone can **sign up** with an email and password.
- New accounts start as **pending** and can't see any ledger data yet.
- The **admin** (an email address you set yourself) sees an extra **Admin**
  tab and can **approve**, **reject**, or later **revoke** any account.
- Once approved, a user's salary and expense entries are private to them —
  no one else can read or edit them, including other approved users.

## One-time setup (you only need to do this once)

### 1. Create a Firebase project
Go to [console.firebase.google.com](https://console.firebase.google.com),
click **Add project**, and follow the steps (you can skip Google Analytics).

### 2. Turn on Email/Password sign-in
In the Firebase console: **Build → Authentication → Get started →
Sign-in method → Email/Password → Enable → Save**.

### 3. Create a Firestore database
**Build → Firestore Database → Create database**. Any region is fine.
Start in **production mode** — you'll paste in the real rules next.

### 4. Publish the security rules
Open `firestore.rules` in this repo, replace `youremail@example.com` with
the email you'll actually log in with as admin, then in the Firebase
console go to **Firestore Database → Rules**, paste the contents in, and
click **Publish**.

### 5. Register a web app and get your config
**Project settings (gear icon) → General → Your apps → click the `</>`
icon → give it a nickname → Register app**. Firebase will show a
`firebaseConfig` object — copy it.

### 6. Paste your config into `index.html`
Open `index.html` and find this block near the top of the `<script>`
section:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  ...
};
const ADMIN_EMAILS = ["youremail@example.com"];
```

Replace it with your real config, and put your own email in `ADMIN_EMAILS`
(this must match exactly the email you'll sign up/log in with).

> This config is safe to commit and publish — it's a client identifier, not
> a secret. Access is protected by the Firestore rules, not by hiding this
> object.

### 7. Push to GitHub and enable Pages
Push this repo, then in **Settings → Pages** set the source to the `main`
branch and `/ (root)` folder. Firebase works fine on static hosting like
GitHub Pages since everything runs in the visitor's browser.

## Becoming the admin yourself

Sign up in the app once using the same email you put in `ADMIN_EMAILS`.
Your own account will also start as "pending" — go to the Firebase console,
**Firestore Database → Data → users → (your document)**, and manually
change its `status` field to `approved`. After that, you'll see the Admin
tab and can approve everyone else from inside the app itself.

## Tech

Plain HTML, CSS, and JavaScript, plus the Firebase Web SDK (loaded from
Google's CDN, no build step or `npm install` needed). Fonts are loaded from
Google Fonts (Fraunces, IBM Plex Mono, Karla).

## License

MIT — see [LICENSE](LICENSE).
