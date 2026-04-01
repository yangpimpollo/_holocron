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
### video 336-340
podemos hacer controladores con `php artisan make:controller PeliculaControlador`
ponemos remplazar en el enrutador
```php
public function index(){ return view('pelicula'); }
-------------------------
Route::get('pelicula', [PeliculaController::class, 'index']);
```
podemos pasar parametros 
```php
public function index($pag){ return view('pelicula',['pag'=>$pag]); }
-------------------------
Route::get('pelicula/{pag?}', [PeliculaController::class, 'index']);
```
podemos un controlador especifico que tiene funciones definidas con  
`php artisan make:controller UserControlador --resource`
donde ya tiene las funciones: index, create, store, show, edit, update, destroy que facilitan un crud
y nos facilita en el enrutador de no poner una ruta por cada función
```php
Route::resource('usuario', [UserControlador::class, 'index']);
```
en los enlaces se puede hacer de 2 maneras
```php
<a href="{{action('[PeliculaController::class, 'index']')}}">
<a href="{{route('pelicula', 'id' => $id)}}">
```
en las redirecciones
```php
return redirect()->route('peliculas');
return redirect()->action('peliculas');
```
el middleware es un intermediario sirve como filtro por ejemplo para validar podemos crearlo por
`php artisan make:middleware CheckAge`
```php
Route::get('/casino', function () { /* ... */ })->middleware(CheckAge::class);

----------------------
// app/Http/Middleware/CheckAge.php
public function handle(Request $request, Closure $next)
{
    if ($request->age <= 18) {
        return redirect('home'); // Bloquea el paso
    }
    return $next($request); // Permite el paso
}
```

## [sec 82] Formularios
### video 341
se puede crear formularios en el blade colocar en action donde desencadena la accion del su envio
```php
<form action="{{ route('posts.store') }}" method="POST">
    @csrf
    <label for="title">Título:</label>
    <input type="text" name="title" value="{{ old('title') }}">
    
    @error('title') <span>{{ $message }}</span> @enderror

    <button type="submit">Guardar</button>
</form>

---------------------------------------
public function recibir(Request $request){
    $title = $request->input('title');          // asi obtenemos el valor por request
}
```
## [sec 83] Base de Datos y QueryBuilder
### video 342-351
para la conección
```ini
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=ejemplo1_laravel
DB_USERNAME=postgres
DB_PASSWORD=root
```
para la migracion, las migraciones son el sistema de control de versiones de la BD
podemos crear con
`php artisan make:migration crear_tabla_animal --table=animal`
nos creara una migracion con una tabla blueprint sin definir
```php
Schema::create('animals', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('pass');
});
```
recordar que up es para hacer y down para deshacer
```bash
php artisan migrate                       migra
php artisan migrate:rollback              deshace la migracion
php artisan migrate:refresh               borra todo empieza migrar de nuevo
```
se puede usar sentencias sql con 
```php
DB::statement(" /*  -- sql --   */  ");
```
los seeders son clases diseñadas para poblar tu base de datos con datos de prueba se crean con
`php artisan make:seeder FrutaSeed`
```php
public function run(): void
    {
        $frutas = [
            ['nombre' => 'Manzana', 'color' => 'Rojo', 'stock' => 50],
            ['nombre' => 'Plátano', 'color' => 'Amarillo', 'stock' => 30],
            ['nombre' => 'Arándano', 'color' => 'Azul', 'stock' => 100],
        ];
  // tambien se puede
  DB::table('frutas')->insert(['nombre' => 'Manzana', 'color' => 'Rojo', 'stock' => 50]);
```
podemos listar datos
```php
public function index(){
    $fruta = DB::table('frutas')->get();
    return view('frutas', ['fruta'  => $fruta]);
}
--------------------------------
<ul>
    @foreach($frutas as $fruta)
    <li>{{ $fruta->name }}</li>
    @endforeach
```

