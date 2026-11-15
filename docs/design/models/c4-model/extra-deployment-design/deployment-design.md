flowchart LR
    browser["User laptop / phone
    Web browser runs CT1
    HTML / CSS / JavaScript"]

    subgraph vps["Team Linux VPS — planned single-host deployment"]
        direction LR

        nginx["Nginx
        Public HTTPS :443
        Static files and reverse proxy"]

        api["CT2 Application API
        Node.js / Express
        127.0.0.1:3000"]

        db[("CT3 PostgreSQL
        Loopback :5432
        or private socket")]

        nginx -->|"HTTP /api
        Loopback"| api

        api -->|"Parameterized SQL
        Local connection"| db
    end

    firebase["Firebase Authentication
    Google-managed external service"]

    backup["Off-host backup storage
    Restricted database backup files"]

    browser -->|"HTTPS :443
    Load CT1 and call /api"| nginx

    browser -->|"HTTPS
    Sign-in / token refresh"| firebase

    api -->|"HTTPS
    Firebase Admin SDK"| firebase

    db -.->|"pg_dump and
    off-host copy"| backup

    backup -.->|"Restore procedure"| db

    classDef client fill:#eaf2fa,stroke:#456789,color:#172b4d
    classDef server fill:#d6e8fa,stroke:#245b91,stroke-width:2px,color:#172b4d
    classDef external fill:#eeeeee,stroke:#777777,color:#333333
    classDef storage fill:#fff3d6,stroke:#ad852c,color:#493810

    class browser client
    class nginx,api,db server
    class firebase external
    class backup storage

    style vps fill:#ffffff,stroke:#7f9db7,stroke-width:1px