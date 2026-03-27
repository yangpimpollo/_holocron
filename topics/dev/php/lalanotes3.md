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

<table width="100%">
  <tr>
    <td width="50%" style="vertical-align: top;">
      <strong>LADO IZQUIERDO (Original)</strong>
<pre><code>
// Tu código aquí
public function up() {
    Schema::create('users', function ($table) {
        $table->id();
        $table->string('name');
    });
}
</code></pre>
    </td>
    <td width="50%" style="vertical-align: top;">
      <strong>LADO DERECHO (Nuevo)</strong>
<pre><code>
// Tu código aquí
public function up() {
    Schema::create('users', function ($table) {
        $table->id();
        $table->string('full_name');
        $table->string('email');
    });
}
</code></pre>
    </td>
  </tr>
</table>





<div style="display: flex; gap: 20px; font-family: sans-serif; background-color: #f6f8fa; padding: 20px; border-radius: 10px;">

  <div style="flex: 1; background: #1e1e1e; color: #d4d4d4; padding: 15px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
    <div style="color: #858585; margin-bottom: 10px; font-size: 12px; font-weight: bold; border-bottom: 1px solid #333; padding-bottom: 5px;">MIGRACIÓN ANTERIOR</div>
    <pre style="margin: 0; font-family: 'Courier New', Courier, monospace; font-size: 13px; line-height: 1.5;">
$table->id();
$table->string('name');
$table->timestamps();
    </pre>
  </div>

  <div style="flex: 1; background: #1e1e1e; color: #d4d4d4; padding: 15px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); border-left: 4px solid #4CAF50;">
    <div style="color: #4CAF50; margin-bottom: 10px; font-size: 12px; font-weight: bold; border-bottom: 1px solid #333; padding-bottom: 5px;">MIGRACIÓN NUEVA</div>
    <pre style="margin: 0; font-family: 'Courier New', Courier, monospace; font-size: 13px; line-height: 1.5;">
$table->id();
$table->string('full_name');
$table->string('email');
$table->timestamps();
    </pre>
  </div>

</div>




| 📄 LADO IZQUIERDO (Original) | 📄 LADO DERECHO (Nuevo) |
| :--- | :--- |
| ```php
public function up() {
    Schema::create('users', function ($table) {
        $table->id();
        $table->string('name');
    });
}
``` | ```php
public function up() {
    Schema::create('users', function ($table) {
        $table->id();
        $table->string('full_name');
        $table->string('email');
    });
}
``` |







```bash
php artisan make:migration clear_defaulttables
```