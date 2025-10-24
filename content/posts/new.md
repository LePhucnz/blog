---
title: "Giới thiệu về ngôn ngữ lập trình Java"
date: 2025-10-24
author: "Hồ Nguyễn Tấn Đạt"
draft: false 
description: "Tìm hiểu tổng quan về Java – ngôn ngữ lập trình mạnh mẽ, hướng đối tượng và đa nền tảng."
tags: ["Java", "Lập trình hướng đối tượng", "Công nghệ phần mềm"]
categories: ["Java"]
---

## 🧠 Java là gì?

**Java** là một ngôn ngữ lập trình hướng đối tượng (OOP – Object Oriented Programming), được phát triển bởi **Sun Microsystems** (nay thuộc **Oracle**).  
Khác với nhiều ngôn ngữ khác, Java nổi bật nhờ khả năng **"viết một lần, chạy mọi nơi"** (*Write Once, Run Anywhere*).

Điều này có nghĩa là chương trình viết bằng Java có thể chạy trên mọi hệ điều hành (Windows, Linux, macOS…) nếu có cài đặt **Java Virtual Machine (JVM)**.

---

## ⚙️ Đặc điểm nổi bật của Java

1. 🔹 **Đa nền tảng:**  
   Java biên dịch ra mã trung gian (bytecode) và JVM sẽ chạy bytecode đó trên mọi hệ thống.

2. 🔹 **Hướng đối tượng:**  
   Mọi thứ trong Java đều là đối tượng – giúp tổ chức mã rõ ràng, dễ mở rộng và tái sử dụng.

3. 🔹 **Bảo mật cao:**  
   Java được thiết kế để hạn chế truy cập trực tiếp vào bộ nhớ và có mô hình bảo mật mạnh.

4. 🔹 **Quản lý bộ nhớ tự động:**  
   Trình dọn rác (Garbage Collector) tự động giải phóng vùng nhớ không còn dùng đến.

5. 🔹 **Thư viện phong phú:**  
   Java cung cấp hàng nghìn thư viện hỗ trợ xử lý file, mạng, cơ sở dữ liệu, GUI, v.v.

---

## 💻 Ví dụ đầu tiên với Java

Dưới đây là chương trình “Hello World” kinh điển trong Java:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Xin chào thế giới Java!");
    }
}
```