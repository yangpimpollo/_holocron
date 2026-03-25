


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
    - leer escribir ficheros
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

