***

estar en la carpeta /home/romeo188/proyects/docker-files/laravel-dev


abrir otra terminal entrar en api


tuve que instalar

```bash
npm install @inertiajs/inertia

```

![[Pasted image 20241124211036.png]]

***

titket apartado de financiamiento colocar un text area 
detalle de credito

label comentario
migracion tb_detalles_financiamiento
editar el formrequest para aceptar el input del modelo
editar el modelo para agregar el campo de comentario


![[Pasted image 20241125122955.png]]

roadmap flujo 

pasos url https://testing.finanssoreal.com/financiamientos/404/edit

nos vamos a ruta  carpeta : routes/Finanssoreal/Financiamiento.php linea 19
nos redirige controlador:  app/Http/Controllers/ControladorFinanciamientoVehiculo.php

tambien nos da a conocer los roles que son admin,master,agencia..

en el controllado esta funcion edit que nos redirigue a la vista FinanciamientoVehiculo.editar
modelo DetallesFinanciamiento.php en la ryta app/Models/DetallesFinanciamiento.php

requests y en modelo colocar el nuevo campo

detallefinanciamiento
financiamientovehiculo

modificar le modelo



Investigar y recomendaciones ataque csrf



***

rutas  en laravel 

Route::ControllerCursoController::clase)->(funtion(){
     Route::get(''curso,'index');
     Route::get(''curso/create,'create');
     Route::get('curso/{course}','show');
});


crear un middleware
se guarda en la ruta: app/Http/Middleware/VerificarEdad
php artisan make:middleware VerificarEdad

en lazygit la d es para descargar cambios

formatear codigo con prettien

instalar en  contenedor 

```bash
npm install --save-dev prettier
```

instalar gloval

```bash
npm install -g prettier
```

correr el comando 

comando

```bash
npx prettier --check . --write
```

```bash
prettier --check . --write
```

***
### comando


```bash
 pnpm run blade-format 
```


```bash
pnpm run blade-format && pnpm run prettier-format && composer run format
```


usuario: romeo188
password: 12345678



echo $GIT_TOKEN 
 echo $GIT_TOKEN | gh auth login --with-token






















