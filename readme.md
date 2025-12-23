Dưới đây là toàn bộ nội dung file `README.md` đã được gộp lại thành một khối duy nhất.

Bạn chỉ cần tạo file tên là **`README.md`** trong thư mục dự án, sau đó bấm nút **Copy** ở góc phải đoạn code dưới đây và dán vào là xong.

````markdown
# 📘 Hướng Dẫn Cài Đặt Dự Án (Laravel Docker/Sail)

Chào mừng các bạn đến với dự án! 👋
Dự án này sử dụng **Docker (Laravel Sail)** để chạy. Điều này giúp tất cả thành viên trong nhóm có môi trường code giống hệt nhau, tránh lỗi xung đột phiên bản giữa các máy.

---

## 🛠 Phần 1: Yêu Cầu Cần Có (Cài 1 lần duy nhất)

Trước khi tải code, hãy chắc chắn máy bạn đã cài:

1.  **Docker Desktop** (Bắt buộc - nhớ bật lên khi code).
2.  **Git**.
3.  **WSL 2** (Nếu dùng Windows): Phải bật tích hợp Ubuntu trong Docker Desktop (_Settings -> Resources -> WSL Integration_).
4.  **Quan trọng:** Tắt hẳn XAMPP, WAMP hoặc Laravel Herd để tránh xung đột cổng.

---

## 🚀 Phần 2: Các Bước Cài Đặt (Khi mới tải dự án về)

Hãy làm theo đúng thứ tự từng bước dưới đây:

### Bước 1: Tải mã nguồn

Mở Terminal (Khuyên dùng **PowerShell** hoặc **Git Bash**) và chạy:

```bash
git clone <LINK_GIT_CUA_NHOM_MINH_TAI_DAY>
cd DuAnTotNghiepDemo
```
````

### Bước 2: Cấu hình môi trường (.env)

Tạo file `.env` từ file mẫu:

```bash
cp .env.example .env

```

_(Nếu lệnh trên lỗi, hãy copy file `.env.example`, dán lại và đổi tên file copy thành `.env` thủ công)._

**QUAN TRỌNG:** Mở file `.env` vừa tạo và sửa thông tin Database như sau:

```ini
DB_CONNECTION=mysql
DB_HOST=mysql          <-- BẮT BUỘC là 'mysql' (Không dùng 127.0.0.1)
DB_PORT=3306
DB_DATABASE=duantotnghiep
DB_USERNAME=sail
DB_PASSWORD=password

```

### Bước 3: Cài đặt thư viện (Vendor)

Vì thư mục `vendor` không có trên Git, bạn cần cài đặt nó để có lệnh Sail.

- **Trường hợp A: Máy bạn có sẵn PHP & Composer**

```bash
composer install

```

- **Trường hợp B: Máy bạn KHÔNG CÓ PHP (Chỉ có Docker)**
  Chạy lệnh sau để Docker tự tải thư viện về (Copy y nguyên):

```bash
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php83-composer:latest \
    composer install --ignore-platform-reqs

```

### Bước 4: Khởi động Server

Đảm bảo Docker Desktop đang chạy (icon màu xanh), sau đó gõ:

```bash
./vendor/bin/sail up -d

```

_(Lần đầu chạy sẽ mất 5-10 phút để tải. Các lần sau chỉ mất vài giây)._

### Bước 5: Khởi tạo dữ liệu

Khi server đã chạy xong, chạy lần lượt các lệnh sau:

```bash
# 1. Tạo Key bảo mật
./vendor/bin/sail artisan key:generate

# 2. Tạo bảng Database
./vendor/bin/sail artisan migrate

# 3. (Tùy chọn) Tạo dữ liệu mẫu
./vendor/bin/sail artisan db:seed

```

### 🎉 Xong! Truy cập Web tại:

- **Trang chủ:** [http://localhost](https://www.google.com/search?q=http://localhost)
- **Database (Nếu dùng phần mềm quản lý):** Host: `localhost` | User: `sail` | Pass: `password` | Port: `3306`.

---

## ⚡ Phần 3: Bảng Các Lệnh Hay Dùng (Cheat Sheet)

Chúng ta chạy qua Docker nên **KHÔNG dùng** `php artisan` trực tiếp. Hãy dùng bảng tra cứu sau:

| Hành động           | Câu lệnh CŨ (XAMPP/Herd)      | Câu lệnh MỚI (Docker/Sail)                      |
| ------------------- | ----------------------------- | ----------------------------------------------- |
| **Bật Server**      | `php artisan serve`           | `./vendor/bin/sail up -d`                       |
| **Tắt Server**      | `Ctrl + C`                    | `./vendor/bin/sail down`                        |
| **Chạy Migration**  | `php artisan migrate`         | `./vendor/bin/sail artisan migrate`             |
| **Tạo Controller**  | `php artisan make:controller` | `./vendor/bin/sail artisan make:controller ...` |
| **Cài thư viện**    | `composer require ...`        | `./vendor/bin/sail composer require ...`        |
| **Kiểm tra status** | N/A                           | `./vendor/bin/sail ps`                          |
| **Xem logs lỗi**    | Xem trên màn hình             | `./vendor/bin/sail logs -f`                     |

---

## ❓ Phần 4: Khắc Phục Lỗi Thường Gặp

**1. Lỗi cổng: "Ports are not available... 3306"**

- **Nguyên nhân:** XAMPP, MySQL hoặc Herd đang chạy ngầm và chiếm cổng.
- **Cách sửa:** Tắt hoàn toàn các phần mềm đó. Hoặc mở Task Manager tìm tiến trình `mysqld` và tắt nó đi (End Task).

**2. Lỗi: "'.' is not recognized..."**

- **Nguyên nhân:** Bạn đang dùng **CMD** của Windows.
- **Cách sửa:** Hãy chuyển sang dùng **PowerShell** hoặc **Git Bash**.

**3. Lỗi Database: "Connection refused"**

- **Nguyên nhân:** File `.env` sai cấu hình.
- **Cách sửa:** Kiểm tra lại dòng `DB_HOST=mysql` trong file `.env`. Đảm bảo container đang chạy (`./vendor/bin/sail ps`).

```

```
