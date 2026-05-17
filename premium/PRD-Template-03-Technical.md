# PRD Template 03: Technical Architecture Document

## For Complex Products Requiring System Design

This technical PRD focuses on architecture decisions, system design, data flows, and technical constraints. Use for products with significant technical complexity or team-based development.

---

## Document Information

**Project:** _________________________________  
**Version:** 1.0 | **Date:** ____________ | **Author:** _________________________________

---

## Section 1: System Overview

### 1.1 Architecture Principles

Our architecture follows these guiding principles:

1. **Scalability:** System must handle 10x current load without architectural changes
2. **Maintainability:** Code and architecture must be understandable by new team members within 2 weeks
3. **Reliability:** 99.9% uptime target with graceful degradation
4. **Security:** Security by design, not as an afterthought
5. **Cost-efficiency:** Optimize for operational costs at expected scale

### 1.2 High-Level Architecture

```
                                    ┌─────────────────┐
                                    │   CDN / Edge    │
                                    │   (Cloudflare)  │
                                    └────────┬────────┘
                                             │
                                    ┌────────▼────────┐
                                    │   Load Balancer │
                                    │   (AWS ALB)     │
                                    └────────┬────────┘
                                             │
                         ┌───────────────────┼───────────────────┐
                         │                   │                   │
                ┌────────▼────────┐ ┌────────▼────────┐ ┌────────▼────────┐
                │   Web Server 1  │ │   Web Server 2  │ │   Web Server 3  │
                │   (Node.js)    │ │   (Node.js)    │ │   (Node.js)    │
                └────────┬────────┘ └────────┬────────┘ └────────┬────────┘
                         │                   │                   │
                         └───────────────────┼───────────────────┘
                                             │
                                    ┌────────▼────────┐
                                    │   Redis Cache  │
                                    │   (Session)    │
                                    └────────┬────────┘
                                             │
                                    ┌────────▼────────┐
                                    │   PostgreSQL   │
                                    │   (Primary DB) │
                                    └─────────────────┘
```

---

## Section 2: Infrastructure

### 2.1 Cloud Provider and Regions

**Primary Region:** _________________________________  
**Secondary Region (DR):** _________________________________

**Services Used:**

| Service | Provider | Purpose |
|---------|----------|---------|
| Compute | | |
| Database | | |
| Cache | | |
| Storage | | |
| CDN | | |
| DNS | | |
| Email | | |
| Monitoring | | |

### 2.2 Server Configuration

| Component | Specs | Count | Scaling |
|-----------|-------|-------|---------|
| Web Servers | 4 vCPU, 8GB RAM | 3 | Auto-scale 2-10 |
| Database | 8 vCPU, 32GB RAM | 1 primary + 2 replicas | Vertical + read replicas |
| Cache | 4 vCPU, 16GB RAM | 3 | Cluster mode |
| Worker Nodes | 2 vCPU, 4GB RAM | 2-20 | Based on queue depth |

### 2.3 Networking

**VPC CIDR:** _________________________________  
**Public Subnets:** _________________________________  
**Private Subnets:** _________________________________

**Security Groups:**

| Name | Ports | Source | Purpose |
|------|-------|--------|---------|
| web-servers | 443 | 0.0.0.0/0 | HTTPS traffic |
| app-servers | 3000 | web-servers | Internal API |
| database | 5432 | app-servers | PostgreSQL |
| redis | 6379 | app-servers | Cache access |

---

## Section 3: Data Architecture

### 3.1 Database Schema

#### Core Entities

**Users Table:**

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    email_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    last_login_at TIMESTAMP WITH TIME ZONE,
    is_active BOOLEAN DEFAULT TRUE,
    metadata JSONB DEFAULT '{}'
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at);
```

**Organizations Table:**

```sql
CREATE TABLE organizations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,
    owner_id UUID REFERENCES users(id),
    plan VARCHAR(50) DEFAULT 'free',
    settings JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**[Additional Entity Tables]**

```sql
-- Add additional entities based on product requirements

[Entity Name]
[Schema definition]
[Indexes]
[Relationships]
```

### 3.2 Data Relationships

```
Users ─────┬────── Organizations ─────┐
           │                          │
           │                          │
    ┌──────┴──────┐            ┌──────┴──────┐
    │             │            │             │
 Projects      Sessions      Members      Billing
```

### 3.3 Caching Strategy

| Data Type | Cache Layer | TTL | Invalidation Strategy |
|-----------|------------|-----|---------------------|
| User sessions | Redis | 24 hours | On logout |
| API responses | Redis | 5 minutes | On update |
| Static assets | CDN | 1 year | Version-based |
| Query results | Redis | 1 minute | Time-based |

### 3.4 Data Migration Strategy

- All migrations must be backward-compatible
- Zero-downtime deployment approach
- Migration scripts stored in version control
- Rollback procedures documented for each migration

---

## Section 4: Application Architecture

### 4.1 Backend Architecture

