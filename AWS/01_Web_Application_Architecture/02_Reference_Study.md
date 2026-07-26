# Chapter 1. Three-tier rationale, two-tier versus three-tier

## Why did AWS choose three tiers?

AWS separates the architecture into the Web Tier, Application Tier, and Data Tier to separate presentation, business logic, and persistent data.
This allows each layer to evolve, deploy, and scale independently while reducing coupling.

---

## Benefits

- Independent scaling of each tier
- Clear separation of responsibilities
- Stronger security boundaries
- Easier maintenance and deployments
- Better fault isolation

---

## Alternative Options

### Option 1. Two-tier Architecture

#### When should it be used?

- Small internal systems
- Simple CRUD applications
- Limited traffic

#### Why wasn't it chosen?

The Web Tier and business logic become tightly coupled. As traffic grows, both layers must scale together even if only one experiences increased load.

---

## Conclusion

A three-tier architecture provides greater scalability, maintainability, and security for enterprise workloads.

---

# Chapter 2. Two ALBs

## Why did AWS choose to use two ALBs?

AWS separates Internet-facing traffic from internal application traffic by using a Public ALB and an Internal ALB.
The Public ALB accepts requests from CloudFront and distributes them to the Web Tier.
The Internal ALB accepts requests only from the Web Tier and distributes them to the Application Tier.

---

## Benefits

- Keeps the Application Tier private
- Independent scaling of Web and Application tiers
- Reduced attack surface
- Separate routing policies
- Easier maintenance

---

## Alternative Options

### Option 1. Single ALB

#### When should it be used?

- Small applications
- Low traffic
- Simple architecture

#### Why wasn't it chosen?

A single ALB reduces architectural separation and exposes more internal components while limiting flexibility.

---

## Conclusion

Two ALBs improve security, scalability, and separation of concerns.

---

# Chapter 3. Scalability and bottleneck analysis under 10× traffic

## What will happen and where will be the bottleneck?

### Components that scale automatically

- Application Load Balancers
- EC2 Auto Scaling Groups
- Amazon EFS

### Potential bottlenecks

#### Amazon RDS

- Write throughput
- Connection limits

Mitigation:

- Read Replicas
- RDS Proxy
- Larger instance class
- Query optimization

#### ElastiCache (Redis)

Potential issue:

- Memory exhaustion
- Cache misses

Mitigation:

- Cluster mode
- Horizontal scaling
- Eviction policy tuning

#### NAT Gateway

Potential issue:

- Heavy outbound traffic

Mitigation:

- Deploy one NAT Gateway per Availability Zone
- Use VPC Endpoints for AWS services

---

## Conclusion

Compute scales horizontally, but stateful components usually become the first bottlenecks and require architectural planning.

---

# Chapter 4. High availability and failure scenarios

## Why did AWS deploy across multiple Availability Zones?

Deploying resources across multiple AZs provides redundancy and high availability.

---

## Benefits

- Survives single AZ failures
- Higher service availability
- Automatic failover
- Better disaster resilience

---

## Active-Active vs Active-Passive

### Web Tier

Active-Active because stateless web servers can process requests simultaneously.

### Application Tier

Active-Active because business logic remains stateless.

### Database Tier

Active-Passive (Multi-AZ) to preserve consistency while providing automatic failover.

---

## Failure Scenario

If one AZ fails:

- ALB routes traffic to healthy instances.
- Auto Scaling replaces failed instances.
- Multi-AZ RDS promotes the standby instance.

---

## Conclusion

AWS combines Active-Active compute with Active-Passive databases to balance availability and data consistency.

---

# Chapter 5. CloudFront and AWS WAF

## Why did AWS choose CloudFront?

CloudFront caches static content at Edge Locations close to users.

### Benefits

- Lower latency
- Reduced origin traffic
- Better user experience
- Lower backend cost

### Alternative Option

#### Direct ALB access

**When should it be used?**

- Internal applications
- Small deployments

**Why wasn't it chosen?**

Every request reaches the origin, increasing latency and backend load.

---

## Why did AWS choose AWS WAF?

AWS WAF filters malicious HTTP/HTTPS traffic before requests reach the application.

### Benefits

- Protection against SQL Injection
- Protection against XSS
- Rate limiting
- Reduced attack surface

### Alternative Option

#### Security Groups only

**When should it be used?**

- Network-level filtering

**Why wasn't it chosen?**

Security Groups cannot inspect HTTP requests or apply application-layer security rules.

---

## Conclusion

CloudFront improves performance, while AWS WAF strengthens application-layer security.
