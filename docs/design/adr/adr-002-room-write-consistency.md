# ADR-002: Serialize room writes and share one occupancy constraint

| Field | Record |
| --- | --- |
| Status | Accepted |
| Context | Two approvals, maintenance creation, suspension and capacity edits must make consistent decisions even when the room initially has no occupied interval. |
| Decision | At READ COMMITTED, lock the persistent rooms row before loading current decision data. Use the same lock for every command affecting that room. Represent Approved bookings and active maintenance in room_occupancy, protected by a room/range GiST exclusion constraint. |
| Alternatives considered | Check then write without a lock: both requests can pass. Lock only overlapping booking rows: locks no guard when the set is empty. An in-memory mutex: does not protect another API process. SERIALIZABLE with retries: viable, but more retry handling than the selected explicit room guard. |
| Consequences | Different rooms can progress concurrently; writes for one room wait for each other. The occupancy projection adds transactional maintenance work and btree_gist must be available. Centralized write paths, source foreign keys and rollback checks are required. Validate the migration and races in PA5. |
