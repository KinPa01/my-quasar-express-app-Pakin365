# Full-Stack Quasar + Express Application

โปรเจกต์ Full-Stack ที่ใช้ Quasar (Vue 3) สำหรับ Frontend และ Express สำหรับ Backend พร้อม Docker และ Docker Compose

## 📁 โครงสร้างโปรเจกต์

```
my-quasar-express-app/
├── frontend/              # Quasar SPA Frontend
│   ├── src/
│   │   ├── pages/        # หน้าต่างๆ
│   │   ├── layouts/      # Layout components
│   │   ├── router/       # Vue Router config
│   │   └── css/          # Global styles
│   ├── Dockerfile        # Frontend Docker image
│   ├── .dockerignore
│   ├── package.json
│   └── quasar.config.js
├── backend/              # Express API Backend
│   ├── server.js         # Main server file
│   ├── logs/             # Access logs (persisted via volume)
│   ├── Dockerfile        # Backend Docker image
│   ├── .dockerignore
│   └── package.json
├── docker-compose.yml    # Multi-service orchestration
├── .gitignore
└── README.md
```

## 🚀 การเริ่มต้นใช้งาน

### Prerequisites

- Node.js 16+ (สำหรับ local development)
- Docker และ Docker Compose (สำหรับ containerized deployment)
- Git

### Local Development

#### Backend

```bash
cd backend
npm install
npm start
# Server รันที่ http://localhost:3000
```

ทดสอบ API:
```bash
curl http://localhost:3000/api/demo
```

#### Frontend

```bash
cd frontend
npm install
npm run dev
# App รันที่ http://localhost:9000
```

**หมายเหตุ:** ตรวจสอบว่า backend รันอยู่ที่ port 3000 และ `.env` ตั้งค่า `VITE_API_URL=http://localhost:3000`

## 🐳 Docker Deployment

### Build และ Run ด้วย Docker Compose

```bash
# Build และรัน services ทั้งหมด
docker compose up --build -d

# ดู logs
docker compose logs -f

# ดู logs เฉพาะ service
docker compose logs -f backend
docker compose logs -f frontend

# ตรวจสอบสถานะ
docker compose ps

# หยุด services
docker compose down

# หยุดและลบ volumes
docker compose down -v
```

### เข้าถึง Application

- **Frontend**: http://localhost:8080
- **Backend API**: http://localhost:3000/api/demo

### Build Images แยก (Optional)

```bash
# Backend
cd backend
docker build -t my-express-backend:latest .

# Frontend
cd frontend
docker build -t my-quasar-frontend:latest .
```

## 🔧 Configuration

### Environment Variables

#### Backend (.env)
```env
PORT=3000
```

#### Frontend (.env)
```env
VITE_API_URL=http://localhost:3000
```

**หมายเหตุ:** ใน Docker Compose, `VITE_API_URL` จะถูก override เป็น `http://backend:3000` เพื่อใช้ internal network

## 📡 API Endpoints

### GET /api/demo

Returns Git and Docker information

**Response:**
```json
{
  "git": {
    "title": "Advanced Git Workflow",
    "detail": "ใช้ branch protection บน GitHub, code review ใน PR, และ squash merge เพื่อ history สะอาด"
  },
  "docker": {
    "title": "Advanced Docker",
    "detail": "ใช้ multi-stage build, healthcheck ใน Dockerfile, และ orchestration ด้วย Compose/Swarm"
  }
}
```

## 🔄 Git Workflow

### Feature Development

```bash
# 1. สร้าง feature branch
git checkout -b feature/your-feature-name

# 2. ทำการแก้ไขและ commit (ใช้ Conventional Commits)
git add .
git commit -m "feat: add new feature description"

# 3. Push branch
git push origin feature/your-feature-name

# 4. เปิด Pull Request บน GitHub
# 5. รอ code review
# 6. Merge (ใช้ squash merge)
# 7. ลบ branch
```

### Conventional Commits

- `feat:` - Feature ใหม่
- `fix:` - Bug fix
- `docs:` - เอกสาร
- `style:` - Formatting
- `refactor:` - Code refactoring
- `test:` - Tests
- `chore:` - Maintenance

## 🏗️ Docker Architecture

### Multi-stage Builds

ทั้ง Frontend และ Backend ใช้ multi-stage builds:

- **Stage 1 (Builder)**: Install dependencies และ build
- **Stage 2 (Production)**: Copy built files, ลดขนาด image

### Healthcheck

Backend มี healthcheck ที่ตรวจสอบ `/api/demo` endpoint ทุก 30 วินาที

### Networks

Services สื่อสารกันผ่าน `app-network` (bridge network)

### Volumes

- `./backend/logs:/app/logs` - Persist access logs

## 🧪 Testing

### Manual Testing

1. เปิด http://localhost:8080
2. ตรวจสอบว่าแสดง 3 cards:
   - Git Workflow
   - Docker Concepts
   - Data from Backend API
3. คลิก "Refresh Data" เพื่อทดสอบ API call
4. ตรวจสอบ browser console ไม่มี errors

### Log Verification

```bash
# ตรวจสอบ logs ถูกเขียน
cat backend/logs/access.log

# หรือใน Docker
docker compose exec backend cat logs/access.log
```

## 🛠️ Troubleshooting

### Frontend ไม่เชื่อมต่อ Backend

1. ตรวจสอบ backend รันอยู่: `docker compose ps`
2. ตรวจสอบ healthcheck: `docker compose ps` (ต้องเป็น healthy)
3. ตรวจสอบ network: `docker compose exec frontend ping backend`
4. ดู logs: `docker compose logs backend`

### Port Conflicts

ถ้า port 3000 หรือ 8080 ถูกใช้แล้ว, แก้ไขใน `docker-compose.yml`:

```yaml
ports:
  - "8081:80"  # เปลี่ยนจาก 8080
```

## 📚 Technologies Used

- **Frontend**: Quasar Framework 2.x, Vue 3, Axios, Vite
- **Backend**: Express 5.x, CORS, dotenv
- **Containerization**: Docker, Docker Compose
- **Web Server**: Nginx (for frontend production)

## 📝 License

ISC

## 👥 Contributing

1. Fork the project
2. Create feature branch
3. Commit changes (use Conventional Commits)
4. Push to branch
5. Open Pull Request

---

**Happy Coding! 🎉**
