# Quick Start Guide

## 🚀 เริ่มใช้งานด่วน

### วิธีที่ 1: ใช้ Docker Compose (แนะนำ)

```bash
# 1. เปิด Docker Desktop ก่อน

# 2. Build และรัน
docker compose up --build -d

# 3. เข้าใช้งาน
# Frontend: http://localhost:8080
# Backend API: http://localhost:3000/api/demo

# 4. ดู logs
docker compose logs -f

# 5. หยุดการทำงาน
docker compose down
```

### วิธีที่ 2: รันแบบ Local Development

```bash
# Terminal 1 - Backend
cd backend
npm install
npm start
# รันที่ http://localhost:3000

# Terminal 2 - Frontend  
cd frontend
npm install
npm run dev
# รันที่ http://localhost:9000
```

## 📋 คำสั่งที่ใช้บ่อย

```bash
# ดูสถานะ containers
docker compose ps

# ดู logs แบบ real-time
docker compose logs -f backend
docker compose logs -f frontend

# Restart services
docker compose restart

# หยุดและลบทุกอย่าง (รวม volumes)
docker compose down -v

# Build ใหม่โดยไม่ใช้ cache
docker compose build --no-cache
```

## 🧪 ทดสอบ API

```bash
# ทดสอบ backend API
curl http://localhost:3000/api/demo

# หรือเปิดใน browser
http://localhost:3000/api/demo
```

## 📁 โครงสร้างสำคัญ

```
my-quasar-express-app/
├── frontend/          # Quasar + Vue 3
│   ├── src/pages/IndexPage.vue
│   └── Dockerfile
├── backend/           # Express API
│   ├── server.js
│   └── Dockerfile
└── docker-compose.yml
```

## ⚠️ Troubleshooting

### Docker ไม่ทำงาน
- ✅ ตรวจสอบ Docker Desktop เปิดอยู่หรือไม่
- ✅ รัน `docker ps` เพื่อเช็คว่า Docker daemon ทำงาน

### Frontend ไม่เชื่อมต่อ Backend
- ✅ ตรวจสอบ backend รันอยู่: `docker compose ps`
- ✅ ดู logs: `docker compose logs backend`

### Port ถูกใช้แล้ว
- แก้ไข port ใน `docker-compose.yml`:
  ```yaml
  ports:
    - "8081:80"  # เปลี่ยนจาก 8080
  ```
