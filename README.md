# 🧩 Employee Enrichment API — Spring Boot (JPA, H2, Configurable Query)

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


---

✅ H2 Setup and Schema

`application.yaml`

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


SQL Schema

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


Sample Data

INSERT INTO employee VALUES ('EMP001', 'Alice');
INSERT INTO employee VALUES ('EMP002', 'Bob');

INSERT INTO empdept VALUES (1, 'IT');
INSERT INTO empdept VALUES (2, 'HR');

INSERT INTO empdetail VALUES ('EMP001', 'CA', 1);
INSERT INTO empdetail VALUES ('EMP002', 'US', 2);


---

🚀 Endpoints

Endpoint	Version	Description	
/api/employees/info	v1	Manual mapping	
/api/v2/employees/info	v2	JPA join fetch	
/api/v3/employees/info?appName=X&page=0	v3	Configurable query + pagination	


---

🧠 Version Explanations

🔹 Version 1 — Manual Mapping

Approach:
Fetch all tables independently and join in Java using Map lookups.

Pros:

• Simple and explicit
• Easy to debug
• No entity coupling


Cons:

• Manual join logic
• No lazy loading
• Not scalable for large datasets


Best for: quick prototypes, legacy systems, or when JPA is not feasible

---

🔹 Version 2 — JPA Relationships + Join Fetch

Approach:
Use @OneToOne and @ManyToOne mappings with JOIN FETCH in repository.

Pros:

• Clean domain model
• Declarative joins
• Reusable relationships


Cons:

• Static join logic
• Entity coupling
• Harder to customize at runtime


Best for: clean domain-driven design, internal APIs

---

🔹 Version 3 — Configurable Query via YAML

Approach:
Externalize filters, joins, and pagination in application.yaml. Resolve page size per app.

Pros:

• Runtime flexibility
• Per-app pagination
• Clean separation of config and logic


Cons:

• Slightly more setup
• Requires config discipline


Best for: multi-tenant APIs, configurable platforms, analytics dashboards

---

🏆 Recommendation

Version 3 is the most flexible and production-ready.
It supports:

• Dynamic filters
• Per-app pagination
• Join toggles
• Clean YAML-driven logic


---

💡 Optional Version 4 — QueryDSL or Native SQL

For advanced use cases:

• Use QueryDSL for type-safe dynamic queries
• Use native SQL for performance-critical joins or reporting


---

📁 Package Structure

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


---

🧭 How to Run

1. Clone the repo
2. Run EmployeeApplication.java
3. Access H2 console at http://localhost:8080/h2-console
4. Use JDBC URL: jdbc:h2:mem:testdb
5. Test endpoints via Postman or browser


---

🙌 Credits

Crafted with clarity, modularity, and recruiter impact in mind.
