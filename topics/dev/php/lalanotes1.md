# Notas de Aprendizaje Laravel

## Notas Previas
-----
revisar el roadmap `https://roadmap.sh/laravel` propuesto po Grok tambien o Gemini o Claude
-----
## Ejercicio 1: Instalación
Instrucciones dadas por Grok

1. En Workspace creamos la carpeta `Laravel_` donde estara todos nuestros trabajos relacionados con el framework
2. nos dirigimos a ella por consola `cd Workspace/Laravel_` ejecutamos `composer create-project laravel/laravel mi-primera-pagina` composer automaticamente creara el proyecto con sus dependencias.
3. El proyecto se llama `mi-primera-pagina` en este caso nos dirigimos a ella `cd mi-primera-pagina`
4. lansamos el servidor ejecutando `php artisan serve` nos dirigimos a `http://127.0.0.1:8000` podemos ver la pagina de bienvenida de Laravel



## Ejercicio 2: Primera pagina
1. Abrir el archivo `routes/web.php`
```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () { return view('welcome'); });
Route::get('/saludo', function () { return view('saludo'); });
```
2. En `resources/views/` crear nuevo archivo `saludo.blade.php` y probar `http://127.0.0.1:8000/saludo`
```php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Mi Primera Página en Laravel</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 100px;
            background: #f0f8ff;
        }
        h1 { color: #1e90ff; }
    </style>
</head>
<body>
    <h1>¡Hola Mundo! 👋</h1>
    <p>Esta es mi primera página creada con Laravel</p>
    <p>Estoy muy feliz de estar aquí ❤️</p>
    
    <a href="/">Volver al inicio</a>
</body>
</html>
```

## Ejercicio 3: Pasar datos del controlador a la vista
1. ejecutamos `php artisan make:controller SaludoController` creara automaticamente un controlador en app\Http\Controllers colocamos
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class SaludoController extends Controller
{
    public function index()
    {
        $nombre = "fulano de tal";
        $edad = 800;
        $pais = "Naboo";
        $habilidades = ["volar", "magia", "herrero", "cocinar"];

        $fecha = now()->format('d/m/Y H:i');

        return view('saludo', compact(
            'nombre', 
            'edad', 
            'pais', 
            'habilidades', 
            'fecha'
        ));
    }
}
```
en saludo.blade.php colocamos
```php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Primera Página con Datos</title>
    <style>
        body {
            font-family: 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            text-align: center;
            padding: 50px 20px;
            min-height: 100vh;
        }
        .card {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            max-width: 600px;
            margin: 0 auto;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        h1 { font-size: 3rem; margin-bottom: 10px; }
        .info { font-size: 1.3rem; margin: 15px 0; }
        ul {
            list-style: none;
            padding: 0;
        }
        li {
            background: rgba(255,255,255,0.2);
            margin: 8px auto;
            padding: 12px;
            border-radius: 10px;
            max-width: 300px;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>¡Hola {{ $nombre }}! 👋</h1>
        
        <p class="info">Hoy es: <strong>{{ $fecha }}</strong></p>
        
        <p class="info">Tengo {{ $edad }} años y soy de <strong>{{ $pais }}</strong></p>
        
        <h2>Mis habilidades actuales:</h2>
        <ul>
            @foreach ($habilidades as $habilidad)
                <li>{{ $habilidad }}</li>
            @endforeach
        </ul>

        <a href="/" style="color: white; margin-top: 30px; display: inline-block;">
            ← Volver al inicio
        </a>
    </div>
</body>
</html>
```
2. en `routes/web.php` aqui es donde el view se conecta con el controlador al poner {{ $variable }} en view aparecera 
por que el controlador lo renderizo hay 3 maneras por compact, with, array asociativo
```php
use App\Http\Controllers\SaludoController;

Route::get('/saludo', [SaludoController::class, 'index']);
```


## Ejercicio 4: Layouts con Blade (Plantilla Maestra)

1. Crear una nueva carpeta dentro de resources/views/ llamada `layouts` crea el archivo `app.blade.php`
```php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Mi App Laravel')</title>
    
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            background: #f4f7fa;
            color: #333;
        }
        .header {
            background: #1e3a8a;
            color: white;
            padding: 20px 0;
            text-align: center;
        }
        .container {
            max-width: 1100px;
            margin: 30px auto;
            padding: 0 20px;
        }
        .nav {
            background: white;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            padding: 15px 0;
            margin-bottom: 30px;
        }
        .nav a {
            margin: 0 15px;
            text-decoration: none;
            color: #1e3a8a;
            font-weight: 500;
        }
        .nav a:hover {
            color: #3b82f6;
        }
        .footer {
            text-align: center;
            padding: 20px;
            background: #1e3a8a;
            color: white;
            margin-top: 50px;
        }
    </style>
</head>
<body>

    <header class="header">
        <h1>Mi Primera Aplicación Laravel</h1>
    </header>

    <nav class="nav">
        <div class="container">
            <a href="/">Inicio</a>
            <a href="/saludo">Saludo</a>
            <!-- Aquí irán más enlaces después -->
        </div>
    </nav>

    <div class="container">
        @yield('content')
    </div>

    <footer class="footer">
        <p>Desarrollado con Laravel - {{ date('Y') }}</p>
    </footer>

</body>
</html>
```

2. Modificar tu vista saludo.blade.php
```php
@extends('layouts.app')

@section('title', 'Saludo - Mi App')

@section('content')

    <div style="background: white; padding: 40px; border-radius: 15px; box-shadow: 0 5px 20px rgba(0,0,0,0.1);">
        <h1>¡Hola {{ $nombre }}! 👋</h1>
        
        <p style="font-size: 1.3rem;">Hoy es: <strong>{{ $fecha }}</strong></p>
        
        <p style="font-size: 1.3rem;">Tengo {{ $edad }} años y soy de <strong>{{ $pais }}</strong></p>
        
        <h2>Mis habilidades actuales:</h2>
        <ul style="list-style: none; padding: 0;">
            @foreach ($habilidades as $habilidad)
                <li style="background: #eff6ff; margin: 8px 0; padding: 12px; border-radius: 8px;">
                    {{ $habilidad }}
                </li>
            @endforeach
        </ul>

        <a href="/" style="display: inline-block; margin-top: 30px; color: #1e3a8a;">
            ← Volver al inicio
        </a>
    </div>

@endsection
```
¿Qué pasó aquí?

- @extends('layouts.app') → Le dice a esta vista: "usa el layout app.blade.php como base"
- @section('title', '...') → Rellena el título de la página
- @section('content') ... @endsection → Todo lo que pongas aquí va dentro del 
- @yield('content') del layout



## Ejercicio 5: Formularios + Recibir datos del usuario


         
















