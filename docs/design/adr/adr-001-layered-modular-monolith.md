# ADR-001: Use a layered modular monolith

| Field | Record |
| --- | --- |
| Status | Accepted |
| Context | Four students must implement five feature groups within the PA1 schedule. Booking and availability changes need one reliable transactional boundary. |
| Decision | Use a browser client and one Express deployment. Separate HTTP/access control, queries, command services, domain policies and persistence. Keep the six business tables in one PostgreSQL database. |
| Alternatives considered | Microservices: adds distributed authorization, deployment and consistency work. A single unstructured Express file: initially shorter but mixes rules, HTTP and SQL. |
| Consequences | The design provides a small deployment and a local transaction boundary for booking decisions. Separating responsibilities allows services and policies to be verified through injected dependencies. Module boundaries must be maintained during implementation and code review. API modules are deployed together, and the initial single-instance deployment provides no automatic failover. Application and database resource usage must be evaluated on the PA1 reference environment before claiming compliance with the PA2 performance targets. |
