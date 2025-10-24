---
title: "REST API là gì? Cách xây dựng với Spring Boot"
date: 2025-10-24
author: "Lê Mai Hoàng Phúc"
draft: false 
description: "Giới thiệu chi tiết về RESTful API và hướng dẫn từng bước xây dựng API với Spring Boot trong Java."
tags: ["Java", "Spring Boot", "REST API", "Backend", "Lập trình mạng"]
categories: ["Java"]
---

## 🌐 REST API là gì?

**REST API** (Representational State Transfer Application Programming Interface) là một kiểu kiến trúc phần mềm giúp các hệ thống giao tiếp với nhau thông qua giao thức HTTP.  
REST API thường được sử dụng để xây dựng các dịch vụ web hiện đại, nơi client (trình duyệt, ứng dụng di động...) có thể gửi yêu cầu đến server để lấy hoặc thay đổi dữ liệu.

---

### 🔑 Đặc điểm chính của REST API:

- **Stateless (Không trạng thái):** Mỗi yêu cầu từ client đến server phải chứa đầy đủ thông tin để server xử lý.  
- **Sử dụng HTTP:** REST tận dụng các phương thức HTTP như `GET`, `POST`, `PUT`, `DELETE` để thực hiện các thao tác CRUD (Create, Read, Update, Delete).  
- **Dữ liệu trả về thường ở định dạng JSON hoặc XML**, trong đó JSON phổ biến nhất vì nhẹ và dễ xử lý.  
- **Tài nguyên được định danh bằng URL:** Ví dụ: `/users`, `/products`, `/orders`

---

## 🚀 Tại sao nên dùng Spring Boot để xây REST API?

**Spring Boot** là một framework mạnh mẽ trong hệ sinh thái Java, giúp lập trình viên xây dựng ứng dụng web và dịch vụ backend một cách **nhanh chóng – dễ dàng – hiệu quả**.

### ⚡ Ưu điểm của Spring Boot:

- ✅ Cấu hình tối giản, khởi tạo nhanh  
- ✅ Tích hợp sẵn các công cụ như Tomcat, Jetty  
- ✅ Hỗ trợ mạnh cho RESTful API qua annotation: `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, ...  
- ✅ Dễ dàng tích hợp với JPA, Hibernate, Security, Swagger, v.v.

---

## 🧩 Bước 2: Viết Controller đầu tiên

Tạo một lớp controller để định nghĩa các endpoint REST API — nơi xử lý yêu cầu từ client.

```java
📄 HelloController.java

package com.example.demo.controllers;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/hello")
public class HelloController {

    @GetMapping("")
    public String sayHello() {
        return "Xin chào từ REST API!";
    }

    @PostMapping("")
    public String receiveHello(@RequestBody String name) {
        return "Chào bạn, " + name + "!";
    }
}
```

---

## 💠 Bước 3: Cấu hình ứng dụng

Đây là lớp khởi động chính của Spring Boot. Khi chạy lớp này, ứng dụng sẽ được khởi tạo, các controller sẽ được quét và server sẽ bắt đầu lắng nghe yêu cầu.

```java
📄 DemoApplication.java

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

### 📝 Giải thích:
- `@SpringBootApplication`: Annotation tổng hợp giúp Spring Boot tự động cấu hình.  
- `SpringApplication.run(...)`: Khởi động ứng dụng và mở cổng mặc định (8080).

---

## 🧪 Kiểm thử REST API

Sau khi chạy ứng dụng bằng lệnh:

```
mvn spring-boot:run
```

### 🔹 Gửi yêu cầu GET:
```
curl http://localhost:8080/hello
```
➡️ **Kết quả:**
```
Xin chào từ REST API!
```

### 🔹 Gửi yêu cầu POST:
```
curl -X POST http://localhost:8080/hello -H "Content-Type: text/plain" -d "Phúc"
```
➡️ **Kết quả:**
```
Chào bạn, Phúc!
```

---

## 🔧 Mở rộng REST API

Sau khi hiểu cách tạo API đơn giản, bạn có thể mở rộng bằng cách:

### 🏗️ Thêm các lớp:
- **Model:** Biểu diễn dữ liệu (User, Product, Order...)  
- **Service:** Xử lý logic nghiệp vụ  
- **Repository:** Kết nối cơ sở dữ liệu (JPA hoặc JDBC)

### ⚡ Thêm tính năng nâng cao:
- Phân trang & lọc dữ liệu  
- Xác thực người dùng (JWT, OAuth2)  
- Quản lý lỗi & exception  
- Tài liệu API bằng Swagger UI  

---

## 📌 Kết luận

**REST API** là nền tảng quan trọng trong phát triển phần mềm hiện đại, đặc biệt khi xây dựng **ứng dụng web, mobile hoặc hệ thống phân tán**.  
Việc sử dụng **Spring Boot** giúp lập trình viên Java triển khai REST API nhanh, dễ bảo trì và mở rộng.

---

## 🌱 Ứng dụng thực tế

- Tạo **backend cho blog cá nhân**  
- Kết nối **frontend (React, Angular, Vue)** với server  
- Xây dựng **hệ thống quản lý người dùng, bài viết, bình luận...**  
- Tích hợp với **mobile app** hoặc **hệ thống IoT**
