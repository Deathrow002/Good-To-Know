# Java Preparation Guide

A structured collection of Q&As covering core Java, Spring ecosystem, messaging, reactive programming, and SQL — from fundamentals to advanced topics.

---

## Contents

| # | File | Topics Covered |
|---|---|---|
| 1 | [Core Java](1.%20Core-Java.md) | OOP, Collections, Generics, Exception Handling, Concurrency, Memory Model |
| 2 | [Advanced Java](2.%20Advanced-Java.md) | Streams, Lambdas, Optional, Reflection, Design Patterns, JVM Internals |
| 3 | [Spring Boot](3.%20Spring-Boot.md) | Auto-configuration, Starters, Actuator, REST, Profiles, Testing |
| 4 | [Spring Data](4.%20Spring-Data.md) | JPA, Hibernate, Repositories, Projections, Transactions, Redis, Caching |
| 5 | [Spring Security](5.%20Spring-Security.md) | Authentication, Authorization, JWT, OAuth2, CSRF, Cookies, Security Patterns |
| 6 | [Spring WebFlux](6.%20Spring%20WebFlux.md) | Reactive Streams, Mono/Flux, Back-pressure, WebClient, R2DBC, Testing |
| 7 | [Rabbit MQ](7.%20Rabbit-MQ.md) | Exchanges, Queues, Bindings, Routing, Dead Letter, Spring AMQP |
| 8 | [Apache Kafka](8.%20Apache-Kafka.md) | Producers, Consumers, Topics, Partitions, Consumer Groups, Streams, Error Handling |
| 9 | [Active MQ](9.%20Active-MQ.md) | Brokers, Destinations, Message Selectors, Spring JMS, Persistence |
| 10 | [SQL Basic](10.%20SQL-Basic.md) | DDL/DML/DCL/TCL, Joins, Indexes, Aggregates, Subqueries, Window Functions, Normalization, Transactions, Views, Constraints |
| 11 | [SQL Advanced](11.%20SQL-ADVANCE.md) | CTEs, Recursive Queries, Execution Plans, Partitioning, Materialized Views, Locks, Isolation Levels, Sharding, Anti-Patterns |
| 12 | [Linux/Unix Basic](12.%20Linux-Unix.md) | OS Differences, Basic Commands, File Viewing/Permissions, Redirection & Piping |
| 13 | [Linux/Unix Advanced](13.%20Linux-Unix-Advanced.md) | Processes/Threads, Boot Process, Zombies, OOM Killer, Inodes, Links, TIME_WAIT, Capabilities, SELinux, epoll |
| 14 | [Basic Docker](14.%20basic-docker.md) | Containers vs. VMs, Images, Dockerfile, Architecture, Volumes, COPY vs ADD, CLI |
| 15 | [Advanced Docker](15.%20advanced-docker.md) | Multi-stage Builds, BuildKit, Network Drivers, Rootless Mode, Init systems/tini, Storage Drivers/CoW, cgroups/namespaces, Resource Pruning |
| 16 | [Basic Kubernetes](16.%20Kubernetes%20Interview.md) | Control Plane/Data Plane, Pods/ReplicaSets/Deployments, Services, StatefulSets, Probes, Storage (PV/PVC/StorageClass) |
| 17 | [Advanced Kubernetes](17.%20Kubernetes%20Advanced%20Interview.md) | RBAC, Autoscaling (HPA/VPA/KEDA), Network Policy, Operators, Service Mesh, GitOps, Admission Webhooks, Zero-Downtime Deployments |
| 18 | [Helm](18.%20Helm%20Interview.md) | Chart Structure, Templating, Lifecycle Hooks, Repositories, Values, Subcharts, Testing, Debugging |
| 19 | [Basic Shell Script](19.%20Basic%20Shell%20Script.md) | Variables, Control Flow, Functions, I/O, Scripting Patterns |
| 20 | [Twelve-Factor App](20.%20Twelve-Factor-App.md) | 12-Factor Methodology, Cloud-Native Best Practices, SaaS Patterns |
| 21 | [IBM Integration Products](21.%20IBM-Integration-Products.md) | IBM MQ, Event Streams, API Connect, ACE, DataPower, Sterling B2B, Aspera, CP4I |
| 23 | [Oracle Base Database Services](23.%20Oracle-Base-Database-Services.md) | OCI BaseDB Architecture, Provisioning, Lifecycle, Backup and Recovery, Data Guard, Security, Migration, Monitoring |
| 24 | [OCI Developer Professional](24.%20Oracle-Cloud-Infrastructure-Developer.md) | Cloud-Native Design, Containers, OCIR, OKE, Functions, API Gateway, Streaming, Queue, DevOps, Observability, Security |
| 25 | [OCI Architect Associate](25.%20Oracle-Cloud-Infrastructure-Architect-Associate.md) | OCI Core Architecture, IAM, Networking, Compute, Storage, Databases, HA/DR, Security, Observability, Cost Governance |
| — | [Java Versions](Java-Versions.md) | Key features per Java release (Java 8 → Java 21+) |

