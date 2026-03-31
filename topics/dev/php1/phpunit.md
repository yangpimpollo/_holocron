# Resumen del Proceso

## A. Configuración Previa
1. crear carpeta `learn_phpunit` en Workspace
2. crear `index.php`
3. ejecutar `composer init`
4. ejecutar `composer require --dev phpunit/phpunit`
    * se puede saber version de php y composer con `php -v` y `composer -v`
    * se puede saber version de de phpunit `./vendor/bin/phpunit --version` dentro de este proyecto
5. cambiar paquete por App `"App\\": "src/"` en composer.json
6. agregrar carpeta tests con `mkdir tests` y modificar composer.json
```json
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    }
```
7. ejecutar `composer dump-autoload`
8. crea `Prueba.php` dentro de src con una función en index colocar `require_once __DIR__ . '/vendor/autoload.php';` y `use App\Prueba;` crear una instancia y llamar la funcion debe funcionar.
9. recordar que nos falta crear `phpunit.xml`

## B. Primer Test
1. crea un archivo dentro de tests/ llamado `EjemploTest.php` recordar las clases de prueba deben terminar con el sufijo `Test` y heredar de `TestCase`.
```php
<?php
namespace Tests;

use PHPUnit\Framework\TestCase;

class EjemploTest extends TestCase
{
    public function test_suma_basica()
    {
        $resultado = 1 + 1;
        
        // "Afirmamos" que el resultado debería ser 2
        $this->assertEquals(2, $resultado);
    }
}
```
2. ejecutamos `./vendor/bin/phpunit tests` nos dara OK (1 test, 1 assertion)
3. ejecutamos `./vendor/bin/phpunit --generate-configuration` enter 4 veces nos creara phpunit.xml colocamos colors="true"
4. ejecutamos `./vendor/bin/phpunit`el resultado en verde se puede desacrtivar el requireCoverageMetadata="false"

5. cada prueba de debe llevar en su nombre `test_` si no tiene colocar encima `#[Test]` importar use `PHPUnit\Framework\Attributes\Test;` antes `//** @test */` pero es opsoleto si no es asi no se le considera como prueba
6. las aserciones:
---
    * Igualdad e Identidad
    * assertEquals($esperado, $actual) : verifica igualdad
    * assertSame($esperado, $actual) : verifica si son identicos
---
    * Booleanos y Nulos
    * assertTrue($valor): Verifica que sea estrictamente true.
    * assertFalse($valor): Verifica que sea estrictamente false.
    * assertNull($valor): Verifica que la variable sea null.
    * assertNotNull($valor): Verifica que no sea nulo. 
---
    * Strings (Cadenas de texto)
    * assertStringContainsString($aguja, $pajar): Verifica si un texto contiene a otro (sensible a mayúsculas).
    * assertStringContainsStringIgnoringCase(): Igual al anterior, pero ignora mayúsculas/minúsculas.
    * assertStringStartsWith($prefijo, $texto): Verifica si empieza con algo específico.
    * assertStringEndsWith($sufijo, $texto): Verifique si termina con algo específico.
    * assertMatchesRegularExpression($patron, $texto): Verifica usando una expresión regular
---
    * Arreglos (Arrays)
    * assertArrayHasKey($clave, $array): Verifique si existe una posición o clave específica.
    * assertContains($valor, $array): Verifica si existe un valor dentro de un arreglo.
    * assertCount($numero, $array): Verifica cuántos elementos tiene el arreglo.
    * assertIsArray($valor): Verifique si el tipo de dato es un arreglo.
---
    * Tipos y Objetos
    * assertInstanceOf(Clase::class, $objeto): Verifique si un objeto pertenece a una clase específica.
    * assertIsString(), assertIsInt(),assertIsFloat() : Verifican el tipo de dato básico.
    * assertObjectHasProperty('propiedad', $objeto): Verifica si un objeto tiene una propiedad definida.