flowchart TB
    student["Student (Person)"]
    admin["Administrator (Person)"]
    room4u["Room4U (Software System)<br/>Simulated library room booking"]
    firebase["Firebase Authentication (External System)<br/>Identity authentication"]

    student -->|"Find rooms; submit, track and cancel own requests"| room4u
    admin -->|"Review requests; manage rooms and maintenance"| room4u
    room4u -->|"Use identity authentication and token verification"| firebase

    classDef person fill:#eaf2fa,stroke:#456789,color:#172b4d
    classDef system fill:#d6e8fa,stroke:#245b91,stroke-width:2px,color:#172b4d
    classDef external fill:#eeeeee,stroke:#777777,color:#333333

    class student,admin person
    class room4u system
    class firebase external