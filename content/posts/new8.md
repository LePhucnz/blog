---
title: "Xác thực người dùng bằng JWT trong Node.js"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Hướng dẫn cách xác thực người dùng bằng JSON Web Token (JWT) trong ứng dụng Node.js sử dụng Express."
tags: ["Node.js", "JWT", "Xác thực", "Bảo mật", "Express"]
categories: ["JavaScript"]
---

## 🔐 JWT là gì?

**JWT (JSON Web Token)** là một chuẩn mở dùng để truyền thông tin giữa các bên một cách an toàn dưới dạng JSON.  
Nó thường được dùng để xác thực người dùng trong các ứng dụng web hiện đại.

Một JWT gồm 3 phần:
- **Header**: thông tin thuật toán và loại token  
- **Payload**: dữ liệu (ví dụ: userId, role)  
- **Signature**: chữ ký số để xác minh tính hợp lệ  

---

## 📁 Cấu trúc dự án

```
jwt-auth-nodejs/
├── models/
│   └── User.js
├── middleware/
│   └── auth.js
├── routes/
│   ├── auth.js
│   └── stocks.js
├── .env
├── package.json
└── server.js
```

---

## 🧩 Cài đặt và cấu hình

### 📝 Step 1: Cài đặt thư viện JWT
```bash
npm install express mongoose dotenv jsonwebtoken bcryptjs
```

---

### 📝 Step 2: Tạo model người dùng  
**📁 models/User.js**
```js
const mongoose = require("mongoose");

const UserSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true
  }
});

module.exports = mongoose.model("User", UserSchema);
```

---

### 📝 Step 3: Tạo route đăng ký và đăng nhập  
**📁 routes/auth.js**
```js
const express = require("express");
const router = express.Router();
const User = require("../models/User");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");

router.post("/register", async (req, res) => {
  const { username, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const newUser = new User({ username, password: hashedPassword });
  await newUser.save();
  res.json({ message: "Đăng ký thành công" });
});

router.post("/login", async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user) return res.status(400).json({ message: "Sai tài khoản" });

  const isMatch = await bcrypt.compare(password, user.password);
  if (!isMatch) return res.status(400).json({ message: "Sai mật khẩu" });

  const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, { expiresIn: "1h" });
  res.json({ token });
});

module.exports = router;
```

---

### 📝 Step 4: Tạo middleware xác thực  
**📁 middleware/auth.js**
```js
const jwt = require("jsonwebtoken");

const auth = (req, res, next) => {
  const token = req.header("Authorization");
  if (!token) return res.status(401).json({ message: "Không có token" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: "Token không hợp lệ" });
  }
};

module.exports = auth;
```

---

### 📝 Step 5: Bảo vệ route bằng middleware  
**📁 routes/stocks.js**
```js
const express = require("express");
const router = express.Router();
const Stock = require("../models/Stock");
const auth = require("../middleware/auth");

router.get("/", auth, async (req, res) => {
  const stocks = await Stock.find();
  res.json(stocks);
});

router.post("/", auth, async (req, res) => {
  const newStock = new Stock(req.body);
  const savedStock = await newStock.save();
  res.json(savedStock);
});

module.exports = router;
```

---

### 📝 Step 6: Cấu hình server chính  
**📁 server.js**
```js
const express = require("express");
const mongoose = require("mongoose");
const dotenv = require("dotenv");
const authRoutes = require("./routes/auth");
const stockRoutes = require("./routes/stocks");

dotenv.config();
const app = express();
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/jwt-auth", { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log("✅ MongoDB Connected"))
  .catch(err => console.error(err));

app.use("/api/auth", authRoutes);
app.use("/api/stocks", stockRoutes);

app.listen(5000, () => console.log("🚀 Server chạy tại http://localhost:5000"));
```

---

### 📝 Step 7: Thêm biến môi trường  
**📁 .env**
```bash
JWT_SECRET=supersecretkey123
```

---

## 🚀 Kết quả

✅ Người dùng có thể **đăng ký và đăng nhập**  
✅ Sau khi đăng nhập, **server trả về JWT**  
✅ Các route quan trọng được **bảo vệ bằng middleware**  
✅ Chỉ người dùng có **token hợp lệ** mới truy cập được  

---

## 📌 Kết luận

Xác thực bằng **JWT** là phương pháp phổ biến và hiệu quả trong các ứng dụng web hiện đại.  
Sau bài này, bạn có thể:

- Tích hợp xác thực vào bất kỳ API nào  
- Quản lý phiên đăng nhập mà không cần session  
- Mở rộng hệ thống với phân quyền (admin, user)  
- Kết nối frontend để gửi token qua header
