# Ejercicios para afianzar Conocimientos crud + mvc

## Ejercicio 1 - crear archivo
1. en linux `/home/yangpimpollo/Workspace` creamos la carpeta `test3`
2. nos movemos a la carpeta `cd test3` ejecutamos `code .` para abrir visual studio code
3. creamos index.php con un hello world!
4. lanzamos el servidor en la carpeta test1 `php -S localhost:8000`
5. visualizamos en la web `http://localhost:8000` 
6. tambien podemos ejecutar en consola `php index.php` para ver en consola

## Ejercicio 2 - conectar con postgresql
1. activamos la BD desde la consola `sudo service postgresql start`
2. se puede verificar si esta activo `sudo service postgresql status`
3. se puede ver si se concta con el siguiente codigo

```php
<?php
    $host = "localhost";
    $db = "proyecto1_db";
    $user = "postgres";
    $pass = "root";

    try {
        $dsn = "pgsql:host=$host;port=5432;dbname=$db;";
        $pdo = new PDO($dsn, $user, $pass, [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]);
        echo "¡Conectado a Postgres en WSL!";
    } catch (PDOException $e) {
        echo $e->getMessage();
    }
?>
```

4. podemos ver que esta habilitado

## Ejercicio 3 - composer
1. en nueva consola ejecutamos `composer init` nos dara la biemvenida
2. ponemos `app/crud` en minusculas sigiente . . . no no src/ yes podemos ver el archivo json

```json
{
    "name": "app/crud",
    "autoload": {
        "psr-4": {
            "App\\Crud\\": "src/"                   //   App\\Crud\\    significa App\Crud   se puede modificar
        }                                           //   "App\\": "src/"     es  App\
    },
```

3. probamos la auto carga de clases creamos Prueba.php dentro de src
```php
<?php

namespace App\Crud;

class Prueba {
    public function saludar() {
        return "¡La autocarga de PSR-4 funciona correctamente!";
    }
}

//--------------------------------------------------------------
require_once __DIR__ . '/vendor/autoload.php';

use App\Crud\Prueba;

$test = new Prueba();
echo $test->saludar();
```
4. creamos `bootstrap.php` insertamos `<?php require_once __DIR__ . '/vendor/autoload.php'; ?>` modificamos en index `require_once __DIR__ . '/bootstrap.php';` recargamos debe mostrar *"¡La autocarga de PSR-4 funciona correctamente!"*.

## Ejercicio 4 - Estructura del Proyecto
```
test3/
├── vendor/     
├── src/                    
│   ├── controller/
│   ├── database/
│   ├── model/
│   ├── view/
│   └── Prueba.php           
├── bootstrap.php        
├── README.md      
├── index.php                
└── composer.json            
```
## Ejercicio 5 - Estructura del Proyecto