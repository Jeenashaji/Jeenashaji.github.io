# Ripple - Backend Setup Guide

## Overview
This guide will help you add user authentication and cloud data storage to Ripple using Firebase (free tier).

---

## Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a project"**
3. Name it "ripple-app" (or your choice)
4. Disable Google Analytics (optional, keeps it simpler)
5. Click **Create Project**

---

## Step 2: Enable Authentication

1. In Firebase Console, click **Authentication** (left sidebar)
2. Click **Get Started**
3. Go to **Sign-in method** tab
4. Enable **Email/Password**
5. (Optional) Enable **Google** sign-in for easier login

---

## Step 3: Create Firestore Database

1. Click **Firestore Database** (left sidebar)
2. Click **Create database**
3. Choose **Start in test mode** (we'll secure it later)
4. Select a location closest to your users
5. Click **Enable**

---

## Step 4: Get Your Firebase Config

1. Click the **gear icon** → **Project settings**
2. Scroll down to **Your apps**
3. Click the **</>** (Web) icon
4. Register app with nickname "ripple-web"
5. Copy the `firebaseConfig` object - you'll need this!

It looks like this:
```javascript
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "ripple-app.firebaseapp.com",
  projectId: "ripple-app",
  storageBucket: "ripple-app.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

## Step 5: Update Your App

1. Open `app.html`
2. Find the `firebaseConfig` object near the top of the `<script>` section
3. Replace it with YOUR config from Step 4

---

## Step 6: Set Security Rules

Go to **Firestore Database** → **Rules** tab and paste:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**.

---

## Step 7: Deploy Your Site

### Option A: Firebase Hosting (Recommended)

1. Install Firebase CLI:
   ```bash
   npm install -g firebase-tools
   ```

2. Login:
   ```bash
   firebase login
   ```

3. Initialize:
   ```bash
   firebase init hosting
   ```
   - Select your project
   - Public directory: `.` (or where your files are)
   - Single-page app: No
   - Don't overwrite index.html

4. Deploy:
   ```bash
   firebase deploy
   ```

Your site will be live at: `https://your-project.web.app`

### Option B: Netlify (Drag & Drop)

1. Go to [netlify.com](https://netlify.com)
2. Drag your folder containing `index.html` and `app.html`
3. Done! You get a free URL

### Option C: Vercel

1. Go to [vercel.com](https://vercel.com)
2. Connect to GitHub or drag files
3. Deploy

---

## How It Works

### User Registration
- User signs up with email/password
- Firebase creates a unique user ID
- All their data is stored under `/users/{userId}`

### Data Storage
- Tasks, pages, meetings all saved to Firestore
- Real-time sync across devices
- Data persists even if browser cleared

### Security
- Each user can ONLY access their own data
- Firebase handles authentication securely
- Passwords are never stored in plain text

---

## Pricing (Firebase Free Tier)

| Feature | Free Limit |
|---------|------------|
| Authentication | 50,000 users/month |
| Firestore Reads | 50,000/day |
| Firestore Writes | 20,000/day |
| Storage | 1 GB |
| Hosting | 10 GB/month |

**This is MORE than enough for hundreds of active users!**

---

## Troubleshooting

### "Permission Denied" Error
- Check Firestore rules are published
- Make sure user is logged in
- Verify config is correct

### Login Not Working
- Enable Email/Password in Authentication settings
- Check browser console for errors

### Data Not Saving
- Check Firestore rules
- Look at browser console for errors
- Verify internet connection

---

## Need Help?

- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase YouTube Channel](https://www.youtube.com/firebase)
- [Stack Overflow - Firebase Tag](https://stackoverflow.com/questions/tagged/firebase)

---

## Next Steps After Launch

1. **Custom Domain**: Add your own domain in Firebase Hosting settings
2. **Password Reset**: Already built into the app!
3. **Analytics**: Enable Firebase Analytics to track usage
4. **Backups**: Set up Firestore backups in Google Cloud Console

Good luck with Ripple! 🌊
