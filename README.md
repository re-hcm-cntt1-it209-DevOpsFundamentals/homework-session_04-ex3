# Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

## 1. Mục tiêu

- Tạo cặp khóa SSH sử dụng thuật toán Ed25519.
- Cấu hình xác thực SSH với GitHub.
- Liên kết repository cục bộ với repository trên GitHub bằng giao thức SSH.
- Push mã nguồn và lịch sử commit từ local repository lên GitHub.

## 2. Tạo SSH Key Ed25519

Kiểm tra thư mục SSH hiện có:

```bash
ls -la ~/.ssh

Máy đã có một số SSH key nên không ghi đè key id_ed25519 hiện có.

Tạo một SSH key Ed25519 riêng cho GitHub:

ssh-keygen -t ed25519 -C "n24dtcn062@student.ptithcm.edu.vn"

Key được lưu với tên riêng:

~/.ssh/id_ed25519_github
~/.ssh/id_ed25519_github.pub

Trong đó:

id_ed25519_github là private key và được giữ lại trên máy.
id_ed25519_github.pub là public key và được thêm vào GitHub.
3. Thêm Public Key vào GitHub

Lấy nội dung public key:

cat ~/.ssh/id_ed25519_github.pub

Sau đó thêm public key vào:

GitHub → Settings → SSH and GPG keys → New SSH key

Private key không được đưa lên GitHub.

4. Cấu hình SSH

Do sử dụng tên key riêng id_ed25519_github, cấu hình file:

~/.ssh/config

Nội dung:

Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github
    IdentitiesOnly yes

Cấu hình này giúp SSH sử dụng đúng private key id_ed25519_github khi kết nối tới GitHub.

5. Kiểm tra kết nối SSH

Thực hiện:

ssh -T git@github.com

Kết quả:

Hi Phat-Nguyen06! You've successfully authenticated, but GitHub does not provide shell access

Kết quả trên cho thấy xác thực SSH với tài khoản GitHub đã thành công.

6. Kết nối Local Repository với GitHub

Đổi tên branch mặc định thành main:

git branch -M main

Thêm GitHub repository làm remote với tên origin:

git remote add origin git@github.com:re-hcm-cntt1-it209-DevOpsFundamentals/homework-session_04-ex3.git

Kiểm tra remote:

git remote -v

Kết quả:

origin  git@github.com:re-hcm-cntt1-it209-DevOpsFundamentals/homework-session_04-ex3.git (fetch)
origin  git@github.com:re-hcm-cntt1-it209-DevOpsFundamentals/homework-session_04-ex3.git (push)

Remote origin sử dụng giao thức SSH với dạng:

git@github.com:username/repository.git

7. Push dự án lên GitHub

Push branch main lên remote:

git push -u origin main

Lệnh trên đẩy các commit từ local branch main lên branch main trên GitHub.

Tùy chọn -u thiết lập quan hệ tracking giữa:

local main
    ↕
origin/main

Sau khi thiết lập tracking, những lần push tiếp theo có thể sử dụng:

git push

8. Kết quả

Đã hoàn thành các yêu cầu:

 Tạo SSH key sử dụng Ed25519.
 Thêm public key vào GitHub.
 Cấu hình SSH sử dụng key riêng cho GitHub.
 Xác thực thành công bằng ssh -T git@github.com.
 Cấu hình remote repository bằng giao thức SSH.
 Push thành công branch main lên GitHub.
 Không đưa private key lên repository.