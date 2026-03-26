 13. Seccion 85 video 358
    - diseñamos la base de datos
```mermaid
erDiagram
    USERS ||--o{ IMAGES : "uploads"
    USERS ||--o{ LIKES : "gives"
    USERS ||--o{ COMMENTS : "writes"
    IMAGES ||--o{ LIKES : "receives"
    IMAGES ||--o{ COMMENTS : "has"

    USERS {
        int id
        string role
        string name
        string surname
        string nick
        string email
        string password
        string image
        datetime created_at
        datetime updated_at
        string remember_token
    }

    IMAGES {
        int id
        int user_id
        string image_path
        string description
        datetime created_at
        datetime updated_at
    }

    LIKES {
        int id
        int user_id
        int image_id
        datetime created_at
        datetime updated_at
    }

    COMMENTS {
        int id
        int user_id
        int image_id
        text content
        datetime created_at
        datetime updated_at
    }
```

