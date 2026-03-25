## Ejercicio 5: Formularios + Recibir datos del usuario
1. agregamos 2 metodos SaludoController.php 
2. creamos resources/views/formulario.blade.php
3. creamos resources/views/resultado.blade.php
4. agregamos 2 rutas en routes/web.php
5. abrir http://127.0.0.1:8000/formulario llenar el for y enviar nos enviara http://127.0.0.1:8000/procesar-formulario teniendo lo que llenamos
6.
```php
    public function formulario()
    {
        return view('formulario');
    }

    public function procesarFormulario(Request $request)
    {
        // Aquí recibimos los datos que el usuario envió
        $nombre = $request->input('nombre');
        $email  = $request->input('email');
        $mensaje = $request->input('mensaje');

        return view('resultado', compact('nombre', 'email', 'mensaje'));
    }

---------------------------------------------------------------------------------

@extends('layouts.app')

@section('title', 'Formulario')

@section('content')
    <div style="max-width: 600px; margin: 0 auto; background: white; padding: 40px; border-radius: 15px; box-shadow: 0 5px 20px rgba(0,0,0,0.1);">

        <h1>Formulario de Contacto</h1>

        <form action="{{ route('procesar') }}" method="POST">
            @csrf   <!-- ¡MUY IMPORTANTE! -->

            <label>Nombre:</label><br>
            <input type="text" name="nombre" required style="width: 100%; padding: 10px; margin: 10px 0;"><br>

            <label>Email:</label><br>
            <input type="email" name="email" required style="width: 100%; padding: 10px; margin: 10px 0;"><br>

            <label>Mensaje:</label><br>
            <textarea name="mensaje" rows="5" required style="width: 100%; padding: 10px; margin: 10px 0;"></textarea><br>

            <button type="submit" style="padding: 12px 30px; background: #1e3a8a; color: white; border: none; border-radius: 8px; cursor: pointer;">
                Enviar Mensaje
            </button>
        </form>

    </div>
@endsection

---------------------------------------------------------------------------------


@extends('layouts.app')

@section('title', 'Resultado')

@section('content')
    <div style="max-width: 600px; margin: 0 auto; background: white; padding: 40px; border-radius: 15px; box-shadow: 0 5px 20px rgba(0,0,0,0.1);">

        <h1>¡Gracias por tu mensaje!</h1>

        <p><strong>Nombre:</strong> {{ $nombre }}</p>
        <p><strong>Email:</strong> {{ $email }}</p>
        <p><strong>Mensaje:</strong> {{ $mensaje }}</p>

        <a href="/formulario">← Enviar otro mensaje</a>
    </div>
@endsection

---------------------------------------------------------------------------------

Route::get('/formulario', [SaludoController::class, 'formulario'])->name('formulario');
Route::post('/procesar-formulario', [SaludoController::class, 'procesarFormulario'])->name('procesar');
```
¿Qué aprendimos hoy?

- `@csrf` → Protege el formulario contra ataques (Laravel lo exige)
- `$request`->input('nombre') → Recibe los datos del formulario
- `->name('nombre')` → Le da un nombre a la ruta (útil para route('nombre'))
- `method="POST"` → Se usa cuando enviamos datos (no solo mostramos)


## Ejercicio 6: 4 Validaciones de Formularios
1. agregamos un validate antes de crear las variables en SaludoController.php
2. mejoramos formulario.blade.php
3. agregamos un if error que con foreach mostrara todos los errores
4. en cada imput le ponemos un error

```php
public function procesarFormulario(Request $request)
{
    // Validación
    $request->validate([
        'nombre'  => 'required|min:3|max:50',
        'email'   => 'required|email',
        'mensaje' => 'required|min:10|max:500',
    ]);

    $nombre  = $requ
    ...

---------------------------------------------------------------------------------
        @if ($errors->any())
            <div style="background: #fee2e2; color: #b91c1c; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                <strong>Por favor corrige los siguientes errores:</strong>
                <ul style="margin: 10px 0 0 20px;">
                    @foreach ($errors->all() as $error)
                        <li>{{ $error }}</li>
                    @endforeach
                </ul>
            </div>
        @endif

        <input type="text" name="nombre" value="{{ old('nombre') }}" 
                   style="width: 100%; padding: 10px; margin: 8px 0;">
        @error('nombre')
            <span style="color: red; font-size: 0.9rem;">{{ $message }}</span>
         @enderror<br>     
```

como funciona:

- El usuario llena el formulario y hace clic en "Enviar"
- Laravel recibe la petición en el método procesarFormulario
- Se ejecuta la línea $request->validate(...)
- Laravel revisa automáticamente cada regla que pusiste:
- ¿nombre está vacío? → error
- ¿nombre tiene menos de 3 letras? → error
- ¿email tiene formato correcto? → error etc.

- Si hay algún error:
- Laravel detiene la ejecución del método
- Guarda los errores en una variable especial ($errors)
- Redirige automáticamente al usuario de vuelta al formulario (/formulario)
- Mantiene los datos que el usuario escribió (gracias a old('nombre'))

Validar en el servidor es mucho más seguro que validar solo con JavaScript en el navegador