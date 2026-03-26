# Ejercicios para afianzar Conocimientos crud + mvc

## Ejercicio 1 - crear archivo
1. en linux `/home/yangpimpollo/Workspace` creamos la carpeta `test3`
2. nos movemos a la carpeta `cd test3` ejecutamos `code .` para abrir visual studio code
3. creamos index.php con un hello world!
4. lanzamos el servidor en la carpeta test1 `php -S localhost:8000`
5. visualizamos en la web `http://localhost:8000` 
6. tambien podemos ejecutar en consola `php index.php` para ver en consola
7. subimos a gitHub
```bash
git init
git add .
git commit -m "Mi primer commit"
git branch -M main
git remote add origin https://github.com/yangpimpollo/test3.git
git push -u origin main

git clone git@github.com:yangpimpollo/test3.git
```
8. al clonar de gitHub tener en cuenta que ya no se tendra el archivo `.env` ni `vendor`, la primera se crea manualmente y el otro 
 ejecutamos `composer install` buscara el archivo composer.lock e instala las versiones exactas de las librerías.


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

## Ejercicio 4 - Configuración de variables de entorno
1. creamos archivos `.env`, `.env.example`, `.gitignore` dentro del ultimo ponemos `/vendor` y `.env` para no subir a remoto dentro de .env colocamos nuestras variables de entorno, ponemos uno de prueba `SECRET_TEXT="Hello yang pimpollo, this is a secret message!"`.
   
2. instalamos vlucas para *leer archivos .env* ejecutando en consola `composer require vlucas/phpdotenv` vemos que se añadio a composer.json
3. para usar vamos a index.php y ponemos ver impreso la variable. una vez que se carga `$dotenv->load()`se cargan en la memoria no es necesario volver a cargarlas se podrian usar en cualquierparte

```php
$dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
$dotenv->load();
echo $_ENV['SECRET_TEXT'] . "\n";
```
## Ejercicio 5 - Archivo de arranque bootstrap.php
1. archivo encargado de preparar el entorno creamos `bootstrap.php` y ponemos todo lo necesario en index en ella y en index colocamos para llamar a bootstrap
```php
# bootstrap.php -----------------------------------------------------------------
<?php
require_once __DIR__ . '/vendor/autoload.php';
$dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
$dotenv->load();

# index.php ---------------------------------------------------------------------
<?php
require_once __DIR__ . '/bootstrap.php';
use Yangpimpollo\Crud\Prueba;

$test = new Prueba();
echo $test->saludar();

# src/Prueba.php ----------------------------------------------------------------
<?php

namespace Yangpimpollo\Crud;

class Prueba {
    public function saludar() {
        echo $_ENV['SECRET_TEXT'] . "\n";
        return "¡La autocarga de PSR-4 funciona correctamente!";
    }
}
```
> Hello yang pimpollo, this is a secret message! ¡La autocarga de PSR-4 funciona correctamente!

## Ejercicio 6 - Estructura del Proyecto
```
test3/
├── vendor/     
├── src/                    
│   ├── controller/
│   ├── database/
│   ├── model/
│   ├── view/
│   └── Prueba.php
├── .env
├── .env.example
├── .gitignore
├── bootstrap.php        
├── composer.json
├── composer.lock    
├── index.php                
└── README.md        
```

## Ejercicio 7 - Clase Connection.php
1. Creamos `Connection.php` dentro de `src/database` usaremos el patron singleton 
```php
<?php

namespace Yangpimpollo\Crud\database;

use PDO;
use PDOException;

class Connection {
    // Aquí guardaremos la única instancia de PDO como fuciona singleton
    //      1. código pide la instancia
    //      2. revisa $instance es nulo?
    //      3. nulo, crea el new PDO y guarda
    //      4. Si ya tiene, devuelve lo que guardó antes
    // No importa cuántas veces llames a getInstance(), la conexión a Postgres solo se hace UNA vez.
    //      $db1 = Connection::getInstance();
    //      $db2 = Connection::getInstance();         ambos son lo mismo   $db1 === $db2


    // se puede de esta manera:      private static $instance = null; 
    // pero esta es mas rigurosa declaración de tipo (Type Hinting)


    private static ?PDO $instance = null;  // unica instancia
    private function __construct() {}      // Constructor privado: impide crear la instancia con 'new' desde fuera
    private function __clone() {}          // evitar clonación
    public function __wakeup() { throw new \Exception("No se permite deserializar la conexión"); }

    public static function getInstance(): PDO {
        if (self::$instance === null) {
            try {
                $host = $_ENV['DB_HOST'];
                $db   = $_ENV['DB_DATABASE'];
                $user = $_ENV['DB_USERNAME'];
                $pass = $_ENV['DB_PASSWORD'];
                $port = $_ENV['DB_PORT'];

                $dsn = "pgsql:host=$host;port=$port;dbname=$db;";
                $options = [
                    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
                    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
                    PDO::ATTR_EMULATE_PREPARES   => false,
                    PDO::ATTR_PERSISTENT         => false,
                ];
                
                self::$instance = new PDO($dsn, $user, $pass, $options);
                echo "¡Conectado a Postgres en WSL!";

            } catch (PDOException $e) {
                die("Error de conexión: " . $e->getMessage());
            }
        }
        return self::$instance;
    }  
}    
```
en index probamos nos saldra `¡Conectado a Postgres en WSL!`
```
use Yangpimpollo\Crud\database\Connection;
$db = Connection::getInstance();
```

## Ejercicio 8 - Diagrama de Flujo del Modelo 
La arquitectura que aplicaremos es Model–view–controller (MVC) donde tendra las siguientes capas:
usuario-->vista-->enrutador-->controlador-->modelo-->base datos

- solo modelo puede conectarse a la BD tiene una instancia de ella pero no conoce controlador
- el controlador instancia a modelo pero no conoce a la BD su funcion principal es transportar info 

## Ejercicio 9 - Modelo de Usuario
Creamos `User.php` en src/model este se encargara de toda la logica recibira array de datos del controlador 
se encargara de guardarlo o autenticarlo en la BD
```php
<?php

namespace Yangpimpollo\Crud\model;

use Yangpimpollo\Crud\database\Connection;
use PDOException;

class User {
    private $id;
    private $user_name;
    private $pass;
    private $email;
    private $role = 'user';
    private $db;

    public function __construct() { $this->db = Connection::getInstance();}

    public function register($data) {
        $this->user_name = $data['user_name'];
        $this->pass = password_hash($data['pass'], PASSWORD_DEFAULT);
        $this->email = $data['email'];

        try {
            $sql = "INSERT INTO usuarios (user_name, pass, email, role) VALUES (?, ?, ?, ?)";
            $stmt = $this->db->prepare($sql);
            
            // Ejecutamos pasando los valores en orden Retorna true si tuvo éxito, false si no
            return $stmt->execute([codigo basura
                $this->user_name,
                $this->pass,
                $this->email,
                $this->role
            ]);
        } catch (PDOException $e) {
            return false;
        }       
    }
}
```
lo probamos en index

```php
<?php

    require_once __DIR__ . '/bootstrap.php';

    use Yangpimpollo\Crud\model\User;
    $us1 = new User();
    $data = [
        'user_name' => 'maria123',
        'pass' => '123456',
        'email' => 'maria123@ejemplo.com'
    ];  
    $result = $us1->register($data);
    if ($result) {
        echo "Usuario registrado exitosamente.";
    } else {
        echo "Error al registrar el usuario.";
    }
```
saldra *Usuario registrado exitosamente.* lo podemos ver en DBeaver