---

## Topic Overview

### Core Java
Fundamentals every Java developer must know:
- OOP principles (Encapsulation, Inheritance, Polymorphism, Abstraction)
- Collections Framework (`List`, `Map`, `Set`, `Queue`) and their internal implementations
- Generics, wildcards, and type erasure
- Exception handling — checked vs. unchecked, custom exceptions
- Multithreading and concurrency (`synchronized`, `volatile`, `ExecutorService`, locks)
- Java Memory Model and garbage collection

### Advanced Java
Deeper language features and architectural concepts:
- Functional programming — Streams API, Lambda expressions, method references
- `Optional<T>` for null safety
- Reflection and annotations
- Common design patterns (Singleton, Builder, Factory, Strategy, Observer)
- JVM internals — class loading, bytecode, JIT compilation

### Spring Boot
Building production-ready Spring applications:
- Auto-configuration and `@SpringBootApplication`
- Dependency injection and the Spring IoC container
- REST API development with `@RestController`
- Spring Boot Actuator for health checks and metrics
- Profiles, externalized configuration (`application.yml`)
- Testing with `@SpringBootTest`, `@MockBean`, `MockMvc`

### Spring Data, JPA & Hibernate
Persistent data access:
- JPA specification and `EntityManager`
- Hibernate as a JPA provider — dirty checking, second-level cache, `ddl-auto`
- Entity lifecycle states — Transient, Managed, Detached, Removed
- Relationship mappings — `@OneToMany`, `@ManyToMany`, cascade, fetch strategies
- JPQL, Criteria API, Native Queries
- Spring Data repositories — `JpaRepository`, derived queries, `@Query`
- Projections and DTOs for performance
- `@Transactional` — propagation, isolation, rollback rules
- Redis caching — `RedisCacheManager`, `RedisTemplate`, `@Cacheable`
- N+1 select problem and solutions (`JOIN FETCH`, `@EntityGraph`, `@BatchSize`)

### Spring Security
Securing Spring applications:
- Filter chain architecture — `DelegatingFilterProxy`, `SecurityFilterChain`
- Authentication vs Authorization (AuthN vs AuthZ)
- JWT — structure, Access Tokens, Refresh Tokens, rotation
- OAuth2 / OpenID Connect — authorization code flow, resource server
- CSRF protection — Synchronizer Token Pattern, `SameSite` cookies
- Cookie security attributes — `HttpOnly`, `Secure`, `SameSite`
- Authentication patterns — Form Login, HTTP Basic, JWT Bearer, OAuth2, API Key
- Authorization patterns — RBAC, ABAC, PBAC, ReBAC, ACL
- SecurityContext propagation in async and reactive contexts

### Spring WebFlux
Reactive, non-blocking web programming:
- Reactive Streams specification — Publisher, Subscriber, back-pressure
- `Mono<T>` and `Flux<T>` — core reactive types
- Two programming models — annotated controllers and functional endpoints (`RouterFunction`)
- Error handling in reactive pipelines
- `WebClient` — non-blocking HTTP client replacing `RestTemplate`
- Reactive database access — R2DBC, Spring Data Reactive repositories
- Testing with `WebTestClient`

### Rabbit MQ
Message queuing with AMQP:
- Core concepts — Producers, Consumers, Queues, Exchanges, Bindings, Routing Keys, vhosts, Channels
- Exchange types — Direct, Fanout, Topic, Headers
- Durability — Durable vs. transient queues, persistent messages
- Message acknowledgement — Auto-ACK vs. Manual ACK, `basicAck`, `basicNack`/`basicReject`
- Dead Letter Exchange (DLX) logic and configurations

### Apache Kafka
Event streaming and asynchronous messaging:
- Core concepts — brokers, topics, partitions, offsets
- Producers, consumers, and consumer groups
- Message ordering and partition keys
- Kafka Streams for stream processing
- Spring Kafka — `@KafkaListener`, `KafkaTemplate`
- Error handling — dead letter topics, retry, idempotency

