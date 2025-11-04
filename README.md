# 🧩 Employee Enrichment API — Spring Boot (JPA, H2, Configurable Query)

![Java](https://img.shields.io/badge/java-17-blue)
![Spring Boot](https://img.shields.io/badge/spring--boot-3.1-green)
![H2](https://img.shields.io/badge/database-H2-lightgrey)
![JPA](https://img.shields.io/badge/JPA-enabled-yellow)
![REST API](https://img.shields.io/badge/API-REST--ful-orange)
![License](https://img.shields.io/github/license/ps-aia-tit/EmployeeEnrichmentAPI)
![Last Commit](https://img.shields.io/github/last-commit/ps-aia-tit/EmployeeEnrichmentAPI)
![Stars](https://img.shields.io/github/stars/ps-aia-tit/EmployeeEnrichmentAPI?style=social)


This project demonstrates three progressive approaches to enrich employee data using Spring Boot and Spring Data JPA. Each version builds on the previous, showcasing modularity, flexibility, and recruiter-facing clarity.

> 📦 Base Package: `com.aiatit.emp`

---

## 📚 Use Case

You have three tables:
- `employee` — basic employee info
- `empdetail` — country and department ID
- `empdept` — department name

The goal is to expose a REST API that returns enriched employee info:
```json
{
  "empid": "EMP001",
  "empname": "Alice",
  "country": "CA",
  "empdept": "IT"
}
```
## ✅ H2 Setup and Schema

`application.yaml`
```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    driverClassName: org.h2.Driver
    username: sa
    password:
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true

employee:
  query:
    enabled: true
    joinFetch: true
    filters:
      empid: null
      country: null
      deptName: null
    pagination:
      default-page-size: 50
      app-page-sizes:
        app-hr: 1000
        app-portal: 20
        app-analytics: 200
```
✅ Notes

• You can access the H2 console at http://localhost:8080/h2-console
• Use admin/admin123 for basic auth (or customize in application.yaml)
• Lombok annotations like @Getter, @Setter, @Data can now be used in your entities and DTOs

## SQL Schema
```sql


CREATE TABLE employee (
    empid VARCHAR(20) PRIMARY KEY,
    empname VARCHAR(100)
);

CREATE TABLE empdetail (
    empid VARCHAR(20) PRIMARY KEY,
    country VARCHAR(100),
    deptid BIGINT
);

CREATE TABLE empdept (
    deptid BIGINT PRIMARY KEY,
    empdept VARCHAR(100)
);
```
## Sample Data
```query


INSERT INTO employee VALUES ('EMP001', 'Alice');
INSERT INTO employee VALUES ('EMP002', 'Bob');

INSERT INTO empdept VALUES (1, 'IT');
INSERT INTO empdept VALUES (2, 'HR');

INSERT INTO empdetail VALUES ('EMP001', 'CA', 1);
INSERT INTO empdetail VALUES ('EMP002', 'US', 2);
```

## 🚀 Endpoints
```TEXT

| Endpoint                                           | Version | Description                          |
|---------------------------------------------------|---------|--------------------------------------|
| `/api/employees/info`                             | v1      | Manual mapping                       |
| `/api/v2/employees/info`                          | v2      | JPA join fetch                       |
| `/api/v3/employees/info?appName=X&page=0`         | v3      | Configurable query + pagination      |

```

## 🧠 Version Explanations

🔹 Version 1 — Manual Mapping

Fetch all tables independently

Join in Java using Map lookups

Pros: Simple, explicit, no entity couplingCons: Manual joins, not scalable, no lazy loading

🔹 Version 2 — JPA Relationships + Join Fetch

Use @OneToOne and @ManyToOne mappings

Use JOIN FETCH in repository

Pros: Clean domain model, declarative joinsCons: Static logic, entity coupling

🔹 Version 3 — Configurable Query via YAML

Externalize filters, joins, and pagination

Resolve page size per app

Pros: Runtime flexibility, multi-tenant supportCons: Requires config discipline

##🏆 Recommendation

Version 3 is the most flexible and production-ready.It supports:

Dynamic filters

Per-app pagination

Join toggles

Clean YAML-driven logic

💡 Optional Version 4 — QueryDSL or Native SQL

For advanced use cases:

Use QueryDSL for type-safe dynamic queries

Use native SQL for performance-critical joins or reporting

## Package Structure
```TEXT

com.aiatit.emp
├── controller
│   └── EmployeeController.java
├── dto
│   └── EmployeeInfo.java
├── entity
│   ├── v1/
│   ├── v2/
│   └── v3/
├── repository
│   ├── v1/
│   ├── v2/
│   └── v3/
├── service
│   ├── EmployeeService.java
│   ├── EmployeeV2Service.java
│   └── EmployeeV3Service.java
└── config
    └── EmployeeQueryConfig.java

```

## 🧭 How to Run

Clone the repo

Run EmployeeApplication.java

Access H2 console at http://localhost:8080/h2-console

Use JDBC URL: jdbc:h2:mem:testdb

Test endpoints via Postman or browser

## Sample json
```json
{
  "empid": "EMP001",
  "empname": "Alice",
  "country": "CA",
  "empdept": "IT"
}
```

## 🙌 Credits

Crafted with clarity, modularity, and design impact in mind.


