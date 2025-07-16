# 🍽️ Bistro Boss Server – RESTful Backend API (Express + MongoDB)

🚀 **Live Demo:** [https://bistro-boss-server-with-auth-gold.vercel.app/](https://bistro-boss-server-with-auth-gold.vercel.app/)

This is the **backend server** for the [Bistro Boss Client](https://github.com/iamharryda/bistro-boss-client) restaurant management app. Built with **Node.js**, **Express.js**, and **MongoDB**, this RESTful API handles authentication, authorization, user roles, menu management, cart operations, and admin functions.

---

## 🚀 Features

- 🔐 JWT Authentication
- 👥 Role-Based Access Control (Admin/User)
- 🧾 Secure API Routes
- 🍽️ Menu CRUD (Create, Read, Update, Delete)
- 🛒 Cart and Order Management
- 📊 Admin Statistics
- 🌐 CORS + Environment Config
- 💳 Payment integration with stripe

---

## 🛠️ Tech Stack

| Tool/Library   | Purpose                        |
| -------------- | ------------------------------ |
| Express.js     | Web framework                  |
| MongoDB        | Database                       |
| Mongoose       | ODM for MongoDB                |
| Firebase Admin | Verify Firebase tokens         |
| JWT            | Authentication                 |
| dotenv         | Manage environment variables   |
| cors           | Cross-Origin requests          |
| nodemon        | Auto-reload during development |
| stripe         | payment gateway                |

---

## 📦 Getting Started

> 💡 Prerequisites: Node.js, npm, MongoDB URI, Firebase Admin SDK credentials

### 1. Clone the repository

Clone the project using Git:

```bash
git clone https://github.com/iamharryda/Bistro-boss-server.git
cd Bistro-boss-server
```

### 2. install dependencies

Install the required packages:

```bash
npm intall
```

### 3. Configure environment variables

Create a .env file in the root directory and add your environment-specific variables:

```bash
DB_URI = your mongodb uri
ACCESS_TOKEN_SECRET=your jwt token
STRIPE_SECRET_KEY= your stripe secret key
```

⚠️ Do not commit your .env file to source control. Keep it private.

### 4. Start the server

```bash
nodemon index.js
```

Or start in production mode:

```bash
npm run start
```

the surver will run on

```bash
http://localhost:5000

```

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙋‍♂️ Author

Built with ❤️ by [@iamharryda](https://github.com/iamharryda)
