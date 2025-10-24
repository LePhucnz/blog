---
title: "WebSocket trong JavaScript: Giao tiếp thời gian thực"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Tìm hiểu cách sử dụng WebSocket trong JavaScript để xây dựng ứng dụng giao tiếp thời gian thực giữa client và server."
tags: ["JavaScript", "WebSocket", "Realtime", "Lập trình mạng"]
categories: ["JavaScript"]
---

## 🌐 WebSocket là gì?

**WebSocket** là một giao thức mạng cho phép thiết lập kết nối hai chiều (full-duplex) giữa client và server thông qua một kết nối TCP duy nhất. Khác với HTTP, WebSocket duy trì kết nối liên tục, giúp truyền dữ liệu nhanh chóng mà không cần gửi lại toàn bộ yêu cầu mỗi lần.

### 🔄 So sánh WebSocket với HTTP:

| Tiêu chí         | HTTP                          | WebSocket                      |
|------------------|-------------------------------|--------------------------------|
| Kiểu kết nối     | Một chiều (client → server)   | Hai chiều (client ↔ server)    |
| Tốc độ phản hồi  | Chậm hơn do phải gửi lại yêu cầu | Nhanh hơn, kết nối liên tục   |
| Ứng dụng phổ biến| Website tĩnh, API REST        | Chat, game, thông báo realtime |

---

## 🎯 Khi nào nên dùng WebSocket?

WebSocket rất hữu ích khi em cần:
- Giao tiếp thời gian thực (chat, thông báo, trạng thái người dùng)
- Cập nhật dữ liệu liên tục mà không cần reload trang
- Xây dựng game online hoặc hệ thống giám sát trực tiếp

---

## 🧪 Ví dụ: Tạo ứng dụng chat đơn giản bằng WebSocket

### 📁 Cấu trúc dự án

```
websocket-chat/
├── server.js       // File tạo WebSocket server bằng Node.js
└── index.html      // Giao diện client kết nối và gửi tin nhắn qua WebSocket
```

### 🔹 Bước 1: Tạo server WebSocket bằng Node.js

**server.js**
```javascript
const WebSocket = require('ws');
const server = new WebSocket.Server({ port: 8080 });

server.on('connection', socket => {
    console.log('Client đã kết nối');

    socket.on('message', message => {
        console.log('Tin nhắn từ client:', message);
        socket.send(`Server nhận: ${message}`);
    });

    socket.on('close', () => {
        console.log('Client đã ngắt kết nối');
    });
});
```

Để chạy server, bạn cần cài thư viện **ws**:

```
npm install ws
node server.js
```

### 🔹 Bước 2: Tạo giao diện client bằng HTML + JavaScript

**index.html**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Chat WebSocket</title>
</head>
<body>
    <h2>Ứng dụng Chat WebSocket</h2>
    <input type="text" id="messageInput" placeholder="Nhập tin nhắn..." />
    <button onclick="sendMessage()">Gửi</button>
    <div id="chatLog"></div>

    <script>
        const socket = new WebSocket('ws://localhost:8080');

        socket.onopen = () => {
            console.log('Đã kết nối đến server');
        };

        socket.onmessage = event => {
            const chatLog = document.getElementById('chatLog');
            chatLog.innerHTML += `<p>${event.data}</p>`;
        };

        function sendMessage() {
            const input = document.getElementById('messageInput');
            socket.send(input.value);
            input.value = '';
        }
    </script>
</body>
</html>
```

### 🧪 Cách chạy thử

Mở terminal, chạy server:
```
node server.js
```

Mở file **index.html** bằng trình duyệt.  
Nhập tin nhắn và nhấn “Gửi” để thấy phản hồi từ server.

---

### 📦 Mở rộng ứng dụng

Sau khi hiểu cách hoạt động cơ bản, em có thể nâng cấp ứng dụng:
- Cho phép nhiều client kết nối và chat với nhau (broadcast)
- Thêm giao diện đẹp bằng CSS hoặc framework như Bootstrap
- Lưu lịch sử chat vào cơ sở dữ liệu
- Tích hợp xác thực người dùng
- Dùng WebSocket kết hợp với Express hoặc NestJS

---

### 📌 Kết luận

**WebSocket** là công nghệ mạnh mẽ giúp xây dựng các ứng dụng thời gian thực hiệu quả.  
Việc hiểu và sử dụng WebSocket trong JavaScript sẽ giúp em phát triển các hệ thống hiện đại như:
- Chat nội bộ công ty  
- Hệ thống thông báo realtime  
- Game đa người chơi  
- Dashboard giám sát trực tiếp
