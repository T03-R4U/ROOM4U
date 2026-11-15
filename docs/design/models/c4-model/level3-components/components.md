flowchart LR
    web["CT1 Web interface"]

    subgraph api["CT2 Application API — Node.js / Express"]
        direction LR

        m02["M02 HTTP boundary
        C09 ApiController
        Routing and DTO validation"]

        m01["M01 Access control
        C07 AuthService
        C12 FirebaseGateway"]

        m03["M03 Queries
        C14 QueryService
        Safe response DTOs"]

        m04["M04 Booking commands
        C04 BookingService
        C01 BookingRequest"]

        m05["M05 Room and maintenance
        C05 RoomService
        C06 MaintenanceService"]

        m06["M06 Eligibility policy
        C03 EligibilityPolicy
        Shared UC-14 checks"]

        m07["M07 Persistence boundary
        C08 UnitOfWork
        Repositories and transactions"]

        m02 -->|"Authenticate and check role"| m01
        m02 -->|"Dispatch reads"| m03
        m02 -->|"Dispatch booking commands"| m04
        m02 -->|"Dispatch management commands"| m05

        m01 -->|"Read application role"| m07
        m03 -->|"Read through repositories"| m07

        m04 -->|"Validate eligibility"| m06
        m04 -->|"Load and commit changes"| m07
        m05 -->|"Load and commit changes"| m07
    end

    db[("CT3 PostgreSQL")]
    firebase["Firebase Authentication
    External system"]

    web -->|"HTTPS / JSON + ID token"| m02
    m07 -->|"Parameterized SQL"| db
    m01 -->|"ID token verification
    Firebase Admin SDK"| firebase

    classDef external fill:#eeeeee,stroke:#777777,color:#333333
    classDef component fill:#eaf2fa,stroke:#456789,color:#172b4d
    classDef policy fill:#fff3d6,stroke:#ad852c,color:#493810

    class web,db,firebase external
    class m01,m02,m03,m04,m05,m07 component
    class m06 policy

    style api fill:#ffffff,stroke:#7f9db7,stroke-width:1px