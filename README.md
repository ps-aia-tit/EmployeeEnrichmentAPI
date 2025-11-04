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
)


### Test
