# FIT4012 Lab 7 - Báo cáo 1 trang: SHA-256

## 1. Mục tiêu / Objective

Mô tả ngắn gọn mục tiêu của bài thực hành: phân tích thuật toán SHA-256, chạy chương trình băm chuỗi, kiểm tra toàn vẹn file, băm mật khẩu và cải tiến bằng salt.

Phân tích thuật toán SHA-256, chạy chương trình băm chuỗi, kiểm tra toàn vẹn file, mô phỏng lưu mật khẩu bằng hash và cải tiến bằng salt. Qua đó hiểu được tính một chiều, tính nhạy cảm với thay đổi và ứng dụng của SHA-256 trong bảo mật dữ liệu.

## 2. Cách làm / Approach

- Biên dịch chương trình bằng `make`, tạo 4 file thực thi: `sha256`, `file_integrity`, `password_hash`, `salted_password_hash`.
- Dùng `./sha256 --self-test` kiểm tra known answer test vector.
- Dùng `./sha256 --hash-string` để băm các chuỗi "abc", "FIT4012", "FIT4012!".
- Tạo file `sample.txt`, tính hash, dùng `file_integrity` kiểm tra trước và sau khi sửa nội dung.
- Dùng `password_hash` để đăng ký và xác thực mật khẩu đúng/sai.
- Dùng `salted_password_hash` để tạo hai bản ghi salt:hash từ cùng mật khẩu, chứng minh hash khác nhau.

## 3. Kết quả / Result
- Hash của chuỗi `abc`:  
  `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad`

- Hash của file mẫu `sample.txt` trước khi sửa:  
  `e5a3e3e3c4e9f8f6d2e1c0b3a2f5e8d4c6b7a1f2e3d4c5b6a7f8e9d0c1b2a3f4` (ví dụ)

- Kết quả kiểm tra file sau khi sửa nội dung:  
  `[FAIL] Hash mismatch. File may have been tampered.`

- Kết quả đăng nhập với mật khẩu đúng:  
  `[OK] Password verified.`

- Kết quả đăng nhập với mật khẩu sai:  
  `[FAIL] Invalid password.`

- Hai bản ghi `salt:hash` của cùng một mật khẩu:  
  **Không giống nhau**. Ví dụ:  
  `a1b2c3:5e8d9f...`  
  `x9y8z7:3a4b6c...`

## 4. Kết luận / Conclusion

Nêu ngắn gọn điều rút ra:

- SHA-256 giúp phát hiện thay đổi dữ liệu vì chỉ cần sửa 1 ký tự trong file/chuỗi, hash thay đổi hoàn toàn, dễ dàng so sánh phát hiện giả mạo.
- Cần **salt** khi lưu hash mật khẩu để tránh tấn công bằng bảng màu (rainbow table) và tránh trường hợp hai người có cùng mật khẩu ra cùng hash.
- SHA-256 trong lab **chưa an toàn cho hệ thống thật** vì:
  - Tốc độ băm quá nhanh (dễ brute force).
  - Thiếu cơ chế làm chậm (key stretching) như bcrypt, Argon2.
  - Chưa xử lý được các tấn công side-channel trong thực tế.