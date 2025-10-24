---
title: "Tạo API đơn giản bằng Node.js và Express"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false
description: "Hướng dẫn tạo REST API cơ bản bằng Node.js và Express để xử lý dữ liệu người dùng."
tags: ["Node.js", "Express", "REST API", "Lập trình mạng"]
categories: ["JavaScript"]
---

# 🌐 Tạo API đơn giản bằng Node.js và Express

## 🔍 REST API là gì?
**REST API** (Representational State Transfer) là giao diện lập trình cho phép **client** (trình duyệt, ứng dụng) giao tiếp với **server** thông qua các phương thức HTTP như `GET`, `POST`, `PUT`, `DELETE`.  
Đây là nền tảng của hầu hết các ứng dụng web và di động hiện đại.

---

## 🎯 Mục tiêu bài viết
Tạo một **API đơn giản** để quản lý danh sách người dùng với hai chức năng:
- ✅ Lấy danh sách người dùng  
- ✅ Thêm người dùng mới  

---

## 🧩 Bước 1: Khởi tạo dự án Node.js
Tạo thư mục mới và cài đặt Express:

```bash
mkdir SimpleAPI && cd SimpleAPI
npm init -y
npm install express
touch index.js
```

---

## 🧠 Bước 2: Viết mã API trong `index.js`
```js
const express = require("express");
const app = express();

app.use(express.json());

// Dữ liệu mẫu
let users = [
  { id: 1, name: "Phúc" },
  { id: 2, name: "Mai" }
];

// 📍 GET: Lấy danh sách người dùng
app.get("/api/users", (req, res) => {
  res.json(users);
});

// 📍 POST: Thêm người dùng mới
app.post("/api/users", (req, res) => {
  const newUser = {
    id: users.length + 1,
    name: req.body.name
  };
  users.push(newUser);
  res.json(newUser);
});

const PORT = 3000;
app.listen(PORT, () => console.log(`🚀 Server chạy tại cổng ${PORT}`));
```

---

## 🧪 Bước 3: Kiểm tra API bằng Postman hoặc cURL

🔹 **Lấy danh sách người dùng**
```bash
GET http://localhost:3000/api/users
```

🔹 **Thêm người dùng mới**
```bash
POST http://localhost:3000/api/users
Content-Type: application/json

{
  "name": "Ngọc"
}
```

Kết quả trả về:
```json
[
  { "id": 1, "name": "Phúc" },
  { "id": 2, "name": "Mai" },
  { "id": 3, "name": "Ngọc" }
]
```

---

## 🧭 Kết luận
Sau bài này, bạn đã biết cách:
- ⚙️ Tạo server bằng **Express**
- 📡 Viết các **route HTTP** cơ bản
- 💾 Quản lý dữ liệu tạm thời bằng **mảng**
- 🔗 Giao tiếp giữa **client và server**

Đây là nền tảng vững chắc để bạn phát triển các tính năng cao cấp hơn như:
- Kết nối **MongoDB / MySQL**  
- Thêm **xác thực người dùng (JWT)**  
- Triển khai **API lên cloud (Render, Vercel, hoặc Railway)**  

---

💡 *Gợi ý tiếp theo*: Ở bài sau, chúng ta sẽ học cách **kết nối API này với frontend React** để hiển thị danh sách người dùng và gửi dữ liệu trực tiếp từ trình duyệt.
