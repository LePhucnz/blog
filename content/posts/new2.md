---
title: "Tạo ứng dụng chat đơn giản bằng Java Socket"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Hướng dẫn xây dựng ứng dụng chat cơ bản giữa client và server bằng Java Socket, giúp hiểu rõ cách giao tiếp qua giao thức TCP."
tags: ["Java", "Socket", "Ứng dụng mạng", "Client-Server"]
categories: ["Java"]
---

## 💬 Mục tiêu bài viết

Trong bài viết này, chúng ta sẽ cùng nhau xây dựng một ứng dụng **chat đơn giản** giữa **client** và **server** sử dụng **Java Socket**.  
Đây là ví dụ điển hình giúp hiểu rõ cách giao tiếp giữa hai máy thông qua giao thức TCP — nền tảng của lập trình mạng.

Ứng dụng này sẽ giúp em:

- Hiểu cách thiết lập kết nối giữa hai máy  
- Biết cách truyền dữ liệu qua luồng đầu vào/ra  
- Làm quen với mô hình client-server trong thực tế

---

## 🧩 Kiến thức cần nắm

Trước khi bắt đầu, em cần nắm một số khái niệm cơ bản:

- **Socket:** Điểm cuối của kết nối mạng. Trong Java, `Socket` đại diện cho kết nối giữa client và server.  
- **ServerSocket:** Lớp dùng để tạo server, lắng nghe và chấp nhận kết nối từ client.  
- **InputStream / OutputStream:** Luồng dữ liệu dùng để đọc và ghi thông tin giữa hai bên.

---

## 🖥️ Cấu trúc ứng dụng

Ứng dụng gồm 2 phần chính:

1. **Server.java** – Chạy trước, lắng nghe kết nối từ client, nhận và phản hồi tin nhắn.  
2. **Client.java** – Kết nối đến server, gửi tin nhắn và nhận phản hồi.

Mô hình này mô phỏng cách hoạt động của các ứng dụng chat như **Messenger**, **Zalo**, hoặc các hệ thống giao tiếp nội bộ.

---

## 🔧 Mã nguồn ứng dụng

### 🔹 Server.java – Máy chủ

```java
import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(1234);
        System.out.println("Server đang chạy và lắng nghe kết nối...");

        Socket clientSocket = serverSocket.accept();
        System.out.println("Client đã kết nối!");

        BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
        PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);

        String message;
        while ((message = in.readLine()) != null) {
            System.out.println("Client: " + message);
            out.println("Server nhận: " + message);
        }

        serverSocket.close();
    }
}
```

### 🔹 Client.java – Máy khách

```java
import java.io.*;
import java.net.*;

public class Client {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 1234);
        System.out.println("Đã kết nối đến server!");

        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));

        String input;
        while ((input = userInput.readLine()) != null) {
            out.println(input);
            System.out.println("Phản hồi từ server: " + in.readLine());
        }

        socket.close();
    }
}
```

---

## 🧪 Cách chạy ứng dụng

### 1️⃣ Biên dịch cả hai file:
```
javac Server.java
javac Client.java
```

### 2️⃣ Chạy server trước:
```
java Server
```

### 3️⃣ Mở cửa sổ terminal khác và chạy client:
```
java Client
```

Sau đó, nhập tin nhắn từ client và xem phản hồi từ server.

---

## 📦 Mở rộng ứng dụng

Sau khi hoàn thành phiên bản cơ bản, em có thể nâng cấp ứng dụng theo các hướng sau:

- Cho phép **nhiều client kết nối cùng lúc** (dùng `Thread` hoặc `ExecutorService`)  
- Thêm **giao diện đồ họa** bằng JavaFX hoặc Swing  
- **Lưu lịch sử chat** vào file hoặc cơ sở dữ liệu  
- **Mã hóa dữ liệu** để tăng bảo mật (sử dụng AES hoặc SSL Socket)  
- Tạo **phòng chat nhóm** hoặc **chat riêng tư**

---

## 📌 Kết luận

Ứng dụng chat đơn giản bằng **Java Socket** là một ví dụ thực tế giúp em hiểu rõ cách giao tiếp giữa hai máy qua mạng.  
Đây là nền tảng để xây dựng các hệ thống phức tạp hơn như:

- Chat đa người dùng  
- Truyền file  
- Dịch vụ mạng nội bộ  

Việc nắm vững **lập trình Socket** sẽ giúp em tiến xa hơn trong các lĩnh vực như:

- Phát triển ứng dụng mạng  
- Lập trình hệ thống phân tán  
- Xây dựng backend cho ứng dụng thời gian thực
