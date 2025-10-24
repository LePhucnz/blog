---
title: "Kết nối React với API Node.js"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Hướng dẫn cách kết nối frontend React với backend Node.js thông qua REST API để hiển thị dữ liệu cổ phiếu."
tags: ["React", "Node.js", "REST API", "Frontend", "Backend"]
categories: ["JavaScript"]
---

## 🌐 Mục tiêu bài viết

Sau khi đã xây dựng xong backend bằng Node.js và MongoDB (bài 6), bài này sẽ hướng dẫn cách kết nối **React frontend** với API đó để hiển thị danh sách cổ phiếu và thêm mới cổ phiếu từ giao diện người dùng.

---

## 🧩 Cấu trúc dự án
```
portfolio-app/
├── backend/        // API Node.js đã hoàn thành ở bài trước
└── frontend/       // React app sẽ tạo trong bài này
```

---

## 📝 Step 1: Tạo ứng dụng React
```bash
npx create-react-app frontend
cd frontend
npm install axios
```

---

## 📝 Step 2: Tạo component hiển thị danh sách cổ phiếu
**src/components/StockList.js**
```javascript
import React, { useEffect, useState } from "react";
import axios from "axios";

const StockList = () => {
  const [stocks, setStocks] = useState([]);

  useEffect(() => {
    axios.get("http://localhost:5000/api/stocks")
      .then(res => setStocks(res.data))
      .catch(err => console.error(err));
  }, []);

  return (
    <div>
      <h2>Danh sách cổ phiếu</h2>
      <ul>
        {stocks.map(stock => (
          <li key={stock._id}>
            {stock.symbol} - {stock.shares} cổ phiếu @ {stock.purchasePrice}$
          </li>
        ))}
      </ul>
    </div>
  );
};

export default StockList;
```

---

## 📝 Step 3: Tạo form thêm cổ phiếu mới
**src/components/AddStock.js**
```javascript
import React, { useState } from "react";
import axios from "axios";

const AddStock = () => {
  const [form, setForm] = useState({
    symbol: "",
    shares: "",
    purchasePrice: ""
  });

  const handleChange = e => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = e => {
    e.preventDefault();
    axios.post("http://localhost:5000/api/stocks", form)
      .then(res => {
        alert("Thêm thành công!");
        setForm({ symbol: "", shares: "", purchasePrice: "" });
      })
      .catch(err => console.error(err));
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Thêm cổ phiếu</h2>
      <input name="symbol" value={form.symbol} onChange={handleChange} placeholder="Mã cổ phiếu" />
      <input name="shares" value={form.shares} onChange={handleChange} placeholder="Số lượng" />
      <input name="purchasePrice" value={form.purchasePrice} onChange={handleChange} placeholder="Giá mua" />
      <button type="submit">Thêm</button>
    </form>
  );
};

export default AddStock;
```

---

## 📝 Step 4: Sử dụng các component trong App.js
**src/App.js**
```javascript
import React from "react";
import StockList from "./components/StockList";
import AddStock from "./components/AddStock";

function App() {
  return (
    <div className="App">
      <h1>Quản lý danh mục đầu tư</h1>
      <AddStock />
      <StockList />
    </div>
  );
}

export default App;
```

---

## 📝 Step 5: Cho phép CORS từ frontend
Trong file **backend/index.js**, đảm bảo đã có:
```javascript
app.use(cors());
```

---

## 🚀 Kết quả
Khi chạy cả **backend** và **frontend**:
- Người dùng có thể nhập thông tin cổ phiếu và gửi lên server  
- Dữ liệu sẽ được lưu vào **MongoDB**  
- Danh sách cổ phiếu sẽ được hiển thị ngay trên giao diện React  

---

## 📌 Kết luận
Việc kết nối **React** với **Node.js** thông qua **REST API** là bước quan trọng để xây dựng ứng dụng web hoàn chỉnh. Sau bài này, em có thể:
- Thêm chức năng **sửa và xóa cổ phiếu**  
- Hiển thị **biểu đồ đầu tư** bằng thư viện **Chart.js**  
- Tích hợp **xác thực người dùng** bằng **JWT**  
- Deploy cả **frontend** và **backend** lên **Vercel**, **Render**, hoặc **Railway**
