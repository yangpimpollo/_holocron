# Ejercicios para afianzar Conocimientos

## Ejercicio 1 - crear archivo
1. en linux `/home/yangpimpollo/Workspace` creamos la carpeta `test1`
2. nos movemos a la carpeta `cd test1` ejecutamos `code .` para abrir visual studio code
3. creamos index.php con un hello world!
4. lanzamos el servidor en la carpeta test1 `php -S localhost:8000`
5. visualizamos en la web `http://localhost:8000` 
6. tambien podemos ejecutar en consola `php index.php` para ver en consola

## Ejercicio 2 - conectar con postgresql
1. activamos la BD desde la consola `sudo service postgresql start`
2. se puede verificar si esta activo `sudo service postgresql status`
3. en index.php colocamos

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
4. creamos la tabla con un SQLscript en BDeaver si no tenemos y insertamos un valor
```sql
    CREATE TABLE usuarios (
        id INT AUTO_INCREMENT PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        email VARCHAR(150) NOT NULL UNIQUE
    );

    INSERT INTO usuarios (nombre, email) 
    VALUES ('Fulano de Tal', 'fulano@ejemplo.com');
```

5. usemos php para introducir datos dentro de try despues de la conecci{on} colocamos
```php
// Insertar un usuario
    $sql = "INSERT INTO usuarios (nombre, email) VALUES (:nom, :em)";
    $stmt = $pdo->prepare($sql);
    $stmt->execute([
        ':nom' => 'Maria',
        ':em'  => 'maria@ejemplo.com'
    ]);

    echo "¡Usuario insertado con éxito!";
```
| id |      nombre    |         email         |
|----|:--------------:|:---------------------:|
|  1 |  Fulano de Tal |  'fulano@ejemplo.com' |
|  2 |     Maria      |   'maria@ejemplo.com' |