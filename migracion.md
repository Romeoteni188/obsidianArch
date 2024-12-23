Los campos tienen que estar en ingles a nivel de base de datos.
Comandos para agregar un nuevo campo, se crean dos funciones up y down necesitamos editar

```bash
php artisan make:migration add_comment_financing --table=tb_detalles_financiamiento
```

tener una configuracion,  asi para la agregar un campo donde se quiere tener despues del otro.
dentro de la funcion up:

```bash
Schema::table('tb_detalles_financiamiento', function (Blueprint $table) {

//

$table->text('comment')->nullable(true)->after('fecha_pago');

});
```

coment es el nuevo campo que va estar detras de fecha_pago

dentro de la funcion down

```bash
Schema::table('tb_detalles_financiamiento', function (Blueprint $table) {

$table->dropColumn('comment');

});
```


despues

```bash
php artisan migrate
```


