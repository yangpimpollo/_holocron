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
    ```php
        Route::get('/pagina1', function() {
            $dat1 = "Hola Mundo 8 8 8";
            return view('pagina1', compact('dat1'));
        });

        ----  pagina1.blade.php  ---------------------------    

        <?php echo $dat1; ?>

        <h1><mark>{{ $dat1 }}</mark></h1>
    ```