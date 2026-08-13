# Not-Chatrooms
A basic HTML chat room (backend hosted on Firebase)
 Just a basic chatroom i made for a website i have
 The index.html does not work in its current condition and needs to be set up 

(This project was made with the assistance of Ai and i suggest you just make your own)
 # Instructions

## 1. Create a free Firebase backend

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a new project (you can turn off Google Analytics, it's not needed).
2. In the left sidebar, go to **Build > Realtime Database** and click **Create Database**. Choose any region, and start in **test mode** for now (see security note below).
3. Go to **Project settings** (gear icon) > **General**, scroll to "Your apps", and click the **</> (Web)** icon to register a new web app. You don't need Firebase Hosting — just registering the app.
4. Firebase will show you a `firebaseConfig` object. Copy it.

## 2. Drop your config into index.html

Open `index.html` and find this block near the bottom:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Replace it with the real values Firebase gave you. This isn't a secret key in the traditional sense — it's meant to be public in client-side code — but access is still controlled by your database's security rules (next step).

## 3. Set database rules

Test mode rules expire after 30 days and are wide open. For a basic public chat room, go to **Realtime Database > Rules** and use something like:

```json
{
  "rules": {
    "messages": {
      ".read": true,
      ".write": true,
      ".indexOn": ["ts"]
    },
    "presence": {
      ".read": true,
      ".write": true
    },
    "typing": {
      ".read": true,
      ".write": true
    }
  }
}
```

This keeps it simple (anyone can read/write messages, presence, and typing state, which is what a public chat room needs), but scopes access to just those paths rather than the whole database. Anyone with your config could technically spam the room — that's a real limitation of a backend-less, authless setup. If you want to lock it down further later, Firebase Authentication (e.g. anonymous sign-in) is the next step, but it's beyond "basic."

## 4. Push to GitHub and enable Pages

```bash
git init
git add index.html README.md
git commit -m "basic chat room"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Then on GitHub: **Settings > Pages > Build and deployment > Source: Deploy from a branch**, pick `main` and `/ (root)`, and save. Your chat room will be live at `https://YOUR_USERNAME.github.io/YOUR_REPO/` within a minute or two.