### Active MQ
Enterprise messaging with JMS:
- Key features & JMS 1.1 / 2.0 implementation
- Core interfaces — `ConnectionFactory`, `Session`, `Destination`, `MessageProducer`, `MessageConsumer`
- Point-to-Point (Queue) vs. Publish-Subscribe (Topic) messaging models
- Durable Subscriptions for message retention
- Standard JMS message types (`TextMessage`, `BytesMessage`, `MapMessage`, `ObjectMessage`, `StreamMessage`)

### SQL — Basic
Relational database essentials:
- SQL command categories — DDL, DML, DCL, TCL
- `DELETE` vs `TRUNCATE` vs `DROP`
- All JOIN types — INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF
- `WHERE` vs `HAVING` — filtering rows vs. groups
- Primary Key, Foreign Key, and Unique Key
- Indexes — Clustered, Non-Clustered, Composite, Unique
- Aggregate functions — `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- Subqueries — single-row, multi-row, correlated, derived tables
- Window functions — `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`, `LEAD`
- Normalization — 1NF, 2NF, 3NF with examples
- Transactions and ACID properties
- Views — creation, updatable views, limitations
- `UNION` vs `UNION ALL`
- Stored Procedures vs. Functions
- Constraints — `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT`, `FOREIGN KEY`

### SQL — Advanced
Performance, architecture, and production patterns:
- CTEs and Recursive CTEs — hierarchical data traversal (org charts, trees)
- Advanced window functions — frame clauses, rolling averages, `FIRST_VALUE`, `NTH_VALUE`
- Advanced JOINs — LATERAL, Anti-Join, Semi-Join, Non-Equi JOIN
- SQL logical execution order and query optimization
- Reading `EXPLAIN`/`EXPLAIN ANALYZE` output — plan nodes, cost metrics
- Database partitioning — Range, List, Hash partitioning with partition pruning
- Materialized Views — storage, refresh strategies, indexes on views
- Locks and Deadlocks — lock types, deadlock scenarios, `SKIP LOCKED`, prevention
- Isolation levels and concurrency anomalies — dirty reads, phantom reads, lost updates
- OLTP vs OLAP schema design — Star Schema, Fact and Dimension tables
- `GROUPING SETS`, `ROLLUP`, `CUBE` — multi-level aggregations
- Efficient pagination — OFFSET pitfalls, Keyset/Cursor pagination
- Triggers — BEFORE/AFTER/INSTEAD OF, audit log, validation triggers
- Sharding vs Replication — shard key strategies, tradeoffs
- SQL anti-patterns — SELECT *, implicit casts, correlated subqueries, NOT IN with NULLs

### Linux/Unix Basic
Fundamentals of Linux/Unix command-line:
- Difference between Linux and Unix
- Basic navigation and file manipulation (`ls`, `cd`, paths, absolute vs. relative)
- File viewing commands (`cat`, `less`, `head`, `tail`)
- File permissions and `chmod` (numeric representations, read/write/execute)
- Superuser access via `sudo` and administrative privileges
- Pattern searching inside files with `grep`
- Basic process monitoring (`ps`, `top`)
- Output redirection and command piping (`>`, `>>`, `|`)

### Linux/Unix Advanced
Deep dive into operating system internals and advanced configuration:
- Kernel level distinctions between processes and threads (`clone()`, `task_struct`)
- Linux boot process from BIOS/UEFI to login prompt
- Zombie processes management, parent signalling, and init reaping
- Out-of-Memory (OOM) Killer scoring, monitoring (`oom_score_adj`), and troubleshooting
- Inode structure, metadata (`stat`), and inode exhaustion
- Hard links vs. Symbolic (soft) links
- TCP connection states (TIME_WAIT, socket reuse configurations)
- Linux Capabilities (`cap_net_bind_service`, etc.) and securing binaries
- Standard Unix Discretionary Access Control (DAC) vs. SELinux Mandatory Access Control (MAC)
- I/O multiplexing comparison: `select()`, `poll()`, and `epoll()`

### Basic Docker
Essential concepts of containerization:
- Containerization benefits and the "it works on my machine" resolution
- Image (immutable template) vs. Container (running instance)
- Docker vs. Virtual Machines (OS kernel sharing vs. hypervisor/guest OS)
- Dockerfile syntax (`FROM`, `RUN`, `COPY`, `CMD`) and build layers
- Running new containers (`docker run`) vs. starting existing ones (`docker start`)
- Docker Client-Server architecture (Client, Daemon, Registry)
- Persistent storage with Docker Volumes
- File inclusion using `COPY` vs `ADD`
- Docker Hub and container registries
- Safe removal of containers and images (`docker rm`/`docker rmi`)

### Advanced Docker
Advanced container optimization, storage, and networking:
- Multi-stage builds for image optimization and security
- BuildKit features (parallel execution, build secrets, remote caching)
- Network drivers (Overlay, Macvlan, Host)
- Security hardening (non-root `USER` declaration, Rootless Docker mode)
- PID 1 zombie process mitigation with lightweight init systems (`tini`)
- Storage driver architectures (`overlay2`) and Copy-on-Write (CoW) filesystems
- Linux namespace isolation vs. control group (cgroup) limits
- Troubleshooting container termination and OOMKilled exit code 137
- Zero-downtime rolling updates with Docker Compose
- Safe container resource pruning (dangling image/volume cleaning, label filters)

### Basic Kubernetes
Core orchestration concepts and architecture:
- Cluster architecture (Control Plane: apiserver, etcd, scheduler, controller-manager; Data Plane: kubelet, kube-proxy, container runtime)
- Pod vs. ReplicaSet vs. Deployment workloads
- Service accessibility types (ClusterIP, NodePort, LoadBalancer, ExternalName, Headless)
- StatefulSets vs. Deployments (identity persistence, ordered startup, stable DNS, dedicated volume claims)
- Container monitoring using liveness, readiness, and startup probes
- Persistent storage abstractions: PersistentVolume (PV), PersistentVolumeClaim (PVC), and dynamic StorageClasses

### Advanced Kubernetes
Enterprise-grade Kubernetes configuration, security, and operations:
- Role-Based Access Control (RBAC): Roles, ClusterRoles, RoleBindings, and ClusterRoleBindings
- Scaling strategies: Horizontal Pod Autoscaler (HPA), Vertical Pod Autoscaler (VPA), and event-driven KEDA
- Network Policies for pod-to-pod communication restriction (ingress/egress rules)
- Operator pattern and Custom Resource Definitions (CRDs)
- Service mesh fundamentals (Istio, Linkerd) and sidecar injection
- GitOps deployments using tools like ArgoCD or Flux
- Custom validating and mutating Admission Webhooks
- Zero-downtime deployment strategies (RollingUpdate, Blue-Green, Canary)

### Helm
Package management for Kubernetes:
- Chart-based application packaging and parameterization benefits
- Standard chart directory layout (`Chart.yaml`, `values.yaml`, `templates/`, `charts/`)
- Go templates rendering context (`.Values`, `.Release`, `.Chart`, `.Files`, `.Capabilities`)
- Advanced Helm templating (Sprig functions, control structures, pipelines)
- Helm lifecycle hooks for pre/post installation operations
- Subcharts, global values sharing, and dependency resolution
- Revisions, rollback history, and state storage as Secrets
- Packaging, signing, and hosting charts in repositories

### Java Versions
Key language and JVM improvements per release:
- **Java 8** — Lambdas, Streams, `Optional`, default methods, Date/Time API
- **Java 11** — `var` improvements, HTTP Client, `String` utilities
- **Java 14–16** — Records, Sealed Classes (preview), Pattern Matching (`instanceof`)
- **Java 17** — LTS: Records, Sealed Classes (stable), Text Blocks
- **Java 21** — LTS: Virtual Threads (Project Loom), Pattern Matching for `switch`, Sequenced Collections

---

## How to Use

1. Start with **Core Java** to solidify fundamentals.
2. Progress to **Advanced Java** for language-level depth.
3. Study **Spring Boot** as the foundation for the Spring ecosystem.
4. Cover **Spring Data** for persistence knowledge.
5. Study **Spring Security** and **Spring WebFlux** for modern Spring patterns.
6. Review **Rabbit MQ**, **Apache Kafka**, and **Active MQ** for asynchronous messaging and distributed system architectures.
7. Study **SQL Basic** and **SQL Advanced** for relational database fundamentals, advanced querying, performance tuning, and schema design.
8. Review **Linux/Unix Basic** and **Linux/Unix Advanced** to master core shell navigation, file system internals, network configuration, and process management.
9. Cover **Basic Docker** and **Advanced Docker** to understand containerization, volume mapping, network drivers, multi-stage optimizations, and security.
10. Explore **Basic Kubernetes** and **Advanced Kubernetes** to master container orchestration, cluster components, storage provisioning, RBAC security, and scaling.
11. Study **Helm** to bundle, configure, and deploy application packages on Kubernetes.
12. Use **Java Versions** as a quick reference for release features from Java 8 through Java 21+.

---

## Study Tips

- Each file follows a **Question → Answer** format; try answering before reading.
- Code examples are provided for every concept — read them carefully.
- Pay attention to **comparison tables** — they are common in interviews.
- Focus on the **"why"** behind each design decision, not just the "what".

---

