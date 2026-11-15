# ADR-003: Use Firebase identity with application role checks

| Field | Record |
| --- | --- |
| Status | Accepted |
| Context | PA1 uses Firebase Authentication and pre-created accounts. Room4U still owns Student/Admin permissions and request ownership; there is no university SSO integration. |
| Decision | Use the Firebase browser SDK for email/password sign-in. The API verifies bearer ID tokens through Firebase Admin SDK, looks up firebase_uid in users and derives the role from that database row. The browser cannot supply a trusted role or owner ID. |
| Alternatives considered | A custom password database: unnecessary credential-handling work. Client-side role checks only: bypassable. University SSO: outside the committed scope. |
| Consequences | Passwords do not enter the Room4U API or database. Identity verification depends on Firebase configuration and network/key availability. Sign-out clears local identity/private views; it does not promise revocation of a copied token. No public registration or recovery screen is added. |
