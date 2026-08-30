# 📘 Learning TypeScript (Bản dịch Tiếng Việt)

> **Bản dịch tiếng Việt song ngữ Markdown của cuốn sách *"Learning TypeScript: Enhancing Your Web Development with Type-Safe JavaScript"* của tác giả Josh Goldberg (O'Reilly Media).**

[![Language: Vietnamese](https://img.shields.io/badge/Language-Tiếng%20Việt%20%7C%20English-blue.svg)](README.md)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6.svg?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Format: Markdown](https://img.shields.io/badge/Format-Markdown-083fae.svg)](chapters_vi/)

---

## 📖 Giới thiệu (Introduction)

**Learning TypeScript** là một trong những cuốn sách hay và toàn diện nhất về TypeScript do **Josh Goldberg** biên soạn. Cuốn sách không chỉ hướng dẫn cú pháp TypeScript mà còn giải thích sâu sắc về tư duy hệ thống kiểu (type system), cách TypeScript hiểu code JavaScript của bạn, và làm thế nào để xây dựng các ứng dụng web quy mô lớn an toàn, ít lỗi và dễ bảo trì.

Kho lưu trữ này cung cấp:
- 🇻🇳 **Bản dịch Tiếng Việt đầy đủ (`chapters_vi/`)**: Được dịch thuật cẩn thận, giữ nguyên ngữ nghĩa kỹ thuật chuẩn, thuật ngữ chuyên ngành có kèm chú thích gốc.
- 🇬🇧 **Bản gốc Tiếng Anh (`chapters_en/`)**: Toàn bộ nội dung gốc định dạng Markdown tiện đối chiếu song ngữ.
- 🖼️ **Hình ảnh minh họa & Sơ đồ**: Đầy đủ ảnh minh họa trực quan từ sách gốc.

---

## 📂 Cấu trúc thư mục (Repository Structure)

```text
├── chapters_vi/        # Bản dịch Tiếng Việt (Markdown + Hình ảnh minh họa)
│   ├── images/         # Sơ đồ và ảnh chụp minh họa
│   └── 00_*.md ...    # Các chương 00 - 18 bằng Tiếng Việt
├── chapters_en/        # Bản gốc Tiếng Anh (English Markdown + Images)
│   ├── images/         # Original diagrams & screenshots
│   └── 00_*.md ...    # Original chapters in English
├── LICENSE             # Giấy phép MIT
├── .gitignore          # Cấu hình bỏ qua các file công cụ / tạm thời
└── README.md           # Hướng dẫn và Mục lục sách
```

---

## 📑 Mục lục sách (Table of Contents)

Dưới đây là danh sách đầy đủ các chương sách. Bạn có thể bấm vào liên kết để đọc trực tiếp bản **Tiếng Việt** hoặc bản **Tiếng Anh**:

| # | Tên chương (Tiêu đề Tiếng Việt / Tiếng Anh) | Bản Tiếng Việt 🇻🇳 | Bản Tiếng Anh 🇬🇧 |
|---|---|:---:|:---:|
| **00** | **Thông tin xuất bản & Lời khen ngợi** *(Front Matter & Praise)* | [Đọc Tiếng Việt](chapters_vi/00_Front_Matter.md) | [Read English](chapters_en/00_Front_Matter.md) |
| **00** | **Lời nói đầu** *(Preface)* | [Đọc Tiếng Việt](chapters_vi/00_Preface.md) | [Read English](chapters_en/00_Preface.md) |
| | <th colspan="3" align="left">**Phần I. Khái niệm (Part I. Concepts)**</th> |
| **01** | **Chương 1: Từ JavaScript sang TypeScript** *(From JavaScript to TypeScript)* | [Đọc Tiếng Việt](chapters_vi/01_Chapter_01_From_JavaScript_to_TypeScript.md) | [Read English](chapters_en/01_Chapter_01_From_JavaScript_to_TypeScript.md) |
| **02** | **Chương 2: Hệ thống kiểu** *(The Type System)* | [Đọc Tiếng Việt](chapters_vi/02_Chapter_02_The_Type_System.md) | [Read English](chapters_en/02_Chapter_02_The_Type_System.md) |
| **03** | **Chương 3: Unions và Literals** *(Unions and Literals)* | [Đọc Tiếng Việt](chapters_vi/03_Chapter_03_Unions_and_Literals.md) | [Read English](chapters_en/03_Chapter_03_Unions_and_Literals.md) |
| **04** | **Chương 4: Đối tượng** *(Objects)* | [Đọc Tiếng Việt](chapters_vi/04_Chapter_04_Objects.md) | [Read English](chapters_en/04_Chapter_04_Objects.md) |
| | <th colspan="3" align="left">**Phần II. Các tính năng (Part II. Features)**</th> |
| **05** | **Chương 5: Hàm** *(Functions)* | [Đọc Tiếng Việt](chapters_vi/05_Chapter_05_Functions.md) | [Read English](chapters_en/05_Chapter_05_Functions.md) |
| **06** | **Chương 6: Mảng** *(Arrays)* | [Đọc Tiếng Việt](chapters_vi/06_Chapter_06_Arrays.md) | [Read English](chapters_en/06_Chapter_06_Arrays.md) |
| **07** | **Chương 7: Giao diện** *(Interfaces)* | [Đọc Tiếng Việt](chapters_vi/07_Chapter_07_Interfaces.md) | [Read English](chapters_en/07_Chapter_07_Interfaces.md) |
| **08** | **Chương 8: Lớp** *(Classes)* | [Đọc Tiếng Việt](chapters_vi/08_Chapter_08_Classes.md) | [Read English](chapters_en/08_Chapter_08_Classes.md) |
| **09** | **Chương 9: Các bộ bổ từ kiểu** *(Type Modifiers)* | [Đọc Tiếng Việt](chapters_vi/09_Chapter_09_Type_Modifiers.md) | [Read English](chapters_en/09_Chapter_09_Type_Modifiers.md) |
| **10** | **Chương 10: Kiểu tổng quát** *(Generics)* | [Đọc Tiếng Việt](chapters_vi/10_Chapter_10_Generics.md) | [Read English](chapters_en/10_Chapter_10_Generics.md) |
| | <th colspan="3" align="left">**Phần III. Sử dụng trong thực tế (Part III. Usage)**</th> |
| **11** | **Chương 11: Các tệp khai báo** *(Declaration Files)* | [Đọc Tiếng Việt](chapters_vi/11_Chapter_11_Declaration_Files.md) | [Read English](chapters_en/11_Chapter_11_Declaration_Files.md) |
| **12** | **Chương 12: Sử dụng các tính năng IDE** *(Using IDE Features)* | [Đọc Tiếng Việt](chapters_vi/12_Chapter_12_Using_IDE_Features.md) | [Read English](chapters_en/12_Chapter_12_Using_IDE_Features.md) |
| **13** | **Chương 13: Các tùy chọn cấu hình** *(Configuration Options)* | [Đọc Tiếng Việt](chapters_vi/13_Chapter_13_Configuration_Options.md) | [Read English](chapters_en/13_Chapter_13_Configuration_Options.md) |
| | <th colspan="3" align="left">**Phần IV. Điểm cộng thêm (Part IV. Extra Credit)**</th> |
| **14** | **Chương 14: Phần mở rộng cú pháp** *(Syntax Extensions)* | [Đọc Tiếng Việt](chapters_vi/14_Chapter_14_Syntax_Extensions.md) | [Read English](chapters_en/14_Chapter_14_Syntax_Extensions.md) |
| **15** | **Chương 15: Các thao tác kiểu** *(Type Operations)* | [Đọc Tiếng Việt](chapters_vi/15_Chapter_15_Type_Operations.md) | [Read English](chapters_en/15_Chapter_15_Type_Operations.md) |
| | <th colspan="3" align="left">**Phụ lục & Thông tin thêm (Appendix)**</th> |
| **16** | **Bảng thuật ngữ** *(Glossary)* | [Đọc Tiếng Việt](chapters_vi/16_Glossary.md) | [Read English](chapters_en/16_Glossary.md) |
| **17** | **Chỉ mục** *(Index)* | [Đọc Tiếng Việt](chapters_vi/17_Index.md) | [Read English](chapters_en/17_Index.md) |
| **18** | **Về tác giả** *(About the Author)* | [Đọc Tiếng Việt](chapters_vi/18_About_the_Author.md) | [Read English](chapters_en/18_About_the_Author.md) |

---

## 💡 Hướng dẫn học tập & Sử dụng (Learning Guide)

1. **Đọc song ngữ**: Khuyến khích mở song song hai tab (hoặc chia đôi màn hình trong VSCode) bản [chapters_vi](chapters_vi/) và [chapters_en](chapters_en/) để vừa tiếp thu kiến thức nhanh vừa làm quen với các thuật ngữ chuyên ngành tiếng Anh.
2. **Thực hành code**: Đừng chỉ đọc lý thuyết! Hãy tạo một dự án TypeScript nhỏ hoặc sử dụng [TypeScript Playground](https://www.typescriptlang.org/play) để tự tay gõ và kiểm chứng các đoạn code mẫu trong từng chương.
3. **Tra cứu thuật ngữ**: Khi gặp các khái niệm lạ hoặc chưa rõ nghĩa, hãy tham khảo [Chương 16 - Bảng thuật ngữ](chapters_vi/16_Glossary.md).

---

## 🤝 Đóng góp (Contributing)

Dự án dịch thuật này được thực hiện với tinh thần chia sẻ kiến thức phi lợi nhuận cho cộng đồng lập trình viên Việt Nam. Nếu bạn phát hiện:
- Lỗi chính tả, lỗi dịch thuật hoặc câu văn chưa mượt.
- Lỗi định dạng Markdown hoặc liên kết hỏng.
- Thuật ngữ kỹ thuật cần được cải thiện.

Vui lòng mở một **Issue** hoặc gửi một **Pull Request**. Mọi sự đóng góp của bạn đều rất đáng trân trọng!

---

## 📜 Tuyên bố miễn trừ trách nhiệm & Bản quyền (Disclaimer & License)

- Cuốn sách gốc **"Learning TypeScript"** thuộc bản quyền của tác giả **Josh Goldberg** và nhà xuất bản **O'Reilly Media, Inc.**
- Bản dịch này nhằm mục đích học tập, nghiên cứu và phi thương mại.
- Mã nguồn và tài liệu trong repo này được chia sẻ theo giấy phép [MIT License](LICENSE).
