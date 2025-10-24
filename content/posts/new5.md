---
title: "So sánh TCP và UDP trong lập trình mạng"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Phân biệt TCP và UDP, hai giao thức truyền tải phổ biến trong lập trình mạng, kèm ví dụ minh họa bằng Java."
tags: ["Java", "TCP", "UDP", "Lập trình mạng", "Socket"]
categories: ["Java"]
---

## 🌐 Giới thiệu

Trong lập trình mạng, hai giao thức phổ biến nhất để truyền dữ liệu là **TCP (Transmission Control Protocol)** và **UDP (User Datagram Protocol)**. Mỗi giao thức có đặc điểm riêng, phù hợp với từng loại ứng dụng khác nhau.

Bài viết này sẽ giúp em hiểu rõ sự khác biệt giữa TCP và UDP, cách sử dụng chúng trong Java, và khi nào nên chọn giao thức nào.

---

## 🔍 So sánh TCP và UDP

| Tiêu chí              | TCP                                      | UDP                                      |
|-----------------------|-------------------------------------------|-------------------------------------------|
| Kiểu kết nối          | Có kết nối (connection-oriented)         | Không kết nối (connectionless)            |
| Đảm bảo dữ liệu       | Có (gửi đúng thứ tự, không mất gói)      | Không đảm bảo (có thể mất gói, sai thứ tự)|
| Tốc độ truyền         | Chậm hơn do kiểm tra lỗi và xác nhận     | Nhanh hơn vì không kiểm tra lỗi           |
| Ứng dụng phổ biến     | Web, Email, FTP, Chat                    | Video streaming, game online, VoIP        |
| Mức độ tin cậy        | Cao                                      | Thấp                                      |

---

## 🧪 Ví dụ: TCP và UDP trong Java

### 🔹 TCP Server
```java
import java.io.*;
import java.net.*;

public class TCPServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(1234);
        System.out.println("TCP Server đang chạy...");

        Socket socket = serverSocket.accept();
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

        String message = in.readLine();
        System.out.println("Client gửi: " + message);
        out.println("Server nhận: " + message);

        socket.close();
        serverSocket.close();
    }
}
```

### 🔹 TCP Client
```java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 1234);
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

        out.println("Xin chào TCP Server!");
        System.out.println("Phản hồi từ server: " + in.readLine());

        socket.close();
    }
}
```

### 🔹 UDP Server
```java
import java.net.*;

public class UDPServer {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(1234);
        byte[] buffer = new byte[1024];

        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
        System.out.println("UDP Server đang chạy...");
        socket.receive(packet);

        String message = new String(packet.getData(), 0, packet.getLength());
        System.out.println("Client gửi: " + message);

        String response = "Server nhận: " + message;
        byte[] responseData = response.getBytes();
        DatagramPacket responsePacket = new DatagramPacket(
            responseData, responseData.length, packet.getAddress(), packet.getPort()
        );
        socket.send(responsePacket);
        socket.close();
    }
}
```

### 🔹 UDP Client
```java
import java.net.*;

public class UDPClient {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket();
        byte[] data = "Xin chào UDP Server!".getBytes();

        InetAddress address = InetAddress.getByName("localhost");
        DatagramPacket packet = new DatagramPacket(data, data.length, address, 1234);
        socket.send(packet);

        byte[] buffer = new byte[1024];
        DatagramPacket response = new DatagramPacket(buffer, buffer.length);
        socket.receive(response);

        String reply = new String(response.getData(), 0, response.getLength());
        System.out.println("Phản hồi từ server: " + reply);
        socket.close();
    }
}
```

---

## 📌 Kết luận

- **TCP** phù hợp với các ứng dụng cần độ tin cậy cao như chat, email, truyền file.  
- **UDP** phù hợp với các ứng dụng cần tốc độ cao, chấp nhận mất dữ liệu như video, game, truyền âm thanh.  

Việc hiểu rõ sự khác biệt giữa TCP và UDP sẽ giúp em chọn đúng giao thức cho từng loại ứng dụng mạng, từ đó tối ưu hiệu năng và trải nghiệm người dùng.