**Framework:** Node.js with Express  
**Language:** TypeScript  
**Pattern:** Layered architecture (Routes → Controllers → Services → Repositories)

**Directory Structure:**

```
src/
├── api/              # API routes and controllers
│   ├── routes/
│   └── controllers/
├── core/             # Business logic
│   ├── services/
│   ├── models/
│   └── interfaces/
├── infrastructure/   # External integrations
│   ├── database/
│   ├── cache/
│   └── queues/
├── shared/           # Utilities and helpers
│   ├── utils/
│   ├── constants/
│   └── errors/
└── config/           # Configuration management
```

### 4.2 Frontend Architecture

**Framework:** [React / Vue / Next.js / etc.]  
**State Management:** [Redux / Vuex / Zustand / etc.]  
**Styling:** [Tailwind / Styled Components / etc.]

**Key Technical Decisions:**

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Bundler | | |
| State library | | |
| CSS approach | | |
| Testing framework | | |
| API client | | |

### 4.3 API Design

#### RESTful Endpoints

**Base URL:** /api/v1

**Users:**

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /users | Create user |
| GET | /users/:id | Get user |
| PATCH | /users/:id | Update user |
| DELETE | /users/:id | Delete user |
| GET | /users/me | Get current user |

**Authentication:**

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /auth/register | Register new user |
| POST | /auth/login | Login |
| POST | /auth/logout | Logout |
| POST | /auth/refresh | Refresh token |
| POST | /auth/forgot-password | Request password reset |
| POST | /auth/reset-password | Reset password |

**[Additional API Endpoints]**

| Method | Endpoint | Description |
|--------|----------|-------------|
| | | |
| | | |

#### API Response Format

**Success Response:**

```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com"
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z"
  }
}
```

