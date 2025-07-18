# 🧪 Thực hành đọc hiểu Infrastructure Design

## 🎯 Mục tiêu bài tập

Sau khi hoàn thành các bài tập này, bạn sẽ có khả năng:
- Phân tích một sơ đồ infrastructure hoàn chỉnh
- Xác định luồng dữ liệu và các thành phần quan trọng
- Đặt câu hỏi đúng để làm rõ thiết kế
- Mapping infrastructure với code implementation

## 📋 Bài tập 1: Phân tích sơ đồ cơ bản

### **Đề bài**: Cho sơ đồ sau của một blog system

```
👤 Users (Web Browser)
↓
🌐 CloudFront CDN
↓
⚖️ Application Load Balancer
↓
🐳 EC2 Instances (3 servers)
- Node.js + Express
- Port 3000
↓
💾 RDS MySQL (Primary)
↓
💾 RDS MySQL (Read Replica)
```

### **Câu hỏi:**
1. Luồng đi của request từ user đến database là gì?
2. Tại sao cần CloudFront CDN?
3. Load Balancer sử dụng thuật toán nào phù hợp?
4. 3 EC2 instances có cần stateless không? Vì sao?
5. Khi nào nên dùng Read Replica?

### **Đáp án:**
<details>
<summary>Xem đáp án</summary>

1. **Luồng request**: User → CloudFront → ALB → EC2 (Node.js) → RDS Primary/Replica
2. **CloudFront CDN**: Cache static assets (CSS, JS, images), giảm latency
3. **Load Balancer**: Round-robin hoặc Least Connections
4. **Stateless**: Có, để có thể scale horizontal và failover
5. **Read Replica**: Khi có nhiều SELECT queries, đọc comments, analytics

</details>

## 📋 Bài tập 2: E-commerce System

### **Đề bài**: Phân tích sơ đồ e-commerce

```
📱 Mobile App + 🌐 Web App
↓
🛡️ API Gateway
- Rate limiting: 1000 req/min
- JWT Authentication
↓
⚖️ Load Balancer
↓
🐳 Application Servers (5 instances)
- Node.js + Express
- Docker containers
↓                    ↓
🚀 Redis Cluster    💾 PostgreSQL
- Session storage   - Product catalog
- Shopping cart     - User accounts
- Product cache     - Order history
↓
📬 Amazon SQS
↓
📧 Email Service (SendGrid)
📦 Inventory Service
💳 Payment Service (Stripe)
```

### **Câu hỏi:**
1. Khi user đặt hàng (`POST /api/orders`), luồng xử lý như thế nào?
2. Tại sao cần Redis Cluster thay vì single Redis instance?
3. SQS queue xử lý những tác vụ nào?
4. Nếu Payment Service down, hệ thống xử lý như thế nào?
5. Code cần implement những pattern nào?

### **Đáp án:**
<details>
<summary>Xem đáp án</summary>

1. **Luồng đặt hàng**:
   - API Gateway validate JWT
   - Load Balancer route to healthy instance
   - App server validate order data
   - PostgreSQL save order
   - Redis clear cart
   - SQS send async tasks
   - Background services process email/inventory/payment

2. **Redis Cluster**: High availability, data sharding, handle more connections

3. **SQS tasks**: Send confirmation email, update inventory, process payment, analytics

4. **Payment Service down**: Circuit breaker pattern, retry mechanism, fallback to manual processing

5. **Code patterns**: 
   - Connection pooling (PostgreSQL)
   - Cache-aside pattern (Redis)
   - Producer-consumer pattern (SQS)
   - Circuit breaker pattern (external services)

</details>

## 📋 Bài tập 3: Microservices Architecture

### **Đề bài**: Phân tích hệ thống microservices

```
🌐 Frontend (React SPA)
↓
🛡️ API Gateway (Kong)
↓
🔄 Service Mesh (Istio)
↓
┌─────────────────────────────────┐
│ 👤 User Service (Node.js)        │
│ 🛍️ Product Service (Java)        │
│ 🛒 Cart Service (Python)         │
│ 📦 Order Service (Go)           │
│ 💳 Payment Service (Node.js)    │
│ 📧 Notification Service (Python)│
└─────────────────────────────────┘
↓
📬 Message Bus (Apache Kafka)
↓
💾 Database per Service
- User DB (PostgreSQL)
- Product DB (MongoDB)
- Cart DB (Redis)
- Order DB (PostgreSQL)
- Payment DB (PostgreSQL)
```

### **Câu hỏi:**
1. Tại sao mỗi service có database riêng?
2. Service Mesh (Istio) giải quyết vấn đề gì?
3. Kafka message bus có những advantages nào?
4. Khi có transaction spanning multiple services, xử lý như thế nao?
5. Monitoring và logging cho microservices như thế nào?

### **Đáp án:**
<details>
<summary>Xem đáp án</summary>

1. **Database per service**: 
   - Data isolation
   - Technology diversity
   - Independent scaling
   - Fault isolation

2. **Service Mesh benefits**:
   - Traffic management
   - Security (mTLS)
   - Observability
   - Load balancing

