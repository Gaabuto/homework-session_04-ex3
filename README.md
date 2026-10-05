# Session 04 - Bài 3: Cấu hình xác thực SSH và đẩy dự án lên GitHub

## 1. Mục tiêu

Tạo cặp khóa SSH (Ed25519), đăng ký khóa công khai với GitHub, sau đó khởi tạo một repository Git cục bộ và đẩy (push) lên GitHub thông qua kết nối SSH.

## 2. Các bước thực hiện

### Bước 1: Tạo cặp khóa SSH

```bash
ssh-keygen -t ed25519 -C "gaabu"
```

- Nhấn **Enter** để giữ đường dẫn mặc định (`~/.ssh/id_ed25519`).
- Passphrase là **tùy chọn** (có thể để trống hoặc đặt để tăng bảo mật).

### Bước 2: Kiểm tra các file khóa được tạo

```bash
ls ~/.ssh
```

| File | Loại | Ghi chú |
|------|------|---------|
| `id_ed25519` | Khóa riêng tư (private key) | **Tuyệt đối không chia sẻ** với bất kỳ ai |
| `id_ed25519.pub` | Khóa công khai (public key) | Dùng để đăng ký trên GitHub |

### Bước 3: Sao chép khóa công khai

```bash
cat ~/.ssh/id_ed25519.pub
```

Sao chép toàn bộ nội dung được in ra (bắt đầu bằng `ssh-ed25519 ...`).

### Bước 4: Thêm khóa công khai vào GitHub

Trên GitHub: **Settings** > **SSH and GPG keys** > **New SSH key**
- **Title:** đặt tên dễ nhận biết (ví dụ: tên máy tính).
- **Key type:** Authentication Key.
- **Key:** dán nội dung khóa công khai vừa sao chép, sau đó nhấn **Add SSH key**.

### Bước 5: Kiểm tra kết nối SSH tới GitHub

```bash
ssh -T git@github.com
```

Lần đầu kết nối, gõ `yes` để xác nhận fingerprint của GitHub.

### Bước 6: Khởi tạo repository và cấu hình thông tin người dùng

```bash
git init -b main
git config --local user.name "<tên của bạn>"
git config --local user.email "<email của bạn>"
```

### Bước 7: Liên kết với repository trên GitHub

```bash
git remote add origin git@github.com:Gaabuto/homework-session_04-ex3.git
git remote -v
```

### Bước 8: Commit và push lên GitHub

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

## 3. Kết quả

Kết quả kiểm tra kết nối SSH (`ssh -T git@github.com`), danh sách remote (`git remote -v`) và fingerprint của khóa công khai (`ssh-keygen -lf ~/.ssh/id_ed25519.pub`):

```text
Hi Gaabuto! You've successfully authenticated, but GitHub does not provide shell access.
-----
origin	git@github.com:Gaabuto/homework-session_04-ex3.git (fetch)
origin	git@github.com:Gaabuto/homework-session_04-ex3.git (push)
-----
256 SHA256:l/ql8m1Xxo6Th6Lqf/Jb0vxO52aPwLrjTxTHIBFD3rY gaabu (ED25519)
```

### Ảnh chụp màn hình

<!-- Thêm ảnh chụp màn hình vào thư mục screenshots/ và cập nhật đường dẫn bên dưới -->

- Khóa SSH đã thêm trên GitHub: `![SSH key trên GitHub](screenshots/github-ssh-key.png)`
- Kết quả `ssh -T git@github.com`: `![Kiểm tra SSH](screenshots/ssh-test.png)`
- Kết quả `git push -u origin main`: `![Git push](screenshots/git-push.png)`

## 4. Repository

https://github.com/Gaabuto/homework-session_04-ex3

## 5. Ghi chú: Vì sao dùng SSH với Ed25519 thay vì HTTPS?

- **Không cần nhập mật khẩu/token mỗi lần push:** sau khi đăng ký khóa công khai, Git tự xác thực bằng khóa SSH trên máy.
- **Xác thực dựa trên khóa:** khóa riêng tư không bao giờ rời khỏi máy và không được gửi qua mạng, an toàn hơn so với việc lưu hay gõ token.
- **Ed25519** là thuật toán hiện đại: khóa ngắn, ký/xác minh nhanh và bảo mật cao hơn RSA truyền thống.
