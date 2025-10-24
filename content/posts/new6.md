---
title: "Giới thiệu giao thức HTTP và cách gửi yêu cầu bằng Java"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Tìm hiểu giao thức HTTP và cách gửi yêu cầu GET/POST bằng Java sử dụng HttpURLConnection."
tags: ["Java", "HTTP", "HttpURLConnection", "Lập trình mạng"]
categories: ["Java"]
---

## 🌐 HTTP là gì?

**HTTP (HyperText Transfer Protocol)** là giao thức truyền tải dữ liệu giữa client và server trong môi trường web. Khi em truy cập một trang web, trình duyệt sẽ gửi yêu cầu HTTP đến server, và server sẽ phản hồi lại bằng nội dung HTML, JSON, hình ảnh, v.v.

---

## 🔍 Các phương thức HTTP phổ biến

| Phương thức | Mô tả |
|-------------|-------|
| GET         | Lấy dữ liệu từ server |
| POST        | Gửi dữ liệu lên server |
| PUT         | Cập nhật dữ liệu |
| DELETE      | Xóa dữ liệu |

---

## 🧪 Gửi yêu cầu HTTP bằng Java

Trong Java, em có thể sử dụng lớp `HttpURLConnection` để gửi yêu cầu HTTP đến một địa chỉ URL.

---

### 🔹 Gửi yêu cầu GET
```java
import java.io.*;
import java.net.*;

public class HttpGetExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://jsonplaceholder.typicode.com/posts/1");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");

        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String inputLine;
        StringBuilder response = new StringBuilder();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();

        System.out.println("Phản hồi từ server:");
        System.out.println(response.toString());
    }
}
```

### 🔹 Gửi yêu cầu POST
```java
import java.io.*;
import java.net.*;

public class HttpPostExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://jsonplaceholder.typicode.com/posts");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json; utf-8");
        conn.setDoOutput(true);

        String jsonInput = "{\"title\":\"Phúc\",\"body\":\"Hello HTTP!\",\"userId\":1}";

        try (OutputStream os = conn.getOutputStream()) {
            byte[] input = jsonInput.getBytes("utf-8");
            os.write(input, 0, input.length);
        }

        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"));
        StringBuilder response = new StringBuilder();
        String line;

        while ((line = in.readLine()) != null) {
            response.append(line.trim());
        }
        in.close();

        System.out.println("Phản hồi từ server:");
        System.out.println(response.toString());
    }
}
```

---

## 📦 Mở rộng

Sau khi hiểu cách gửi yêu cầu HTTP bằng Java, em có thể:
- Sử dụng thư viện **HttpClient (Java 11+)** để viết code gọn hơn  
- Gửi yêu cầu có **Header**, **Token**, hoặc **Cookie**  
- Xử lý **mã trạng thái HTTP** (200, 404, 500...)  
- Tích hợp với **REST API** trong các dự án thực tế  

---

## 📌 Kết luận

**HTTP** là nền tảng của web hiện đại. Việc biết cách gửi yêu cầu HTTP bằng Java sẽ giúp em:
- Tương tác với **REST API**  
- Kết nối **frontend** và **backend**  
- Kiểm thử dịch vụ web  
- Xây dựng **ứng dụng mạng chuyên nghiệp**
