# Web Basic Assignment

Bài tập môn **Lập trình Web** sử dụng **WSL2, Docker Compose, Nginx, Node-RED, MariaDB, phpMyAdmin và Cloudflare Tunnel**.

Project triển khai 2 website với 2 domain khác nhau, xây dựng API bằng Node-RED, kết nối MariaDB và dùng JavaScript `fetch()` để gọi API từ website.

---

## Công nghệ sử dụng

- Windows 11 + WSL2 Ubuntu
- Docker / Docker Compose
- Nginx
- Node-RED
- MariaDB
- phpMyAdmin
- Cloudflare Tunnel
- HTML / CSS / JavaScript
- Git / GitHub

---

## Kiến trúc hệ thống

```text
Internet
   ↓
Cloudflare
   ↓
Cloudflare Tunnel
   ↓
Nginx
   ├── web1.trinhquyen.id.vn → Website 1
   ├── web2.trinhquyen.id.vn → Website 2
   └── api
         ↓
      Node-RED
         ↓
      MariaDB
```

## Domain

- Website 1: `https://web1.trinhquyen.id.vn`
- Website 2: `https://web2.trinhquyen.id.vn`
- API: `https://web1.trinhquyen.id.vn/api/sinhvien`

## Docker Compose Services

Project chạy các service:
- `nginx`
- `nodered`
- `mariadb`
- `phpmyadmin`
- `cloudflared`

Khởi động project:
```bash
docker compose up -d
```

Kiểm tra container:
```bash
docker ps
```

## Node-RED API

API sử dụng:
```text
HTTP In
↓
Query Students
↓
MariaDB
↓
Create Student Response
↓
HTTP Response
```

Endpoint:
`GET /api/sinhvien`

Ví dụ JSON trả về:
```json
{
  "ok": 1,
  "msg": "Thành công (Dữ liệu lấy trực tiếp từ Database)",
  "dssv": [
    {
      "name": "Quyền",
      "money": 123
    },
    {
      "name": "Vũ",
      "money": 456
    },
    {
      "name": "Độ",
      "money": 789
    }
  ]
}
```

## MariaDB

- Database: `webdb`
- Table: `students`

Node-RED query:
```js
msg.topic = "SELECT name, money FROM webdb.students";
return msg;
```

## JavaScript gọi API

Website 1 sử dụng `fetch()` để gọi API:
```js
const response = await fetch(
    "[https://web1.trinhquyen.id.vn/api/sinhvien](https://web1.trinhquyen.id.vn/api/sinhvien)"
);

const data = await response.json();
```

Dữ liệu sau đó được hiển thị lên bảng HTML.

## Nginx

Nginx được dùng để:
- Host 2 website với 2 domain khác nhau.
- Reverse proxy API từ `web1.trinhquyen.id.vn` tới Node-RED.

Luồng API:
```text
web1.trinhquyen.id.vn
↓
Nginx
↓
Node-RED
↓
MariaDB
```

## Cloudflare Tunnel

Cloudflare Tunnel giúp public hệ thống ra Internet mà không cần mở port router.

Các hostname:
```text
web1.trinhquyen.id.vn → nginx:80
web2.trinhquyen.id.vn → nginx:80
```

## Evidence

Thư mục `evidence/` chứa ảnh minh chứng quá trình thực hiện bài tập:
- `evidence/04-nginx/`: Minh chứng chạy 2 website trên 2 domain.
- `evidence/07-cloudflare/`: Cấu hình route trên Cloudflare Tunnel Dashboard.
- `evidence/08-api/`: Sơ đồ flow Node-RED, phản hồi JSON và kết quả website gọi API hiển thị dữ liệu sinh viên.

---
**Trịnh Xuân Quyền**
