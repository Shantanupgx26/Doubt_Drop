# ✦ DoubtDrop — Real-time Classroom Q&A Board

> Anonymous doubts. Live answers. Smarter classrooms.

DoubtDrop is a real-time classroom doubt board where students can anonymously submit questions during class, upvote doubts they share, and get live answers from the teacher — all without raising a hand.

---

## 🖼 Features

### For Students
- 🎒 Join any class with a **6-character code** — no account needed
- 💬 Submit doubts **anonymously** (only a nickname is shown)
- ▲ **Upvote** doubts you share with classmates
- ✅ See **teacher answers** appear in real-time
- 🏷 Each doubt is **auto-tagged** (concept / formula / example / doubt) using AI

### For Teachers
- 👩‍🏫 Secure **email + password login**
- 🏫 Create and manage **multiple classrooms**
- 📋 Live dashboard sorted by **most upvoted doubts**
- ✏️ **Reply to doubts** — answers appear instantly for students
- ✓ Mark doubts as **answered**
- 🚫 **Kick / block students** from a session
- 🗑 **Delete classrooms** along with all their data
- 📊 Session stats — total, pending, answered

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, Vanilla JS |
| Auth | Firebase Authentication (Email/Password) |
| Database | Firebase Realtime Database |
| AI Tagging | Claude API (claude-sonnet) |
| Hosting | Vercel |

---

## 🚀 Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/doubtdrop.git
cd doubtdrop
```

### 2. Set up Firebase

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Create a new project
3. Enable **Authentication → Email/Password**
4. Enable **Realtime Database** (start in test mode)
5. Go to **Project Settings → Your Apps → Add Web App**
6. Copy your `firebaseConfig` object

### 3. Paste your Firebase config

Open `doubtdrop.html` and find this section near the top:

```js
const firebaseConfig = {
  apiKey: "PASTE_YOUR_API_KEY",
  authDomain: "PASTE_YOUR_AUTH_DOMAIN",
  databaseURL: "PASTE_YOUR_DATABASE_URL",
  projectId: "PASTE_YOUR_PROJECT_ID",
  storageBucket: "PASTE_YOUR_STORAGE_BUCKET",
  messagingSenderId: "PASTE_YOUR_MESSAGING_SENDER_ID",
  appId: "PASTE_YOUR_APP_ID"
};
```

Replace the placeholder values with your actual Firebase config.

### 4. Open in browser

Just open `doubtdrop.html` directly in your browser — no build step needed.

---

## ☁️ Deploying to Vercel

1. Push this repo to GitHub
2. Go to [vercel.com](https://vercel.com) → **Add New Project**
3. Import your GitHub repo
4. Set Framework Preset to **Other**
5. Click **Deploy**

Your app will be live at `https://your-project.vercel.app` in ~30 seconds.

> ⚠️ **Important:** After deploying, go to Firebase Console → Authentication → Settings → **Authorized Domains** and add your Vercel URL (e.g. `doubtdrop.vercel.app`). Otherwise login will be blocked.

Every time you push to GitHub, Vercel automatically redeploys.

---

## 📁 Project Structure

```
doubtdrop/
├── index.html     # Entire app — single file, no build needed
└── README.md          # This file
```

---

## 🔑 How It Works

```
Teacher creates classroom → Gets a 6-char join code
         ↓
Students enter nickname + code → Join the live session
         ↓
Student submits doubt → AI tags it → Appears on teacher dashboard
         ↓
Classmates upvote → Doubt rises to the top
         ↓
Teacher replies → Answer appears instantly for all students
```

---

## 🔒 Firebase Database Rules (Recommended)

Once you're done testing, replace the default rules in Firebase with these for better security:

```json
{
  "rules": {
    "teachers": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    },
    "classrooms": {
      ".read": true,
      "$cid": {
        ".write": "auth != null"
      }
    },
    "doubts": {
      "$cid": {
        ".read": true,
        ".write": true
      }
    },
    "students": {
      "$cid": {
        ".read": true,
        ".write": true
      }
    }
  }
}
