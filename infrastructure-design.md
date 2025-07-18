# 🏗️ Infrastructure Components Examples với Mermaid Architecture

> Tài liệu này chứa các ví dụ về infrastructure components sử dụng **Mermaid Architecture Diagrams** để minh họa cho việc học tập **đọc hiểu Infrastructure Design** dành cho Junior Backend Developer.

---

## 📋 Mục lục

1. [Basic Infrastructure Components](#1-basic-infrastructure-components)
2. [Web Application Architecture](#2-web-application-architecture)
3. [Microservices Architecture](#3-microservices-architecture)
4. [E-commerce System Architecture](#4-e-commerce-system-architecture)
5. [CI/CD Pipeline Architecture](#5-cicd-pipeline-architecture)
6. [Cloud-Native Architecture](#6-cloud-native-architecture)
7. [High Availability Architecture](#7-high-availability-architecture)
8. [Data Pipeline Architecture](#8-data-pipeline-architecture)

---

## 1. Basic Infrastructure Components

### 🎯 **Mục tiêu**: Hiểu các component cơ bản nhất

```mermaid
architecture-beta
    group frontend(cloud)[Frontend Layer]
    group backend(cloud)[Backend Layer]
    group database(cloud)[Database Layer]

    service webapp(internet)[Web App] in frontend
    service api(server)[API Server] in backend
    service cache(disk)[Cache] in backend
    service db(database)[Database] in database

    webapp:B --> T:api
    api:R --> L:cache
    api:B --> T:db
    cache:B --> T:db
```

**Giải thích:**
- **Frontend Layer**: Nơi user tương tác
- **Backend Layer**: Xử lý business logic
- **Database Layer**: Lưu trữ dữ liệu
- **Cache**: Tăng tốc truy xuất dữ liệu

---

## 2. Web Application Architecture

### 🎯 **Mục tiêu**: Kiến trúc web app cơ bản với load balancer

```mermaid
architecture-beta
    group internet(cloud)[Internet]
    group edge(cloud)[Edge Layer]
    group app(cloud)[Application Layer]
    group data(cloud)[Data Layer]

    service users(internet)[Users] in internet
    service cdn(disk)[CDN] in edge
    service lb(server)[Load Balancer] in edge
    service web1(server)[Web Server A] in app
    service web2(server)[Web Server B] in app
    service web3(server)[Web Server C] in app
    service redis(disk)[Redis Cache] in data
    service mysql(database)[MySQL DB] in data

    users:B --> T:cdn
    users:B --> T:lb
    lb:B --> T:web1
    lb:B --> T:web2
    lb:B --> T:web3
    web1:B --> T:redis
    web2:B --> T:redis
    web3:B --> T:redis
    web1:B --> T:mysql
    web2:B --> T:mysql
    web3:B --> T:mysql
```

**Key Components:**
- **CDN**: Phục vụ static assets
- **Load Balancer**: Phân phối traffic
- **Multiple Web Servers**: Horizontal scaling
- **Redis Cache**: Session và data caching
- **MySQL**: Primary database

---

## 3. Microservices Architecture

### 🎯 **Mục tiêu**: Kiến trúc microservices với API Gateway

```mermaid
architecture-beta
    group external(cloud)[External]
    group gateway(cloud)[API Gateway]
    group services(cloud)[Microservices]
    group messaging(cloud)[Message Queue]
    group storage(cloud)[Storage]

    service client(internet)[Client App] in external
    service apiGw(server)[API Gateway] in gateway
    service userSvc(server)[User Service] in services
    service orderSvc(server)[Order Service] in services
    service paymentSvc(server)[Payment Service] in services
    service notifSvc(server)[Notification Service] in services
    service queue(disk)[Message Queue] in messaging
    service userDb(database)[User DB] in storage
    service orderDb(database)[Order DB] in storage
    service paymentDb(database)[Payment DB] in storage

    client:B --> T:apiGw
    apiGw:B --> T:userSvc
    apiGw:B --> T:orderSvc
    apiGw:B --> T:paymentSvc
    orderSvc:B --> T:queue
    paymentSvc:B --> T:queue
    queue:B --> T:notifSvc
    userSvc:B --> T:userDb
    orderSvc:B --> T:orderDb
    paymentSvc:B --> T:paymentDb
```

**Benefits:**
- **Loose coupling**: Mỗi service độc lập
- **Scalability**: Scale từng service riêng biệt
- **Fault isolation**: Lỗi một service không ảnh hưởng toàn hệ thống
- **Technology diversity**: Mỗi service có thể dùng tech stack khác

---

## 4. E-commerce System Architecture

### 🎯 **Mục tiêu**: Hệ thống e-commerce hoàn chỉnh

```mermaid
architecture-beta
    group users(cloud)[Users]
    group edge(cloud)[Edge Security]
    group application(cloud)[Application Tier]
    group processing(cloud)[Background Processing]
    group storage(cloud)[Storage Tier]

    service customer(internet)[Customer] in users
    service admin(internet)[Admin] in users
    service waf(server)[WAF] in edge
    service cdn(disk)[CDN] in edge
    service lb(server)[Load Balancer] in edge
    service web(server)[Web App] in application
    service api(server)[API Server] in application
    service search(server)[Search Service] in application
    service emailWorker(server)[Email Worker] in processing
    service orderWorker(server)[Order Worker] in processing
    service queue(disk)[Queue] in processing
    service redis(disk)[Redis] in storage
    service postgres(database)[PostgreSQL] in storage
    service elasticsearch(database)[Elasticsearch] in storage
    service s3(disk)[S3 Storage] in storage

    customer:B --> T:waf
    admin:B --> T:waf
    waf:B --> T:cdn
    waf:B --> T:lb
    lb:B --> T:web
    lb:B --> T:api
    web:R --> L:api
    api:B --> T:search
    api:B --> T:queue
    api:R --> L:redis
    api:B --> T:postgres
    search:B --> T:elasticsearch
    queue:B --> T:emailWorker
    queue:B --> T:orderWorker
    emailWorker:B --> T:postgres
    orderWorker:B --> T:postgres
    api:B --> T:s3
```

**Advanced Features:**
- **WAF**: Web Application Firewall
- **Search Service**: Elasticsearch for product search
- **Background Workers**: Async processing
- **S3 Storage**: File uploads (images, documents)

---

## 5. CI/CD Pipeline Architecture

### 🎯 **Mục tiêu**: DevOps pipeline cho deployment

```mermaid
architecture-beta
    group development(cloud)[Development]
    group cicd(cloud)[CICD Pipeline]
    group testing(cloud)[Testing]
    group deployment(cloud)[Deployment]
    group monitoring(cloud)[Monitoring]

    service developer(internet)[Developer] in development
    service github(server)[GitHub] in development
    service jenkins(server)[Jenkins] in cicd
    service docker(server)[Docker Build] in cicd
    service testRunner(server)[Test Runner] in testing
    service staging(server)[Staging] in testing
    service production(server)[Production] in deployment
    service k8s(server)[Kubernetes] in deployment
    service prometheus(server)[Prometheus] in monitoring
    service grafana(server)[Grafana] in monitoring

    developer:B --> T:github
    github:B --> T:jenkins
    jenkins:B --> T:docker
    jenkins:B --> T:testRunner
    testRunner:B --> T:staging
    staging:B --> T:production
    production:B --> T:k8s
    k8s:B --> T:prometheus
    prometheus:B --> T:grafana
```

**Pipeline Stages:**
1. **Code Commit**: Developer push code to GitHub
2. **Build**: Jenkins triggers Docker build
3. **Test**: Automated testing
4. **Deploy**: Deploy to staging, then production
5. **Monitor**: Prometheus + Grafana monitoring

---

## 6. Cloud-Native Architecture

### 🎯 **Mục tiêu**: AWS-based cloud architecture

```mermaid
architecture-beta
    group internet(cloud)[Internet]
    group aws(cloud)[AWS Cloud]
    group compute(cloud)[Compute]
    group storage(cloud)[Storage]
    group monitoring(cloud)[Monitoring]

    service users(internet)[Users] in internet
    service cloudfront(disk)[CloudFront] in aws
    service alb(server)[ALB] in aws
    service apiGateway(server)[API Gateway] in aws
    service lambda(server)[Lambda] in compute
    service ecs(server)[ECS] in compute
    service ec2(server)[EC2] in compute
    service rds(database)[RDS] in storage
    service s3(disk)[S3] in storage
    service elasticache(disk)[ElastiCache] in storage
    service cloudwatch(server)[CloudWatch] in monitoring
    service xray(server)[XRay] in monitoring

    users:B --> T:cloudfront
    users:B --> T:alb
    cloudfront:B --> T:alb
    alb:B --> T:apiGateway
    apiGateway:B --> T:lambda
    apiGateway:B --> T:ecs
    ecs:B --> T:ec2
    lambda:B --> T:rds
    ecs:B --> T:rds
    lambda:B --> T:s3
    ecs:B --> T:s3
    lambda:B --> T:elasticache
    ecs:B --> T:elasticache
    lambda:B --> T:cloudwatch
    ecs:B --> T:cloudwatch
    cloudwatch:B --> T:xray
```

**AWS Services:**
- **CloudFront**: Global CDN
- **ALB**: Application Load Balancer
- **Lambda**: Serverless functions
- **ECS**: Container orchestration
- **RDS**: Managed database
- **ElastiCache**: Managed Redis/Memcached
- **XRay**: Distributed tracing

---

## 7. High Availability Architecture

### 🎯 **Mục tiêu**: Kiến trúc đảm bảo uptime cao

```mermaid
architecture-beta
    group region1(cloud)[Region Primary]
    group region2(cloud)[Region DR]
    group global(cloud)[Global Services]
    group monitoring(cloud)[Monitoring]

    service dns(server)[Route53] in global
    service waf(server)[WAF] in global
    service primaryLB(server)[Primary LB] in region1
    service primaryApp1(server)[App A] in region1
    service primaryApp2(server)[App B] in region1
    service primaryDB(database)[Primary DB] in region1
    service drLB(server)[DR LB] in region2
    service drApp1(server)[App A] in region2
    service drApp2(server)[App B] in region2
    service drDB(database)[Replica DB] in region2
    service healthCheck(server)[Health Check] in monitoring
    service alerting(server)[Alerting] in monitoring

    dns:B --> T:waf
    waf:B --> T:primaryLB
    waf:B --> T:drLB
    primaryLB:B --> T:primaryApp1
    primaryLB:B --> T:primaryApp2
    primaryApp1:B --> T:primaryDB
    primaryApp2:B --> T:primaryDB
    drLB:B --> T:drApp1
    drLB:B --> T:drApp2
    drApp1:B --> T:drDB
    drApp2:B --> T:drDB
    primaryDB:R --> L:drDB
    healthCheck:B --> T:primaryLB
    healthCheck:B --> T:drLB
    healthCheck:B --> T:alerting
```

**HA Features:**
- **Multi-Region**: Primary + Disaster Recovery
- **Database Replication**: Real-time sync
- **Health Monitoring**: Automatic failover
- **DNS Failover**: Route 53 health checks

---

## 8. Data Pipeline Architecture

### 🎯 **Mục tiêu**: Big Data processing pipeline

```mermaid
architecture-beta
    group sources(cloud)[Data Sources]
    group ingestion(cloud)[Data Ingestion]
    group processing(cloud)[Data Processing]
    group storage(cloud)[Data Storage]
    group analytics(cloud)[Analytics]

    service apps(server)[Applications] in sources
    service logs(server)[Log Files] in sources
    service apis(server)[External APIs] in sources
    service kafka(server)[Kafka] in ingestion
    service kinesis(server)[Kinesis] in ingestion
    service spark(server)[Spark] in processing
    service airflow(server)[Airflow] in processing
    service hdfs(disk)[HDFS] in storage
    service redshift(database)[Redshift] in storage
    service s3data(disk)[S3 DataLake] in storage
    service tableau(server)[Tableau] in analytics
    service jupyter(server)[Jupyter] in analytics

    apps:B --> T:kafka
    logs:B --> T:kinesis
    apis:B --> T:kafka
    kafka:B --> T:spark
    kinesis:B --> T:spark
    spark:B --> T:airflow
    airflow:B --> T:hdfs
    airflow:B --> T:redshift
    airflow:B --> T:s3data
    redshift:B --> T:tableau
    s3data:B --> T:jupyter
    hdfs:B --> T:jupyter
```

**Data Flow:**
1. **Ingestion**: Kafka/Kinesis collect data
2. **Processing**: Spark processes data
3. **Orchestration**: Airflow manages workflows
4. **Storage**: HDFS, Redshift, S3 store processed data
5. **Analytics**: Tableau, Jupyter for visualization

---

## 🎯 Tổng kết: Cách sử dụng Mermaid Architecture

### 📝 **Syntax Summary:**

```mermaid
architecture-beta
    group groupName(icon)[Group Label]
    service serviceName(icon)[Service Label] in groupName
    junction junctionName
    
    serviceName:T --> B:anotherService
    serviceName{group}:R --> L:externalService{group}
```

### 🔧 **Key Elements:**

1. **Groups**: `group name(icon)[Label]`
2. **Services**: `service name(icon)[Label] in group`
3. **Junctions**: `junction name`
4. **Connections**: `service1:T --> B:service2`
5. **Directions**: `T` (Top), `B` (Bottom), `L` (Left), `R` (Right)

### 🎨 **Available Icons:**
- `cloud`, `server`, `database`, `disk`, `internet`
- AWS icons: `logos:aws-lambda`, `logos:aws-s3`, `logos:aws-ec2`
- 200,000+ icons from iconify.design

### 💡 **Best Practices:**

1. **Group related services** together
2. **Use descriptive labels** for clarity
3. **Show data flow direction** with arrows
4. **Keep diagrams simple** and readable
5. **Use consistent naming** conventions


