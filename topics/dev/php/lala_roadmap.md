# 🚀 Roadmap de Aprendizaje Laravel (2026)

## Nivel 1: Fundamentos (1 semana)
- Rutas (`routes/web.php`)
- Controladores (`php artisan make:controller`)
- Vistas con Blade (`.blade.php`)
- Pasar datos del controlador a la vista
- Blade Templates básico:
  - `@if`, `@foreach`, `@extends`, `@section`, `@yield`, `@include`
- Formularios y protección CSRF
- Layouts (plantilla maestra)

**Proyecto recomendado:**  
**Todo List simple** (sin base de datos todavía)

---

## Nivel 2: Base de Datos y Eloquent (2-3 semanas)
- Migraciones (`php artisan make:migration`)
- Modelos (`php artisan make:model`)
- Eloquent ORM (CRUD básico)
- Seeders y Factories
- Relaciones entre tablas:
  - Uno a Uno
  - Uno a Muchos
  - Muchos a Muchos
- Consultas avanzadas con Eloquent

**Proyecto recomendado:**  
**Blog simple** o **CRUD completo de Posts**

---

## Nivel 3: Autenticación y Autorización (1-2 semanas)
- Instalación de Breeze o Jetstream
- Login, Registro, Recuperación de contraseña
- Middleware
- Gates y Policies (permisos)
- Verificación de email

**Proyecto recomendado:**  
Extender el Blog para que solo usuarios autenticados puedan crear/editar posts

---

## Nivel 4: Intermedio (2-3 semanas)
- Validaciones de formularios (`$request->validate()`)
- Manejo de archivos (subir imágenes, `Storage`)
- Paginación
- Colecciones y Helpers
- Envío de correos (Mail)
- Notificaciones
- Queues (colas de trabajo)
- Eventos y Listeners
- Testing básico

**Proyecto recomendado:**  
Sistema de gestión de proyectos o Tienda online pequeña

---

## Nivel 5: Avanzado / Full-Stack (3+ semanas)
- Creación de APIs REST (Laravel como backend)
- Laravel Sanctum o Passport (autenticación API)
- Inertia.js + Vue/React (o Livewire)
- Optimizaciones:
  - Eager Loading (evitar N+1)
  - Caché
  - Query Optimization
- Testing avanzado
- Despliegue (Forge, Vapor, Ploi, Railway, etc.)
- Laravel Octane (opcional)

**Proyecto final recomendado:**  
Aplicación completa tipo:
- Red social
- Marketplace
- Sistema de reservas
- Dashboard administrativo

---

## Comandos Artisan más usados

```bash
php artisan make:controller NombreController
php artisan make:model Nombre -m
php artisan make:migration crear_tabla_nombre
php artisan make:request NombreRequest
php artisan make:seeder NombreSeeder
php artisan migrate
php artisan db:seed
php artisan serve
php artisan storage:link