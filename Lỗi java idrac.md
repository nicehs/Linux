## Các lỗi khi chạy java với idrac

### 1. Untrusted website
- Chạy jre1.8.0_351/bin/ControlPanel
- Thêm web vừa chạy vào Securiy > Exception site list
- Comment dòng: jdk.jar.disabledAlgorithms trong file jre1.8.0_351/lib/security/java.security

### 2. Lỗi connection failed
- Xoá RC4 tại dòng: jdk.tls.disabledAlgorithms trong file jre1.8.0_351/lib/security/java.security
[How to fix Java for iDRAC6 virtual console - YouTube](https://www.youtube.com/watch?v=drhSo9Xl9M0)

### 3. Lỗi login failed, ... slow network ...
Mỗi session có sẽ có 1 file java chạy khác nhau. Nên hãy chắc chắn rằng java đã down từ idrac là mới nhất
[Login Failed Possibly Due To Slow Network Connection \| Dell Remote Access Console Home Lab Server - YouTube](https://www.youtube.com/watch?v=EjTzIokJPcI)
