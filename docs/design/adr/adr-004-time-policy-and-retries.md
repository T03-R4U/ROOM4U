# ADR-004: Keep time policy centralized and retries explicit

| Field | Record |
| --- | --- |
| Status | Accepted |
| Context | Booking rules use local library hours, a rolling seven-day limit and duration input. Stale schedules and lost responses must not be interpreted as successful decisions. |
| Decision | Store instants as timestamptz, display Asia/Ho_Chi_Minh, and derive endAt from start plus duration. Evaluate decision time after obtaining the room lock. Use stable error codes and a client-generated submission key for each logical create-request command. |
| Alternatives considered | Store local timestamp strings: ambiguous comparisons. Accept end time as an independent booking input: changes the current PA2 form. Blindly retry POST with a new key: can create duplicate requests. |
| Consequences | The clock can be fixed for boundary verification. The submission key is a technical retry mechanism, not a new booking feature. Same owner/key and same payload returns the existing request; a reused key with different input is refused. Implementation must preserve these agreed time and replay semantics across the API, persistence and user interface. |