3. **Kafka advantages**:
   - High throughput
   - Durability
   - Replay capability
   - Decoupling services

4. **Distributed transactions**:
   - Saga pattern
   - Event sourcing
   - Compensation transactions
   - Eventual consistency

5. **Monitoring microservices**:
   - Distributed tracing (Jaeger)
   - Centralized logging (ELK)
   - Service mesh metrics
   - Health checks

</details>

## 📋 Bài tập 4: Thực hành tìm vấn đề

### **Đề bài**: Xác định vấn đề trong sơ đồ

```
👤 Users
↓
🌐 Single Web Server
- Apache + PHP
- 2 CPU, 4GB RAM
↓
💾 MySQL Database
- Single instance
- No backup
- No indexing
↓
📁 File Storage
- Local disk
- No redundancy
```

### **Câu hỏi:**
1. Những vấn đề gì có thể xảy ra với architecture này?
2. Đề xuất cải tiến cho từng component?
3. Ưu tiên cải tiến theo thứ tự nào?
4. Estimate cost và effort cho mỗi cải tiến?

### **Đáp án:**
<details>
<summary>Xem đáp án</summary>

1. **Vấn đề nghiêm trọng**:
   - Single point of failure
   - No scalability
   - No backup (data loss risk)
   - Poor performance (no indexing)
   - No monitoring

2. **Cải tiến đề xuất**:
   - **Web Server**: Load balancer + multiple instances
   - **Database**: Primary-replica setup, automated backup
   - **Storage**: Cloud storage (S3), CDN
   - **Monitoring**: Health checks, logging, alerting

3. **Ưu tiên cải tiến**:
   1. Database backup (critical)
   2. Database indexing (performance)
   3. Web server scaling (availability)
   4. Storage redundancy (reliability)
   5. Monitoring setup (observability)

4. **Cost estimation**:
   - Database backup: Low cost, high impact
   - Load balancer: Medium cost, high impact
   - Storage migration: Medium cost, medium impact
   - Monitoring: Low cost, high impact

</details>

## 📋 Bài tập 5: Thiết kế từ requirements

### **Đề bài**: Thiết kế infrastructure cho requirements

**Requirements:**
- Social media platform
- 100,000+ concurrent users
- Real-time chat and notifications
- Image/video upload and sharing
- Global user base
- High availability (99.9% uptime)
- GDPR compliance

### **Câu hỏi:**
1. Vẽ sơ đồ infrastructure tổng thể?
2. Chọn technology stack cho mỗi component?
3. Estimate resource requirements?
4. Identify potential bottlenecks?
5. Plan for disaster recovery?

### **Gợi ý đáp án:**
<details>
<summary>Xem gợi ý</summary>

1. **Infrastructure components**:
   - Multi-region deployment
   - CDN (CloudFront)
   - API Gateway
   - Load balancers
   - Auto-scaling groups
   - WebSocket servers (real-time)
   - Message queues
   - Database clusters
   - File storage (S3)
   - Caching layers

2. **Technology stack**:
   - **Frontend**: React/Vue.js
   - **API**: Node.js/Python
   - **Real-time**: Socket.io/WebSocket
   - **Database**: PostgreSQL + Redis
   - **Queue**: Kafka/SQS
   - **Storage**: S3 + CloudFront
   - **Monitoring**: Prometheus + Grafana

3. **Resource planning**:
   - 50-100 application servers
   - 10-20 database instances
   - 5-10 cache instances
   - Multi-AZ deployment
   - Auto-scaling policies

4. **Bottlenecks**:
   - Database write operations
   - WebSocket connection limits
   - File upload bandwidth
   - Cache invalidation

5. **Disaster recovery**:
   - Multi-region replication
   - Automated backups
   - Failover procedures
   - Data retention policies

</details>

## 🎯 Tự đánh giá

### **Level 1: Beginner**
- [ ] Có thể xác định các component cơ bản
- [ ] Hiểu được luồng request đơn giản
- [ ] Biết role của Load Balancer và Database

### **Level 2: Intermediate**
- [ ] Phân tích được microservices architecture
- [ ] Hiểu về caching strategies
- [ ] Xác định được scaling bottlenecks

### **Level 3: Advanced**
- [ ] Thiết kế được architecture từ requirements
- [ ] Estimate được resource và cost
- [ ] Plan được disaster recovery

## 💡 Tips thực hành

### 🔍 **Phân tích hệ thống có sẵn**
1. Tìm hiểu architecture của công ty hiện tại
2. Đọc blog posts về architecture của các công ty lớn
3. Tham gia các tech talk về infrastructure

### 🛠️ **Hands-on practice**
1. Setup local environment với Docker
2. Deploy simple app lên AWS/GCP
3. Implement caching và monitoring
4. Test scaling và failover scenarios

### 📚 **Tài liệu tham khảo**
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google Cloud Architecture Center](https://cloud.google.com/architecture)
- [Azure Architecture Center](https://docs.microsoft.com/en-us/azure/architecture/)
- [High Scalability](http://highscalability.com/)
