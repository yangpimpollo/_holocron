# FORMULARIOS PHP
se PUEDE recopilar datos de formularios con $_GET $_POST

```html
    <form action="" method="get">
        Modelo: <input type="text" name="modelo"><br>
        Color: <input type="text" name="color"><br>
        Motor: <input type="text" name="motor"><br>
        Fuerza (HP): <input type="number" name="fuerza"><br>
        <input type="submit" value="Registrar Carro">
    </form>
```
en `action` es lo que se quiere hacer podemos poner `welcome.php` si queremos redirigir lo dejamos vacio para retornar 
en method ponemos get o post hay varios tipos de input: text, email, password, number, url, checkbox, radio, color, file, etc o
botones como submit, button

🛑 tener en cuenta con el método GET los datos son visible para todos al enviar el gormulario la url termina con la info                
    localhost:8000/index.php?modelo=Mustang+1967&color=Rojo&motor=V8+4.7L&fuerza=200