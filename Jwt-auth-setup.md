# 🔐 Firebase JWT Authentication Setup (React + Node.js)

This guide explains how to set up Firebase JWT authentication in a full-stack app with React (frontend) and Node.js (backend).

---

## ⚙️ FRONTEND – React + Firebase

### ✅ Step 1: Get Firebase JWT Token After Login

```js
import { getAuth } from "firebase/auth";
const auth = getAuth();

const token = await auth.currentUser.getIdToken(true);
```

---

### ✅ Step 2: Send JWT Token with Every Request (Axios Interceptor)

```js
import axios from "axios";
import { getAuth } from "firebase/auth";

const axiosSecure = axios.create();

axiosSecure.interceptors.request.use(async (config) => {
  const token = await getAuth().currentUser?.getIdToken();
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

---

## 🔧 BACKEND – Node.js + Firebase Admin

### ✅ Step 3: Install Firebase Admin SDK

```bash
npm install firebase-admin
```

---

### ✅ Step 4: Initialize Firebase Admin

```js
const admin = require("firebase-admin");

admin.initializeApp({
  credential: admin.credential.cert(require("./serviceAccountKey.json")),
});
```

> 🔐 You get `serviceAccountKey.json` from Firebase Console → Project Settings → Service Accounts.

---

### ✅ Step 5: JWT Verification Middleware

```js
const verifyJWT = async (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader?.startsWith("Bearer ")) {
    return res.status(401).json({ message: "Unauthorized" });
  }

  const token = authHeader.split(" ")[1];

  try {
    const decoded = await admin.auth().verifyIdToken(token);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(403).json({ message: "Forbidden" });
  }
};
```

---

### ✅ Step 6: Apply JWT Middleware to Protected Routes

```js
app.get("/payments/:email", verifyJWT, async (req, res) => {
  const userEmail = req.user.email;
  const paramEmail = req.params.email;

  if (userEmail !== paramEmail) {
    return res.status(403).json({ message: "Access denied" });
  }

  const payments = await paymentCollection.find({ email: userEmail }).toArray();
  res.send(payments);
});
```

---

## ✅ Summary

| Step | Task                                   |
| ---- | -------------------------------------- |
| 1    | Get token with `getIdToken()` on login |
| 2    | Send token via Axios interceptor       |
| 3    | Use Firebase Admin SDK in backend      |
| 4    | Create JWT verification middleware     |
| 5    | Use middleware on protected routes     |

---
