# Notas del tutorial version Limpia 
# [ secciones 77 - 83 ] intro a Laravel

* [ sec 77 ] intro (319)
* [ sec 78 ] primeros pasos (320 - 323)
* [ sec 79 ] routing (324-328)
* [ sec 80 ] View layouts (329-335) 
* [ sec 81 ] controller (336-340)
* [ sec 82 ] Form (341)
* [ sec 83 ] Query Builder (342-351) 

## [sec 77] intro

### video 319 
lo que se va a ver
1. primeros pasos
2. routing
3. view y layouts
4. controllers
5. form
6. data base
7. Proyecto
8. modelos y entidades
9. login y registro
10. configuracion de usuario
11. imagen y comentario
12. sistema de likes
13. perfil de usuario
14. buscardor

## [sec 78] primeros pasos
### video 320-323
abarca de la instalación y la estructura de un proyecto laravel 
aqui incluyo mis pasos de creacion de proyecto de laravel
0. instalar globalmente Laravel `composer global require laravel/installer`
1. ejecutar los siguientes comandos y creamos nuestro proyecto `ejemplo1_laravel`
```bash
yangpimpollo@PC-VONEX-086:~/Workspace$ laravel new ejemplo1_laravel
yangpimpollo@PC-VONEX-086:~/Workspace$ cd e*
yangpimpollo@PC-VONEX-086:~/Workspace/ejemplo1_laravel$ npm install && npm run build
yangpimpollo@PC-VONEX-086:~/Workspace/ejemplo1_laravel$ sudo service postgresql start
yangpimpollo@PC-VONEX-086:~/Workspace/ejemplo1_laravel$ php artisan serve
```
2. modificar para conectar con la BD
```ini
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=ejemplo1_laravel
DB_USERNAME=postgres
DB_PASSWORD=root
```
la estructura de un proyecto abarca
```
ejemplo1_laravel/
├── app/                         
│   ├── http/Contollers/         controladores
│   ├── Models/                  modelos
│   ├── ...
├── database/ 
│   ├── factories/
│   ├── migrations/              migraciones
│   ├── seeders/
├── resources/ 
│   ├── css/
│   ├── js/
│   ├── views/                   vista
├── routes/ 
│   ├── web.php/                 enrutador
└── ...      
```
## [sec 79] routing
### video 324-328
metodos http get(conseguir), post(guardar), put(actualizar recursos), delete(eliminar recursos)

```php
Route::get('/pagina1', function() {
    $dat1 = "Hola Mundo 8 8 8";
    return view('pagina1', compact('dat1'));
});
----  pagina1.blade.php  ---------------------------    

<?php echo $dat1; ?>
<h1><mark>{{ $dat1 }}</mark></h1>
```
le pasamos el dato a pagina1 para php `$` y para html `{{}}`
```php
Route::get('/pagina1/{title?}', function($title="default title") {
    return view('pagina1', array('title'=>$title));
});
```

```php
Route::get('/pagina1/{title?}', function ($title = 'default title') {
    return view('pagina1', ['title' => $title]);
});

    //  /pagina sin nada default title     /pagina/-palabra-    aperecera -palabra-    colocar title ? hace opcional
```
se puede pasar parametros por compact, array o with
```php
Route::get('', ...)->where(array(
    'titulo' => '[a-zA-Z]+',                // minusculas , mayusculas + se puede repetir
    'year' => '[0-9]+'                      // numeros + se puede repetir
));
```

comandos *ARTISAN*
```bash
php artisan help
php artisan list
php artisan route:list
php artisan make:controller TestController
```

## [sec 80] View layouts
### video 329-335

views/peliculas/listado.blade.php

```php
Route::get('listado.peliculas', function(){
    $titulo = "listado de peliculas";
    $listado = array("p1","p2","p3");
    return view('peliculas.listado')>with('titulo', $titulo)->with('listado', $listado);
});

<h1><?= $titulo ?></h1>
<h1><?= $listado[2] ?></h1>
```

```php
{{-- comentario blade --}}

    <?= isset($var1) ? $var1 : 'variable no existe' ?> 
    {{ $var1 ?? 'variable no existe' }}      // hacer de 2 maneras

    // condicioanles
    @if(isset($var)) <h1> {{$var1}} </h1> @else <h1> variable no existe </h1> @endif    //@elseif()
    // bucles
    @for($i = 0; $i< 5; $i++) {{$i}} @endfor
    <hr/>
    @php $var1 = 0; @endphp
    @while($var1 < 5) {{$var1}} </br> @php $var1++ @endphp @endwhile

    <hr/>
    @php $array = ['Uno', 'Dos', 'Tres']; @endphp
    @foreach($array as $item) {{$item}} </br> @endforeach
```
> variable no existe                                                               
    0 1 2 3 4                                                                        
    <hr/>                               
    0                                                                            
    1                                                                                      
    2                                                                                                         
    3                                                                                              
    4                                                                                
    <hr/>                                                            
    Uno                                                                                 
    Dos                                                                               
    Tres

```php
-------- layouts/app.blade.php -----------------------------------------
<html>
<body>
    <nav>Menú de navegación</nav>   
    @yield('content') {{-- Aquí se inyectará el contenido --}}
    <footer>Pie de página</footer>
</body>
</html>
-------- pagina1.blade.php --------------------------------------------
@extends('layouts.app') {{-- "Quiero usar el diseño general" --}}
@section('content')
    <h1>Mi Título</h1>
        
     @include('partials.boton-social') {{-- "Inserta el botoncito aquí" --}}
@endsection
```


## [sec 81] Controllers
### video 329-335

















