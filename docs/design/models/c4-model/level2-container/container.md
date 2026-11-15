flowchart LR
    student["Student
    (Person)"]

    admin["Administrator
    (Person)"]

    subgraph room4u["Room4U system boundary"]
        direction LR

        web["CT1 Web interface
        HTML / CSS / JavaScript
        Browser; screens S01-S08"]

        api["CT2 Application API
        Node.js / Express
        Access control and business rules"]

        db[("CT3 Business database
        PostgreSQL
        Business records and occupancy")]

        web -->|"HTTPS / JSON
        ID token"| api

        api -->|"Parameterized SQL
        Transactions"| db
    end

    firebase["Firebase Authentication
    (External System)"]

    student -->|"Uses"| web
    admin -->|"Uses"| web

    web -->|"Sign-in / refresh
    Browser SDK"| firebase

    api -->|"Verify ID token
    Admin SDK"| firebase

    classDef person fill:#eaf2fa,stroke:#456789,color:#172b4d
    classDef container fill:#d6e8fa,stroke:#245b91,stroke-width:2px,color:#172b4d
    classDef external fill:#eeeeee,stroke:#777777,color:#333333

    class student,admin person
    class web,api,db container
    class firebase external

    style room4u fill:#ffffff,stroke:#7f9db7,stroke-width:1px