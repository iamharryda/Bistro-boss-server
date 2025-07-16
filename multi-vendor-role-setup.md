# 🛍️ Multi-Vendor Role Setup (Admin, Seller, User)

This guide explains how to implement a role-based system in a full-stack app using Firebase Auth, MongoDB, and Express.

---

## 🎭 Roles You Support

- `admin`: Full access to manage users, products, system.
- `seller`: Can add/update/delete their products.
- `user`: Can browse, purchase, and interact.

---

## ✅ Step 1: Save User and Default Role After Signup/Login

```js
const userData = {
  name: user.displayName,
  email: user.email,
  role: "user", // default
};

await axios.post("/users", userData);
```

---

## ✅ Step 2: Backend Route to Store Users

```js
app.post("/users", async (req, res) => {
  const user = req.body;
  const existingUser = await usersCollection.findOne({ email: user.email });

  if (existingUser) {
    return res.send({ message: "User already exists" });
  }

  user.role = user.role || "user";
  const result = await usersCollection.insertOne(user);
  res.send(result);
});
```

---

## ✅ Step 3: Create Role-Based Middleware

```js
const verifyAdmin = async (req, res, next) => {
  const user = await usersCollection.findOne({ email: req.user.email });
  if (user?.role !== "admin") {
    return res.status(403).send({ message: "Admins only" });
  }
  next();
};

const verifySeller = async (req, res, next) => {
  const user = await usersCollection.findOne({ email: req.user.email });
  if (user?.role !== "seller") {
    return res.status(403).send({ message: "Sellers only" });
  }
  next();
};
```

---

## ✅ Step 4: Use Middleware on Protected Routes

```js
app.get("/admin/stats", verifyJWT, verifyAdmin, handler);
app.post("/products", verifyJWT, verifySeller, handler);
```

---

## ✅ Step 5: Allow Admin to Change Roles

```js
app.put("/users/role/:id", verifyJWT, verifyAdmin, async (req, res) => {
  const id = req.params.id;
  const role = req.body.role;

  const result = await usersCollection.updateOne(
    { _id: new ObjectId(id) },
    { $set: { role } }
  );

  res.send(result);
});
```

---

## ✅ Step 6: Frontend – Fetch Role & Conditionally Render UI

```js
const { data: userRole } = useQuery(["role", user.email], async () => {
  const res = await axiosSecure.get(`/users/role/${user.email}`);
  return res.data.role;
});

{
  userRole === "admin" && <AdminDashboard />;
}
{
  userRole === "seller" && <SellerDashboard />;
}
```

---

## ✅ Step 7: Endpoint to Return User Role

```js
app.get("/users/role/:email", verifyJWT, async (req, res) => {
  const email = req.params.email;
  const user = await usersCollection.findOne({ email });
  res.send({ role: user?.role || "user" });
});
```

---

## ✅ MongoDB Example Document

```json
{
  "_id": "12345",
  "name": "John Doe",
  "email": "john@example.com",
  "role": "seller"
}
```

---

## ✅ Summary

| Step | Task                                             |
| ---- | ------------------------------------------------ |
| 1    | Save user + default role to DB                   |
| 2    | Middleware to verify `admin` and `seller`        |
| 3    | Apply middleware to role-based routes            |
| 4    | Admin can change user roles                      |
| 5    | Frontend fetches role and shows UI conditionally |

---
