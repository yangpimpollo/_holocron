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

## Ejercicio 7: 5 Proyecto Todo List
1. en routes/web.php agregar
2. agregar funciones al controlador
3. crear carpeta y archivo resources/views/tareas/index.blade.php
4. como los datos no son persistentes el array $tareas se guarda en seccion
```php
// ==================== TODO LIST ====================
Route::get('/tareas', [SaludoController::class, 'indexTareas'])->name('tareas.index');
Route::post('/tareas', [SaludoController::class, 'storeTarea'])->name('tareas.store');
Route::post('/tareas/{id}/completar', [SaludoController::class, 'completarTarea'])->name('tareas.completar');
Route::post('/tareas/{id}/eliminar', [SaludoController::class, 'eliminarTarea'])->name('tareas.eliminar');
---------------------------------------------------------------------------------
    // ==================== TODO LIST CON SESSION ====================

    public function indexTareas()
    {
        $tareas = session('tareas', []);   // si no existe, devuelve array vacío

        return view('tareas.index', compact('tareas'));
    }

    public function storeTarea(Request $request)
    {
        $request->validate([
            'texto' => 'required|min:5|max:100'
        ]);

        $tareas = session('tareas', []);

        $tareas[] = [
            'id' => count($tareas) + 1,
            'texto' => $request->texto,
            'completada' => false
        ];

        session(['tareas' => $tareas]);

        return redirect()->route('tareas.index')
                         ->with('success', '¡Tarea agregada correctamente!');
    }

    public function completarTarea($id)
    {
        $tareas = session('tareas', []);

        foreach ($tareas as &$tarea) {
            if ($tarea['id'] == $id) {
                $tarea['completada'] = !$tarea['completada'];
                break;
            }
        }

        session(['tareas' => $tareas]);

        return redirect()->route('tareas.index')
                         ->with('success', 'Estado de la tarea actualizado');
    }

    public function eliminarTarea($id)
    {
        $tareas = session('tareas', []);

        $tareas = array_filter($tareas, function($tarea) use ($id) {
            return $tarea['id'] != $id;
        });

        // Reindexar los IDs
        $tareas = array_values($tareas);

        session(['tareas' => $tareas]);

        return redirect()->route('tareas.index')
                         ->with('success', 'Tarea eliminada correctamente');
    }
---------------------------------------------------------------------------------
@extends('layouts.app')

@section('title', 'Mi Todo List')

@section('content')

    <div style="max-width: 700px; margin: 0 auto;">

        <h1 style="text-align: center;">📋 Mi Lista de Tareas</h1>

        {{-- Mensajes de éxito --}}
        @if (session('success'))
            <div style="background: #d1fae5; color: #065f46; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                {{ session('success') }}
            </div>
        @endif

        {{-- Formulario para agregar tarea --}}
        <div style="background: white; padding: 25px; border-radius: 15px; box-shadow: 0 5px 20px rgba(0,0,0,0.1); margin-bottom: 30px;">
            <form action="{{ route('tareas.store') }}" method="POST">
                @csrf
                <input type="text" name="texto" placeholder="¿Qué tienes que hacer?" 
                       style="width: 75%; padding: 12px; border: 1px solid #ccc; border-radius: 8px;">
                <button type="submit" 
                        style="padding: 12px 25px; background: #1e3a8a; color: white; border: none; border-radius: 8px; cursor: pointer;">
                    Agregar Tarea
                </button>
            </form>
            @error('texto')
                <p style="color: red; margin-top: 5px;">{{ $message }}</p>
            @enderror
        </div>

        {{-- Lista de tareas --}}
        <div style="background: white; padding: 25px; border-radius: 15px; box-shadow: 0 5px 20px rgba(0,0,0,0.1);">
            <h2>Tareas ({{ count($tareas) }})</h2>
            
            @if (count($tareas) == 0)
                <p style="text-align: center; color: #666;">No hay tareas aún. ¡Agrega una!</p>
            @else
                <ul style="list-style: none; padding: 0;">
                    @foreach ($tareas as $tarea)
                        <li style="padding: 15px; background: #f8fafc; margin-bottom: 10px; border-radius: 10px; display: flex; align-items: center; justify-content: space-between;">
                            
                            <span style="{{ $tarea['completada'] ? 'text-decoration: line-through; color: #888;' : '' }}">
                                {{ $tarea['texto'] }}
                            </span>

                            <div>
                                <form action="{{ route('tareas.completar', $tarea['id']) }}" method="POST" style="display: inline;">
                                    @csrf
                                    <button type="submit" style="padding: 6px 12px; margin-right: 5px;">
                                        {{ $tarea['completada'] ? '↩️' : '✅' }}
                                    </button>
                                </form>

                                <form action="{{ route('tareas.eliminar', $tarea['id']) }}" method="POST" style="display: inline;">
                                    @csrf
                                    <button type="submit" style="padding: 6px 12px; background: #ef4444; color: white; border: none; border-radius: 5px;">
                                        🗑️
                                    </button>
                                </form>
                            </div>
                        </li>
                    @endforeach
                </ul>
            @endif
        </div>

    </div>

@endsection
```