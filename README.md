# Key Server - Văn Khánh VIP

Server quản lý key & thiết bị, deploy trên Render.com.

## Deploy lên Render

1. Push repo này lên GitHub
2. Vào [render.com](https://render.com) → New → Web Service → kết nối repo
3. Render tự đọc file `render.yaml` và cấu hình tự động
4. Deploy xong sẽ có link dạng: `https://key-server.onrender.com`

## Đường link check key/device (tích hợp vào app/tool)

```
GET/POST  https://your-server.onrender.com/api/check_key
```

**Tham số:**
| Tham số | Bắt buộc | Mô tả |
|---------|----------|-------|
| `key` | ✅ | Mã key cần kiểm tra |
| `device_id` | ✅ | ID thiết bị (Machine ID) |

**Ví dụ GET:**
```
https://your-server.onrender.com/api/check_key?key=VIP-ABC-123&device_id=DEVICE-XYZ-456
```

**Ví dụ POST (form-data):**
```
POST https://your-server.onrender.com/api/check_key
key=VIP-ABC-123&device_id=DEVICE-XYZ-456
```

**Kết quả trả về (JSON):**
```json
// Key hợp lệ, thiết bị được đăng ký
{ "status": "success", "message": "Key hop le", "time_left": "7 ngày" }

// Key hợp lệ, thêm thiết bị mới thành công
{ "status": "success", "message": "Them thiet bi thanh cong", "time_left": "7 ngày" }

// Key hết hạn trên thiết bị này
{ "status": "expired", "message": "Key tren thiet bi nay da het han!" }

// Quá số thiết bị cho phép
{ "status": "device_limit", "message": "Qua so thiet bi cho phep (1)" }

// Key không tồn tại
{ "status": "invalid", "message": "Key khong ton tai" }

// Thiếu tham số
{ "status": "error", "message": "Thieu tham so" }
```

## Trang quản trị Admin

- **Trang chủ / Check key:** `/`
- **Admin panel:** Đăng nhập từ trang chủ, tài khoản mặc định: `vkhanh` / `1`
- **Nhận key free:** `/nhan-key-free`
- **Check IP key:** `/check-ip-key`
- **Đăng ký thiết bị:** `/dang-ky-thiet-bi`

## Stack

- Python + Flask 3.1.1
- Gunicorn (production server)
- Database: JSON file (`database_keys.json`) — tự động tạo, dữ liệu lưu bền vững

## Nhạc nền

Đặt file `nhac.mp3`, `nhac2.mp3`, `nhac3.mp3` cùng thư mục với `server.py`.
