# 🧭 Một sơ đồ hạ tầng điển hình gồm gì?

## 🎨 Cách đọc sơ đồ Infrastructure:

1. **Luồng từ trên xuống** - Request từ client → backend → database
2. **Mũi tên** - Hướng flow của dữ liệu
3. **Hình chữ nhật** - Các service/component
4. **Đường nét đứt** - Kết nối không đồng bộ (async)

## ✅ Ví dụ sơ đồ:

```
🌐 Internet
   ↓
📡 CloudFront (CDN)
   ↓
🛡️ API Gateway
   ↓
⚖️ Load Balancer (ALB)
   ↓
🔄 Nginx (Reverse Proxy)
   ↓
🐳 Application Servers (Node.js, Laravel, etc.)
   ↓          ↓
🚀 Redis     💾 Database (RDS/MySQL)
   ↓
📬 Queue (SQS, RabbitMQ)
   ↓
📊 Background Workers
```

## 🔍 Giải thích chi tiết từng tầng:

### 🌐 **1. CDN Layer (CloudFront)**
- **Mục đích**: Cache static files gần user
- **Ví dụ**: CSS, JS, images, videos
- **Lợi ích**: Giảm latency, bandwidth

### 🛡️ **2. API Gateway**
- **Mục đích**: Rate limiting, authentication, routing
- **Ví dụ**: AWS API Gateway, Kong
- **Chức năng**: Validate request, transform data

### ⚖️ **3. Load Balancer**
- **Mục đích**: Phân tán traffic đến nhiều server
- **Thuật toán**: Round Robin, Least Connections
- **Health Check**: Tự động loại bỏ server lỗi

### 🔄 **4. Reverse Proxy (Nginx)**
- **Mục đích**: SSL termination, compression, caching
- **Cấu hình**: Route `/api/*` → Backend, `/static/*` → S3
- **Security**: Hide backend architecture

### 🐳 **5. Application Layer**
- **Horizontal scaling**: Nhiều instance cùng chạy
- **Stateless**: Không lưu session trong memory
- **Container**: Docker để đảm bảo consistency

### 💾 **6. Database Layer**
- **Primary/Replica**: Read/Write splitting
- **Connection Pool**: Tối ưu database connections
- **Backup**: Automated backup và recovery

### 🚀 **7. Cache Layer (Redis)**
- **Session Storage**: User sessions
- **Query Cache**: Kết quả database phức tạp
- **Rate Limiting**: Counter API calls

### 📬 **8. Queue System**
- **Async Processing**: Email, notifications
- **Retry Logic**: Xử lý lỗi tự động
- **Dead Letter Queue**: Lưu message lỗi

## 🧩 Các thành phần cần chú ý:

| Thành phần                 | Mô tả                                | Ví dụ thực tế         | Vai trò với Backend Code |
| -------------------------- | ------------------------------------ | --------------------- | ----------------------- |
| Load Balancer              | Phân chia tải đến các server backend | AWS ALB               | App cần stateless design |
| Web server / Reverse Proxy | Xử lý request, routing               | Nginx                 | Config routing rules |
| Application Server         | Server chạy app                      | Docker container, ECS | Nơi code chạy |
| Database                   | Lưu trữ dữ liệu                      | MySQL, PostgreSQL     | Connection pool config |
| Cache                      | Truy xuất nhanh                      | Redis, Memcached      | Cache strategy trong code |
| Queue                      | Tác vụ xử lý bất đồng bộ             | SQS, RabbitMQ         | Job processing logic |
| Storage                    | Lưu file                             | AWS S3                | File upload/download API |
| CDN                        | Tăng tốc truyền tải                  | CloudFront            | Static assets serving |

## 🚀 Ví dụ thực tế - E-commerce System:

```
📱 Mobile App / 🌐 Website
   ↓
🛡️ API Gateway (Rate limit: 1000 req/min)
   ↓
⚖️ Load Balancer (3 instances)
   ↓
🐳 Node.js Apps (Port 3000)
   ↓               ↓
🚀 Redis           💾 PostgreSQL
(Sessions,         (Users, Products,
 Cart data)         Orders)
   ↓
📬 SQS Queue
   ↓
📧 Email Service (SendGrid)
📊 Analytics Service
```

**Khi user đặt hàng:**
1. **Mobile App** gửi POST `/api/orders`
2. **API Gateway** kiểm tra rate limit
3. **Load Balancer** chọn server có ít load nhất
4. **Node.js App** xử lý business logic
5. **PostgreSQL** lưu order data
6. **Redis** clear cart cache
7. **SQS** gửi message "send confirmation email"
8. **Background Worker** xử lý email bất đồng bộ
