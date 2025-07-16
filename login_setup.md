# ✅ Firebase Authentication Setup in React + Node.js (Full Guide)

This checklist covers everything you did to set up Firebase Authentication in your full-stack React + Node.js project.

---

## 1️⃣ Create a Firebase Project

- Go to [Firebase Console](https://console.firebase.google.com/)
- Click **"Add project"**
- Enable Firebase Authentication
- Add your app (e.g., Web App)
- Copy the **Firebase config object**

---

## 2️⃣ Install Firebase in Frontend

```bash
npm install firebase
```

---

## 3️⃣ Create `firebase.config.js`

```js
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "xxx",
  authDomain: "xxx",
  projectId: "xxx",
  appId: "xxx",
  // other fields
};

export const app = initializeApp(firebaseConfig);
```

---

## 4️⃣ Create Auth Context (`AuthProvider`)

Wrap your app in this provider to share auth state.

```js
import { createContext, useEffect, useState } from "react";
import { getAuth, onAuthStateChanged } from "firebase/auth";
import { app } from "../firebase.config";

export const AuthContext = createContext();
const auth = getAuth(app);

const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (loggedInUser) => {
      setUser(loggedInUser);
      setLoading(false);
    });

    return () => unsubscribe();
  }, []);

  const authInfo = { user, loading };
  return (
    <AuthContext.Provider value={authInfo}>{children}</AuthContext.Provider>
  );
};

export default AuthProvider;
```

---

## 5️⃣ Create `useAuth` Hook

Simplifies access to the Auth context anywhere in your app.

```js
import { useContext } from "react";
import { AuthContext } from "../providers/AuthProvider";

const useAuth = () => useContext(AuthContext);
export default useAuth;
```

---

## 6️⃣ Implement Auth Functions

### Signup using email/password:

```js
createUserWithEmailAndPassword(auth, email, password);
```

### Login:

```js
signInWithEmailAndPassword(auth, email, password);
```

### Logout:

```js
signOut(auth);
```

### Social Login (e.g., Google):

```js
const provider = new GoogleAuthProvider();
signInWithPopup(auth, provider);
```

---

## 7️⃣ Wrap App in `AuthProvider`

In `main.jsx` or `App.jsx`:

```js
<AuthProvider>
  <App />
</AuthProvider>
```

---

## 8️⃣ Protect Routes (Optional)

Create a `PrivateRoute` component to protect authenticated routes.

---

## 9️⃣ Backend Integration (Optional)

- Send Firebase **JWT token** to your backend with each request.
- Verify the token using Firebase Admin SDK in your Node.js backend.

---

## 🗂 Summary Checklist

| Task                                 | Done ✅ |
| ------------------------------------ | ------- |
| Create Firebase Project              | ✅      |
| Install & Configure Firebase         | ✅      |
| Create Auth Context (`AuthProvider`) | ✅      |
| Create `useAuth` Hook                | ✅      |
| Implement Signup/Login/Logout        | ✅      |
| Add Social Login (Google etc.)       | ✅      |
| Wrap App with `AuthProvider`         | ✅      |
| Add Route Protection (if needed)     | ✅      |
| Backend JWT Verification (optional)  | ✅/🟡   |

---

Save this file as a reference whenever you implement Firebase authentication in new projects.