**Error Response:**

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      { "field": "email", "message": "Invalid email format" }
    ]
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z"
  }
}
```

### 4.4 Background Jobs

| Job | Queue | Frequency | SLA |
|-----|------|-----------|-----|
| Email sending | emails | Real-time | < 30 seconds |
| Report generation | reports | Scheduled | < 5 minutes |
| Data cleanup | maintenance | Daily | < 1 hour |
| Sync external data | integrations | Hourly | < 10 minutes |

---

## Section 5: Security Architecture

### 5.1 Authentication

**Method:** JWT with refresh token rotation  
**Access Token TTL:** 15 minutes  
**Refresh Token TTL:** 7 days  
**Token Storage:** HttpOnly cookies (not localStorage)

**Password Requirements:**

- Minimum 12 characters
- At least one uppercase, lowercase, number, and symbol
- Common password blacklist
- Bcrypt hashing with cost factor 12

### 5.2 Authorization

**Model:** RBAC (Role-Based Access Control)

| Role | Permissions |
|------|-------------|
| Admin | Full system access |
| Member | Access to own data |
| Guest | Limited read access |

### 5.3 Security Measures

- [ ] HTTPS everywhere
- [ ] HSTS headers
- [ ] CSP headers
- [ ] XSS protection
- [ ] CSRF tokens
- [ ] Rate limiting (100 req/min per IP)
- [ ] Input validation and sanitization
- [ ] SQL injection prevention (parameterized queries)
- [ ] Secrets management (AWS Secrets Manager)
- [ ] Regular security audits

### 5.4 Data Protection

- [ ] Encryption in transit (TLS 1.3)
- [ ] Encryption at rest (AES-256)
- [ ] Database backups (daily, retained 30 days)
- [ ] PII handling compliance (GDPR/CCPA)
- [ ] Data retention policies defined

---

## Section 6: Monitoring and Observability

### 6.1 Logging Strategy

| Log Type | Level | Retention | Tools |
|----------|-------|----------|-------|
| Application | Info, Warn, Error | 30 days | CloudWatch/Papertrail |
| Access logs | All | 90 days | AWS S3 |
| Error logs | Error only | 1 year | Sentry |
| Audit logs | All | 3 years | Custom table |

**Log Format:**

```json
{
  "timestamp": "ISO8601",
  "level": "info",
  "service": "api",
  "trace_id": "uuid",
  "user_id": "uuid",
  "action": "user.login",
  "duration_ms": 45,
  "metadata": {}
}
```

### 6.2 Metrics

**Infrastructure Metrics:**

- CPU utilization (target: < 70%)
- Memory utilization (target: < 80%)
- Disk I/O
- Network throughput
- Error rates (target: < 0.1%)

**Application Metrics:**

- Request latency (p50, p95, p99)
- Request throughput
- Active connections
- Queue depth

**Business Metrics:**

- User signups
- Active users
- Revenue
- Conversion rates

### 6.3 Alerting

| Alert | Condition | Severity | Action |
|-------|-----------|----------|--------|
| High error rate | > 5% errors for 5 min | Critical | Page on-call |
| High latency | p99 > 2s for 10 min | Warning | Slack notification |
| Low disk | < 20% free | Warning | Slack notification |
| High CPU | > 85% for 15 min | Warning | Slack notification |

### 6.4 Dashboards

- [ ] Infrastructure health
- [ ] Application performance
- [ ] Business metrics
- [ ] Error tracking
- [ ] User activity

---

## Section 7: Deployment Strategy

### 7.1 CI/CD Pipeline

**Repository:** GitHub  
**CI Tool:** GitHub Actions  
**Deployment:** AWS CodeDeploy / ECS

**Pipeline Stages:**

```
Push → Lint → Test → Build → Security Scan → Deploy Staging → E2E Tests → Deploy Production
```

### 7.2 Environment Configuration

| Environment | Purpose | URL |
|-------------|---------|-----|
| Development | Local development | localhost:3000 |
| Staging | Pre-production testing | staging.app.com |
| Production | Live users | app.com |

### 7.3 Deployment Process

1. **Code Review:** All changes reviewed by at least one team member
2. **Automated Testing:** All tests must pass before merge
3. **Staging Deployment:** Automatic on main branch
4. **Smoke Tests:** Automated E2E tests on staging
5. **Production Deployment:** Manual trigger after staging verification
6. **Rollback Plan:** One-click rollback to previous version

### 7.4 Rollback Procedures

**Trigger:** Error rate > 5% or p99 latency > 5s for 10 minutes

**Steps:**

1. Alert on-call engineer
2. Evaluate impact and scope
3. If severe, trigger rollback via:
   - AWS Console: Update ECS service to previous task definition
   - CLI: `aws ecs update-service --service app --task-definition app:previous`
4. Verify rollback success
5. Post-mortem within 48 hours

---

## Section 8: Disaster Recovery

### 8.1 Backup Strategy

| Data Type | Frequency | Retention | Storage |
|-----------|-----------|-----------|---------|
| Database | Daily + continuous replication | 30 days daily, 1 year weekly | Cross-region S3 |
| File storage | Daily incremental | 30 days | Cross-region S3 |
| Configuration | On change | Version controlled | Git + S3 |
| Secrets | On change | Version controlled | AWS Secrets Manager |

### 8.2 Recovery Time Objectives

| System | RTO (Recovery Time Objective) | RPO (Recovery Point Objective) |
|--------|------------------------------|------------------------------|
| Database | 4 hours | 1 hour |
| Application | 1 hour | N/A |
| File storage | 4 hours | 24 hours |

### 8.3 Recovery Procedures

**Database Failover:**

1. Promote read replica to primary
2. Update connection strings
3. Verify data integrity
4. Resume operations

**Full Disaster Recovery:**

1. Provision infrastructure in secondary region
2. Restore from latest backup
3. Verify all systems operational
4. Update DNS
5. Confirm data integrity

---

## Section 9: Scalability Planning

### 9.1 Scaling Triggers

| Metric | Warning Threshold | Scale Action |
|--------|-------------------|--------------|
| CPU | > 70% | Add 1 instance |
| Memory | > 80% | Add 1 instance |
| Request queue | > 100 | Add 1 instance |
| Latency p99 | > 2s | Add 2 instances |

### 9.2 Capacity Planning

| Component | Current Capacity | 6 Month Projection | Scaling Approach |
|-----------|-----------------|-------------------|------------------|
| Web servers | 1000 RPS | 5000 RPS | Horizontal |
| Database | 10K ops/sec | 50K ops/sec | Read replicas + sharding |
| Cache | 50K ops/sec | 200K ops/sec | Cluster expansion |
| Storage | 1 TB | 10 TB | Tiered storage |

### 9.3 Performance Budgets

| Metric | Target | Maximum |
|--------|--------|-----------------|
| Page load (LCP) | < 2.5s | 4s |
| API response (p95) | < 200ms | 500ms |
| API response (p99) | < 500ms | 1s |
| Time to Interactive | < 3.5s | 5s |

---

## Section 10: Appendix

### A. Technology Stack Summary

| Layer | Technology | Version |
|-------|------------|---------|
| Frontend framework | | |
| Backend runtime | | |
| Database | | |
| Cache | | |
| Queue | | |
| CDN | | |
| Search | | |
| Monitoring | | |

### B. External Dependencies

| Service | Purpose | SLA | Backup Plan |
|---------|---------|-----|-------------|
| AWS | Cloud infrastructure | 99.99% | Multi-region |
| Stripe | Payments | 99.99% | Manual retry |
| SendGrid | Email | 99.9% | AWS SES fallback |
| Sentry | Error tracking | 99.5% | Log to file |

### C. Glossary

| Term | Definition |
|------|------------|
| RTO | Recovery Time Objective - maximum acceptable downtime |
| RPO | Recovery Point Objective - maximum acceptable data loss |
| p99 | 99th percentile - 99% of requests below this latency |

---

**Document Approval:**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Lead | | | |
| Architect | | | |
| DevOps Lead | | | |
| Security Lead | | | |