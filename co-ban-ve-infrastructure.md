# 📌 Hạ tầng (Infrastructure) là gì?

> Là toàn bộ các tài nguyên phần cứng và dịch vụ nền tảng (cloud, network, database, CI/CD, monitoring...) mà phần mềm của bạn chạy trên đó.

## 🏗️ Tại sao cần hiểu về Infrastructure?

Là **Junior Backend Developer**, bạn cần hiểu Infrastructure để:

1. **Debug hiệu quả** - Biết lỗi có thể xuất phát từ đâu (network, database, cache...)
2. **Tối ưu performance** - Hiểu cách cache, database connection pool hoạt động
3. **Giao tiếp với DevOps** - Có ngôn ngữ chung khi báo cáo vấn đề
4. **Phát triển code phù hợp** - Viết code tận dụng được hạ tầng có sẵn

## 📋 Phân loại Infrastructure theo tầng:

### 🌐 **Tầng 1: Compute (Tính toán)**
- **EC2 Instances** - Máy ảo trên AWS
- **Docker Containers** - Đóng gói ứng dụng
- **Kubernetes Pods** - Orchestration containers
- **Lambda Functions** - Serverless computing

### 💾 **Tầng 2: Storage (Lưu trữ)**
- **Database**: RDS (MySQL, PostgreSQL), DynamoDB
- **File Storage**: S3, EBS Volumes
- **Cache**: Redis, Memcached
- **CDN**: CloudFront, CloudFlare

### 🌍 **Tầng 3: Network (Mạng)**
- **Load Balancer**: ALB, NLB
- **API Gateway**: Route và manage APIs
- **VPC**: Virtual Private Cloud
- **Security Groups**: Firewall rules

### 🔧 **Tầng 4: Services (Dịch vụ)**
- **Queue**: SQS, RabbitMQ
- **Message Broker**: Kafka, SNS
- **Monitoring**: CloudWatch, Prometheus
- **CI/CD**: Jenkins, GitHub Actions

## 🎯 Ví dụ thực tế:

**Khi bạn viết API `/api/users`:**
```javascript
// Code của bạn
app.get('/api/users', async (req, res) => {
  const users = await User.findAll();
  res.json(users);
});
```

**Infrastructure đằng sau:**
- **API Gateway** nhận request
- **Load Balancer** forward đến server
- **EC2/Container** chạy Node.js app
- **RDS** truy vấn database
- **Redis** cache kết quả (nếu có)
