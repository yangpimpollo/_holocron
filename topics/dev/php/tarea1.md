# Documentación de la tarea dejada

## Enunciado del Problema:
Este ejercicio integra los pilares del curso: Interfaces, Clases, Composer (Autoload), Inyección de Dependencias y Lógica Condicional.

*Objetivo:* Crear un sistema que envíe notificaciones. El sistema debe poder cambiar entre un servicio real (simulado) y uno de prueba sin romper el código.

1. *Estructura de Archivos:*
- src/NotificationInterface.php (El contrato) ✅
- src/EmailService.php (Implementación concreta 1) ✅
- src/FakeService.php (Implementación concreta 2) ✅
- src/Notifier.php (Clase que usa el servicio - Inyección de dependencias) ✅
- bootstrap.php (Configuración) ✅
- index.php (Uso del sistema) ✅

2. *Implementación:*
- Paso A (Interfaz): Define el método send($message).
- Paso B (Servicios): Crea EmailService y FakeService. Ambos deben usar implements NotificationInterface.
- Paso C (Inyección): La clase Notifier recibe en su constructor un objeto de tipo NotificationInterface. No sabe si es email o fake, solo sabe que tiene el método send.
- Paso D (Uso): En index.php, instancia el servicio que desees y pásalo al Notifier.

*Reto Adicional:* Añade una validación en el método send: si el mensaje está vacío, no debe realizar ninguna acción (usa un if de retorno temprano). Si usas Composer, asegúrate de que el namespace sea App y carga el vendor/autoload.php en tu bootstrap.php

## Proceso de Resolución:

1. crear la carpeta `test2` en Workspace entrar a visual studio code
2. crear carpeta `src` y el archivo `index.php` ponemos un `<?php echo "Hello World!"; ?>`
3. lanzamos el servido para probar `php -S localhost:8000` fumciona.
4. ponemos `composer init` en una nueva consola dentro vsc ponemos odiseo/app
5. podemos ver nuestro `composer.json` generado

```json
{
    "name": "odiseo/app",
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    },
    "authors": [
        {
            "name": "yangpimpollo",
            "email": "clin@odiseo.pe"
        }
    ],
    "require": {}
}
```

6. estructuramos el proyecto de esta manera
```Plaintext
test2/
├── vendor/     
├── src/                    
│   ├── NotificationInterface.php
│   ├── EmailService.php
│   ├── FakeService.php
│   ├── Notifier.php
│   └── Prueba.php           
├── bootstrap.php        
├── tarea1.md      
├── index.php                
└── composer.json            

```
7. el index que llama a la clase Prueba

```php
<?php
require_once __DIR__ . '/bootstrap.php';

use App\Prueba;

/*
    la documentación del proceso de desarrollo se encuentra en tarea1.md
    llamamos a nuestra clase principal Prueba donde puede saludar y notificar.

*/

$Prueba = new Prueba();
$Prueba->saludar();
$Prueba->quieroNotificar();

?>
```

8. la clase principal de nuestra App es Prueba
```php
<?php
namespace App;

use App\EmailService;
use App\FakeService;    
use App\Notifier;


/*
    clase principal creada para las funciones principales y no colocarlas en el index y así mantener  
    el código más limpio y organizado

    no se documenta mucho porque cada funcion es auto explicativa.

    esta clase prueba puede saludar; y notificar donde este abre menu de opciones para elegir 
    el servicio luego pedir el mensaje si el mensaje es vacio se vuelve a pedir el mensaje y 
    luego se envia la notificación con el servicio elegido.

    
*/


class Prueba {
    public function saludar() {
        echo "Bienvenido al servicio de notificaciones." . PHP_EOL;
        return "¡El autoload de App funciona!";
    }

    private function menuOpciones(): NotificationInterface{
        echo "Seleccione el servicio que desea utilizar:" . PHP_EOL;
        echo "1. EmailService" . PHP_EOL;
        echo "2. FakeService" . PHP_EOL;
        $valor =readline("Ingrese el número correspondiente al servicio: ");


        switch ($valor) {
            case '1':
                echo "Has seleccionado EmailService." . PHP_EOL;
                $service = new EmailService();
                break;
            case '2':
                echo "Has seleccionado FakeService." . PHP_EOL;
                $service = new FakeService();
                break;
            default:
                echo "Opción no válida. Saliendo." . PHP_EOL;
                return $this->menuOpciones();
        } 
        return $service;
    }

    private function getMensaje(): string {
        $message = readline("Ingrese el mensaje que desea enviar: ");
        if (empty($message)) {
            echo "El mensaje no puede estar vacío." . PHP_EOL;
            return $this->getMensaje();;
        }
        return $message;
    }

    public function quieroNotificar(): void {
        $service = $this->menuOpciones();
        $notifier = new Notifier($service);
        $notifier->notify($this->getMensaje());
        echo "gracias por usar el Servicio!." . PHP_EOL;
    }
}
```


9. la clase Notifier
```php
<?php
namespace App;

class Notifier {
    private NotificationInterface $service;

    public function __construct(NotificationInterface $service) {
        $this->service = $service;
    }

    public function notify(string $message): void {        
        $this->service->send($message);
    }
}
```

10. la inteface y los servicios
```php
<?php
namespace App;

interface NotificationInterface { public function send(string $message): void; }

class EmailService implements NotificationInterface {
    public function send(string $message): void {
        echo "notificación por EmailService: " . $message . PHP_EOL;
    }
}

class FakeService implements NotificationInterface {
    public function send(string $message): void {
        echo "notificación por FakeService: " . $message . PHP_EOL;
    }
}

```
