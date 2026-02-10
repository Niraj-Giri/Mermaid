erDiagram

  USERS {
    bigint id PK
    varchar full_name
    varchar email
    varchar phone
    varchar password
    boolean is_active
    boolean is_verified
    timestamp created_at
    timestamp updated_at
    bigint role_id FK
  }

  ROLES {
    bigint id PK
    varchar name
  }

  AUTH_CREDENTIALS {
    bigint user_id PK, FK
    varchar password_hash
    timestamp last_login
    int failed_attempts
  }

  CREW_DETAILS {
    bigint id PK
    bigint user_id FK
    varchar department
    varchar specialization
    bigint availability_status_id FK
    timestamp created_at
  }

  AVAILABILITY_STATUSES {
    bigint id PK
    varchar name
  }

  LOCATIONS {
    bigint id PK
    double latitude
    double longitude
    varchar address_line
    varchar area
    varchar landmark
    varchar city
    varchar state
    varchar pincode
    varchar geohash
    timestamp created_at
  }

  COMPLAINTS {
    bigint id PK
    varchar title
    varchar description
    varchar ml_detected_category
    boolean is_ml_verified
    boolean is_deleted
    int vote_count
    timestamp created_at
    timestamp updated_at
    timestamp resolved_at
    timestamp sla_deadline
    bigint citizen_id FK
    bigint category_id FK
    bigint status_id FK
    bigint location_id FK
    bigint get_assigned_crew_id FK
  }

  COMPLAINT_CATEGORIES {
    bigint id PK
    varchar name
    varchar description
    boolean is_active
  }

  COMPLAINT_STATUSES {
    bigint id PK
    varchar name
  }

  COMPLAINT_STATUS_HISTORY {
    bigint id PK
    bigint complaint_id FK
    bigint status_id FK
    bigint changed_by FK
    varchar remarks
    timestamp changed_at
  }

  COMPLAINT_ASSIGNMENTS {
    bigint id PK
    bigint complaint_id FK
    bigint crew_id FK
    bigint assigned_by FK
    varchar assignment_status
    timestamp assigned_at
    timestamp completed_at
  }

  COMPLAINT_IMAGES {
    bigint id PK
    bigint complaint_id FK
    varchar image_path
    varchar image_type
    bigint uploaded_by
    timestamp uploaded_at
  }

  AUDIT_LOGS {
    bigint id PK
    bigint user_id FK
    varchar action
    varchar entity_type
    bigint entity_id
    timestamp created_at
  }

  %% ===== RELATIONSHIPS =====

  ROLES ||--o{ USERS : "assigned to"

  USERS ||--|| AUTH_CREDENTIALS : "has"

  USERS ||--|| CREW_DETAILS : "is"

  AVAILABILITY_STATUSES ||--o{ CREW_DETAILS : "defines"

  USERS ||--o{ COMPLAINTS : "raises"
  USERS ||--o{ COMPLAINTS : "assigned crew"

  COMPLAINT_CATEGORIES ||--o{ COMPLAINTS : "categorizes"
  COMPLAINT_STATUSES ||--o{ COMPLAINTS : "current status"
  LOCATIONS ||--o{ COMPLAINTS : "located at"

  COMPLAINTS ||--o{ COMPLAINT_STATUS_HISTORY : "status history"
  COMPLAINT_STATUSES ||--o{ COMPLAINT_STATUS_HISTORY : "status"
  USERS ||--o{ COMPLAINT_STATUS_HISTORY : "changed by"

  COMPLAINTS ||--o{ COMPLAINT_ASSIGNMENTS : "assigned"
  USERS ||--o{ COMPLAINT_ASSIGNMENTS : "crew"
  USERS ||--o{ COMPLAINT_ASSIGNMENTS : "assigned by"

  COMPLAINTS ||--o{ COMPLAINT_IMAGES : "has"
  USERS ||--o{ COMPLAINT_IMAGES : "uploads"

  USERS ||--o{ AUDIT_LOGS : "creates"
