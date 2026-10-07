# Bài 1: Khảo sát FHS và Phân quyền File/Folder nâng cao

## Yêu cầu bài toán
Triển khai một thư mục dùng chung cho ứng dụng web tại `/var/www/my-app`.
- Thư mục `public` chứa trang tĩnh: Chủ sở hữu có toàn quyền, nhóm sở hữu chỉ đọc/truy cập, Others không có quyền.
- Thư mục `logs` chứa nhật ký: Chủ sở hữu và nhóm sở hữu có toàn quyền đọc/ghi/truy cập, Others không có quyền.
- Nhóm sở hữu thư mục là `www-data`.

## Nhật ký các lệnh đã thực hiện

1. **Khởi tạo cấu trúc thư mục:**
```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

2. **Gán quyền cho thư mục `public` (Owner: rwx, Group: r-x, Others: --- => 750):**
```bash
sudo chmod 750 /var/www/my-app/public
```

3. **Gán quyền cho thư mục `logs` (Owner: rwx, Group: rwx, Others: --- => 770):**
```bash
sudo chmod 770 /var/www/my-app/logs
```

4. **Đổi nhóm sở hữu về `www-data` cho toàn bộ thư mục ứng dụng:**
```bash
sudo chown -R $USER:www-data /var/www/my-app
```

## Kết quả kiểm tra

Lệnh kiểm tra chi tiết cấu trúc thư mục và quyền hạn:
```bash
ls -la /var/www/my-app
```

**Đầu ra mong đợi (mô phỏng trên môi trường Linux):**
```text
total 16
drwxr-xr-x 4 user www-data 4096 Oct  7 10:20 .
drwxr-xr-x 3 root root     4096 Oct  7 10:20 ..
drwxrwx--- 2 user www-data 4096 Oct  7 10:20 logs
drwxr-x--- 2 user www-data 4096 Oct  7 10:20 public
```

*(Trong đó `user` là tài khoản non-root hiện tại đang sử dụng, kết quả cho thấy quyền của `logs` là `770` và `public` là `750` khớp với yêu cầu).*
