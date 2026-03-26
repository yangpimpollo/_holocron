 13. Seccion 85 video 358
    - diseñamos la base de datos
```mermaid
graph LR
    %% Definición de Estilos
    classDef tabla fill:#fff,stroke:#333,stroke-width:1px,text-align:left;

    %% Nodos con toda la información de la imagen
    USERS["<b>USERS</b><hr/>+id: int<br/>+role: string<br/>+name: string<br/>+surname: string<br/>+nick: string<br/>+email: string<br/>+password: string<br/>+image: string<br/>+created_at: datetime<br/>+updated_at: datetime<br/>+remember_token: string"]:::tabla

    IMAGES["<b>IMAGES</b><hr/>+id: int<br/>+user_id: int<br/>+image_path: string<br/>+description: string<br/>+created_at: datetime<br/>+updated_at: datetime"]:::tabla

    LIKES["<b>LIKES</b><hr/>+id: int<br/>+user_id: int<br/>+image_id: int<br/>+created_at: datetime<br/>+updated_at: datetime"]:::tabla

    COMMENTS["<b>COMMENTS</b><hr/>+id: int<br/>+user_id: int<br/>+image_id: int<br/>+content: text<br/>+created_at: datetime<br/>+updated_at: datetime"]:::tabla

    %% Relaciones Horizontales
    USERS --> IMAGES
    USERS --> LIKES
    USERS --> COMMENTS
    IMAGES --> LIKES
    IMAGES --> COMMENTS
```

