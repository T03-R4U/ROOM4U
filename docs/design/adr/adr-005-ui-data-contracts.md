# ADR-005 Define controlled UI data contracts

| Field | Record |
| --- | --- |
| Status | Proposed. The leader has selected the scope below; the proposed purpose catalogue, equipment catalogue and filter semantics still require team confirmation. No completed team review is claimed. |
| Context | PA2 S03 uses a required purpose dropdown and an optional note with a 250-character counter. S04 and S06 display requester profile fields missing from the initial PA3 schema. S02 and S07 use equipment checkboxes. The leader chooses to retain requester profiles and equipment selection, while removing the separate related-course field and room-area information. |
| Decision | Use stable purpose codes validated by the API and database, with Vietnamese UI labels defined below. Add full_name, student_code and phone to pre-created users. Keep purpose required and note optional with a 250-character limit. Remove related-course and area controls and displays. Represent room equipment in room_equipment rather than an editable delimited description in rooms. Use the shared catalogues below across UI, API, seeds and persistence. |
| Alternatives considered | Unrestricted purpose text preserves flexibility but does not define the dropdown contract. Storing translated labels makes stored values depend on presentation wording. Removing supported profile fields would reduce the PA2 interface unnecessarily. Parsing equipment descriptions to reconstruct checkboxes makes selection and filtering ambiguous. Adding editable purpose/equipment catalogues creates management work outside the current scope. |
| Consequences | The proposed model has seven tables. UI dropdowns, checkbox values, API DTOs, database checks and seed data must remain synchronized. Labels may change without rewriting stable codes. Changing the code catalogue requires a reviewed design/schema update. Equipment joins add persistence work but provide explicit selections and filters. This refines the data representation in ADR-001 without changing its modular-monolith decision. |

## Purpose catalogue proposed for confirmation

| Stored/API code | Vietnamese UI label |
| --- | --- |
| GROUP_STUDY | Học nhóm |
| PROJECT_DISCUSSION | Thảo luận đồ án |
| ASSIGNMENT_WORK | Làm bài tập |
| ACADEMIC_RESEARCH | Nghiên cứu học tập |
| OTHER | Hoạt động học tập khác |

- S03 shows an initially unselected placeholder, “Chọn mục đích sử dụng”. A valid choice is required before submission.
- Requests store and transmit the code, not the Vietnamese label. Unsupported codes and arbitrary text are rejected by the API; the database also constrains the stored code.
- OTHER is itself a valid code. Selecting it does not introduce a new requirement to enter a note; note remains optional and limited to 250 characters.
- Purpose describes the request. It does not introduce approval priority or a new purpose-based eligibility rule.

## Equipment catalogue proposed for confirmation

| Stored/API code | Vietnamese UI label |
| --- | --- |
| WHITEBOARD | Bảng trắng |
| DISPLAY_SCREEN | Màn hình / TV |
| POWER_OUTLET | Ổ cắm điện |
| AIR_CONDITIONER | Điều hòa |
| WIFI | Wi-Fi |

- A selected checkbox sends its code. Repeated codes are not stored twice; unsupported codes are refused.
- An empty selection sends an empty array and produces no room_equipment rows for that room.
- Selecting several equipment filters in S02 means the room must contain all selected codes (AND). Selecting none adds no equipment condition.
- The API returns equipmentCodes and a derived equipmentDescription for the room cards and details. No second editable equipment string is stored in rooms. The generated description remains within the PA2 250-character equipment-description bound.
