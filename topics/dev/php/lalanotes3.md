# Ejercicios para afianzar Conocimientos Laravel + ORM

## Ejercicio 1 - crear proyecto laravel
1. Nos vamos a `cd Workspace/Laravel_` ejecutamos 
```bash
laravel new example-app2
cd example-app2
npm install && npm run build
composer run dev     

php artisan serve
```
no correrá configuremos el archivo `.env` la base de datos
```ini
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=example_app2
DB_USERNAME=postgres
DB_PASSWORD=root
```
2. no hay esa base en postgres ejecutamos `php artisan migrate` nos dirá  
<mark>WARN</mark>  The database 'example_app2' does not exist on the 'pgsql' connection.                                     
le damos a si y creara las tablas predeterminadas (users, ...) en posgres podemos verlas en DBeaver 

3. la tabla `migrations` registra cada migración hecha creamos una nueva migración `php artisan make:migration clear_defaulttables`
ponemos en up las tablas a borrar en up recordar que en cada clase anonima migración hay 2 funciones:                             
    - `up() hacer`              funcionara con   `php artisan migrate`
    - `down() deshacer`         funcionara con   `php artisan migrate:rollback`
    - buena practica colocar en hacer y deshacer 
    - php artisan migrate:rollback --step=3
    - php artisan migrate:reset
    - php artisan migrate:refresh
    - php artisan migrate:fresh
    - php artisan migrate:status
no borramos user, sessions, migrations
```php
    public function up(): void
    {
        Schema::dropIfExists('password_reset_tokens');
        Schema::dropIfExists('cache');
        Schema::dropIfExists('cache_locks');
        Schema::dropIfExists('jobs');
        Schema::dropIfExists('job_batches');
        Schema::dropIfExists('failed_jobs');
    }    
```

4. creamos nuestra primera tabla creamos una migración `php artisan make:migration build_vehicle_table`
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Support\Facades\DB;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        DB::statement("
            CREATE TABLE vehiculos (
                id_vehiculo SERIAL PRIMARY KEY,
                marca VARCHAR(50) NOT NULL,
                modelo VARCHAR(50) NOT NULL,
                anio INT,
                color VARCHAR(30)
            )
        ");
    }

    public function down(): void
    {
        DB::statement("DROP TABLE IF EXISTS vehiculos");
    }
};

```
tambien podemos crear de la manera recomendada por Laravel que servira en futuro para cualquier tipos Mysql , Oracle, etc
esta debe usar Schema y Blueprint a diferencia de DB
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;


public function up(): void
{
    Schema::create('vehiculos', function (Blueprint $table) {
        $table->id('id_vehiculo');
        $table->string('marca', 50);
        $table->string('modelo', 50);
        $table->integer('anio')->nullable();
        $table->string('color', 30)->nullable();
    });
}

public function down(): void
{
    Schema::dropIfExists('vehiculos');
}
```
5. ingresamos datos se puede ingresar desde Seeder, Modelo, Controlador, Consola vamos por Seeder
`php artisan make:seeder VehiculosSeeder`
```php
class VehiculosSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        DB::table('vehiculos')->insert([
            ['marca' => 'Toyota',    'modelo' => 'Corolla',  'anio' => 2022,'color' => 'Blanco'],
            ['marca' => 'Honda',     'modelo' => 'Civic',    'anio' => 2021,'color' => 'Negro'],
            ['marca' => 'Ford',      'modelo' => 'Mustang',  'anio' => 1967,'color' => 'Rojo'],
            ['marca' => 'Tesla',     'modelo' => 'Model 3',  'anio' => 2023,'color' => 'Gris'],
            ['marca' => 'Chevrolet', 'modelo' => 'Onix',     'anio' => 2020,'color' => 'Azul'],
        ]);
    }
}

//------------------------------------------------
// en database/seeders/DatabaseSeeder.php colocamos 
public function run(): void
{
    $this->call([ VehiculosSeeder::class, ]);
}

//-------------------------------------------------------------
        // DB::table('vehiculos')->insert([
        //     ['marca' => 'BMW',       'modelo' => 'Serie 3',  'anio' => 2024,'color' => 'Gris'],
        //     ['marca' => 'Mercedes-Benz', 'modelo' => 'Clase C', 'anio' => 2024, 'color' => 'Negro']
        // ]);

        DB::statement("
            INSERT INTO vehiculos (marca, modelo, anio, color) 
            VALUES ('Audi', 'A4', 2024, 'Plata'),
                   ('Nissan', 'Sentra', 2023, 'Blanco')
        ");


        // INSERT INTO vehiculos (marca, modelo, anio, color) VALUES ('BYD', 'Seal', 2024, 'Azul');

        // php artisan db:seed --class=VehiculosSeeder
```
ejecutamos `php artisan db:seed --class=VehiculosSeeder` la manera recomendada de laravel de insertar datos es mediante modelos                         
- 
- php artisan tinker
- DB::table('vehiculos')->get();
- podemos ver en DBeaver 10 carros

6. ejecutamos `php artisan make:controller VehiculoController`
