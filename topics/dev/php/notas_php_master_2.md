 13. Seccion 85 video 358
    - diseñamos la base de datos
```mermaid
graph LR
    %% Definición de las Tablas
    USERS[USERS]
    IMAGES[IMAGES]
    LIKES[LIKES]
    COMMENTS[COMMENTS]

    %% Relaciones Horizontales
    USERS --> IMAGES
    USERS --> LIKES
    USERS --> COMMENTS
    IMAGES --> LIKES
    IMAGES --> COMMENTS
```

