# Báo cáo Thực hành DevOps - Session 11: Bài 2

## Biên dịch đóng gói tệp ứng dụng .jar với Gradle và lưu trữ Artifacts

### 1. Giới thiệu tổng quan
Bài thực hành mở rộng quy trình CI từ Bài 1 bằng việc tích hợp giai đoạn đóng gói ứng dụng Spring Boot thành tệp lưu trữ khả thi `.jar` (sử dụng Gradle task `bootJar`). Đồng thời, cấu hình lưu trữ sản phẩm biên dịch dưới dạng Artifact trên GitHub Actions thông qua `actions/upload-artifact@v4` để phục vụ việc kiểm định, phân phối hoặc tải về máy cục bộ.

### 2. Cấu trúc thư mục bài nộp
```text
SS11_HW02_DaoTrongTri_PTIT062/
├── .github/
│   └── workflows/
│       └── ci.yml             # Kịch bản CI đóng gói bootJar và upload-artifact
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/com/ptit/devops_ss11/Application.java
│   │   └── resources/application.properties
│   └── test/
│       └── java/com/ptit/devops_ss11/ApplicationTests.java
├── .gitignore
├── build.gradle
├── gradlew
├── gradlew.bat
├── settings.gradle
├── README.md                  # Hướng dẫn tổng quan
└── REPORT.md                  # Báo cáo kỹ thuật phân tích chi tiết bootJar & Artifacts
```

### 3. Tóm tắt nội dung kịch bản CI (`.github/workflows/ci.yml`)
- **Tên Workflow:** `Java Spring Boot CI (Gradle)`
- **Sự kiện kích hoạt:** `push` lên nhánh `main`.
- **Môi trường chạy:** `ubuntu-latest`.
- **Các bước thực thi:**
  1. `actions/checkout@v5`: Kéo mã nguồn.
  2. `actions/setup-java@v5`: Cấu hình Temurin JDK 17.
  3. `chmod +x gradlew`: Cấp quyền thực thi Gradle Wrapper.
  4. `./gradlew test`: Chạy kiểm thử tự động.
  5. `./gradlew bootJar`: Biên dịch và đóng gói Fat JAR Spring Boot vào thư mục `build/libs/`.
  6. `actions/upload-artifact@v4`: Đính kèm tệp `build/libs/*.jar` với tên artifact là `app-jar`.

Chi tiết phân tích cơ chế kỹ thuật và hướng dẫn kiểm tra tải artifact xem tại [REPORT.md](REPORT.md).
