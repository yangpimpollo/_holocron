


1. video 1: intro
    - programación desde cero
    - php moderno
    - BD y sql
    - maquetacion web css
    - poo y mvc
    - librerias 
    - laravel y symfony

2. video 2: repaso html
    - que es html estructura, etiqueta, listas, imagenes, tablas, formulario
    - pasar a php

3. video 3 - 13: etiquetas de texto
    - etiqueta <strong>importante</strong> <b>negrita</b> <em>enfatiza</em> <i>cursiva</i> <s>tachado</s> <mark>resaltado</mark>
    - revisar las etiquetas ... <em><mark>PENDIENTE</mark></em>

4. video 14 - 54: intro a php hasta la seccion 14_funciones
    - var, tipos, const, operadores, condicionale, bucles, funciones, includes, array
    - constantes: `define('PI', 3.14)`  se llama PI sin el $ `echo "valor: ".PI` o const PI = 3.14159;
    - constantes predefinidas PHP_VERSION, PHP_OS, PHP_EXTENSION_DIR, __ FILE__ , __ LINE__ , __ FUCTION__
    - metodo get estoy enviando a submit.php llenamos Rodrigo Perez
    - me dara http://127.0.0.1:8000/submit.php?name=Rodrigo&lastname=Perez 
    - usamos `$_GET['name']` ojo que el identificador es el atributo `name`, el atributo `id` es para css y javascript
    ```php
    <form action="submit.php" method="get">
        <input type="text" id="name" name="name" placeholder="Enter your name">
        <br>
        <input type="text" id="lastname" name="lastname" placeholder="Enter your last name">
        <br>
        <input type="submit" value="Submit">
    </form>

    ----  submit.php  ---------------------------

    <h1>Welcome <?php echo $_GET['name']; ?> <?php echo $_GET['lastname']; ?></h1>
    ```
    - saltarse lineas <mark>goto</mark>de ejecución `goto name; /* cadigo a saltar ... */  name` ideal para debug

5. video 63-64: 
    - se puede crear una cabecera.php   poner el html y poner un include en la otras paginas para no repetir codigo `<?php include 'cabecera.php'; ?>` o `<?php include 'footer.php'; ?>` puede usar include_once para que cargue solo una vez se puede usar `require`
6. video 75 - 77: php en profundidad seccion 18
    - sesiones, cookies, form, validacion, archivos,ficheros
    ```php
    session_start();                                             // iniciar seccion

    $normal_var = "normal variable";
    $_SESSION['persistent_var_'] = "persistent variable";        // aqui creamos la var persistente

    ----  submit.php  ---------------------------
    session_start();                                             // cada archivo debemos iniciar seccion
    echo "This is $_SESSION[persistent_var_].";                  // podemos ver la variable
    session_destroy();                                           // si cerramos ya no podemos ver la variable
    ```
    - PHP es "olvidadizo". cuando cargo una página nueva, PHP trata como si fuera la primera vez; no mantiene una conexión abierta con el navegador. Al ejecutar session_start() busca la "cookie de sesión" en el navegador del usuario recostruye a base de la información guardada en $_SESSION Si lo olvidas en una página, $_SESSION aparecerá vacío.
    - <mark>Cookie</mark> es un fichero que se almacena en el equipo de usuario para recordar el comportamiento del mismo en la web
    ```php
    // setcookie( $name, $value = "", $expires = 0, $path = "", $domain = "", $secure = false, $httponly = false): bool

    setcookie("cookie1", "cookie1_dat")                     // cookie de 1 año time()+(60*60*24*365)

    if(isset($_COOKIE['cookie1'])) echo $_COOKIE['cookie1'];           //  > cookie1_dat

    if(isset($_COOKIE['cookie1'])){                //  eliminar cookie caducandolas
        unset($_COOKIE['cookie1']);                // borra la variable de la memoria actual de PHP
        setcookie('cookie1','',time()-100)         // cookie caduco hace 100s
    }
    ```

7. video 85
    - leer escribir ficheros, subir archivos
    ```php
    <?php

    $archivo fopen("text_file.txt", "r");     // r:read  w:write   x:create   a+=read and write

    while(!feof ($archivo)) (
        $contanido fgets ($archivo);
        echo $content."<br/>";
    }
    
    fwrite($file, "I'm text inserted from PHP");    // para escribir debe cambia permiso a a+
    fclose ($Archivo);
    ```

8. video 95: seccion 25 base de datos sql
    - intro, tablas, diseño, insert-update-select-delete, funciones, agrupamiento, subconsulta, multitabla-join
    ```sql
    USE bd_name;

    CREATE TABLE users (
	    id INTEGER PRIMARY KEY AUTOINCREMENT,
	    firstname VARCHAR(100),
	    lastname VARCHAR(100),
	    mail VARCHAR(100),
	    pass VARCHAR(100) DEFAULT '123'
	);

    DROP TABLE users;

    ALTER TABLE users RENAME TO all_users;
    ALTER TABLE all_users CHANGE lastname lastname1 VARCHAR(50);
    ```
    - el script es ligeramente distinto en cada SGBD dis diagram editor


9. video 218: POO en PHP sección 51


    <mark>PENDIENTE</mark>

10. video 319: sección 77 LARAVEL
    - intro, routing, pantillas-vistas, controladores, form, BD-querybuilder, eloquent, entidades modelos
    - instalacion
    ```bash
    composer global require laravel/installer
    
    cd Workspace/Laravel_
    laravel new example-app
    cd example-app
    npm install && npm run build
    composer run dev
    ```
11. video 324: Rutas
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

    en consola `php artisan route:list` ver todas las rutas
    en consola `php artisan make:controller Controller1` crear controlador

    ```php
    Route::get('/pagina1/{title?}', function ($title = 'default title') {
        return view('pagina1', ['title' => $title]);
    });

    //  /pagina sin nada default title     /pagina/-palabra-    aperecera -palabra-    colocar title ? hace opcional

    ---------------------------------------------------

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
           
 12. video 336: controladores seccion 81                                                               
    - <mark> PENDIENTE </mark>  controladores middlewares seccion 81                         
    - <mark>PENDIENTE</mark>  formularios seccion 82                                                      
    - <mark>PENDIENTE</mark>  query builde BD seccion 83                                                        
    - intro 1° proyecto              

 13. video 361: ORM seccion 85
    - s












