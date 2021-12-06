# CronJob Laravel v6, v7, v8

Como crear un cronJob para Laravel y no morir en el intento  ⚠️ ⚠️

_Crearemos un prueba local en Windows y posteriormente lo subiremos a un servidor Cpanel, si usas Linux los pasos son los mismos solo al final un comando se ejecuta diferente, (**crontab -e**) igual puedes ver las referencias visual al final tanto para Windows como para Linux._

<h3>Introducción:</h3>

1.- Configuraremos una clase comando donde definiremos un comando a ejecutar.

2.- Agregaremos al archivo Kernel el comando recien configurado para poderlo ejecutar.

3.- Como extra configuraremos su Cpanel Server donde agregaremos el cron para que se empiece a ejecutar automático.

---

El cronjob puede ser útil para enviar emails, notificaciones o ejecutar sentencias a tu base de datos, ya sea por un horario definido o un horario futuro.

Para esta ocasión crearé un comando que se encarga de borrar el cache diariamente a las 7:00am

![Cron OK](https://user-images.githubusercontent.com/50802445/130536949-941df4b2-5e25-4d88-8604-1c2f738ff071.jpg)



-----

<h3>Empecemos por crear una clase Comando</h3>

**En ésta definiremos el nombre, descripción y las acciones que ejecutará el comando una vez lo llames**

 Ejecuta este comando en la terminal para crear una clase comando:

_Aquí solo crearemos la **clase** donde empezaremos a definir el nombre, la descripción y el comando, yo le llamaré **CacheClean** a mi clase._

1.- `php artisan make:command NombreDeTuLaClase //php artisan make:command CacheClean`

Después de ejecutar lo anterior, creará un directorio **Commands** dentro de **Console**.

![2](https://user-images.githubusercontent.com/50802445/130149807-7e75f134-a18a-4aa4-b869-beb1c6fb6d3f.jpg)

Abrimos el directorio **Commands** y seguido abres el archivo con el nombre de la clase, en mi caso CacheClean.php.

Dentro de la clase que acabamos de crear existen 2 variables protegidas la firma y la descripción:

En la firma pondras el nombre de tu comando, por ejemeplo **send:notification** ó en mi caso le pondre **clean:cache**.

Y en la descripción es algo breve acerca del comando, puedes visualizarlo más adelante ejecutando solo `php artisan`.

1.- `protected $signature   = 'clean:cache';` 

2.- `protected $description = 'Clear all caches';`

![desca](https://user-images.githubusercontent.com/50802445/130538439-23008122-ef47-4b1d-99a2-8f83e07e4898.jpg)

**Hasta aquí has concluido la firma/nombre del comando, continuemos para definir sus tareas a ejecutar.**

3.- Adicional a estas variables agregaré una más que se encargará de guardar un log después de ejecutar el comando:

`protected $message = '';`

Como en mi caso usaré el _Storage_ para guardar un log en un archivo de texto, para verificar que realmente esta ejecutandose el comando agregare el Facade de Storage y también el Facade Artisan porque utilizaré un comando nátivo de php; que es php artisan optimize:clear;

Al inicio agregue los Facades que usaré:

![illuminate](https://user-images.githubusercontent.com/50802445/130542994-afcb1235-4470-488e-ad3b-bb863dc1e2b7.jpg)


Al final de esta clase existe una función llamada **handle**, aquí pondremos la lógica de lo que ejecutará, en está parte ustedes definirian su lógica según lo necesiten:

![habdle](https://user-images.githubusercontent.com/50802445/130542886-25351ae7-e344-41cc-8e5b-7d343d55b772.jpg)

**Hasta aquí ya hemos terminado de configurar, el nombre del comando, la descripción y las tareas que ejecutará, pasemos al archivo Kernel.php**


---

<h3>Kernel: Llamemos el comando a ejecutar</h3>

Pasamos al archivo Kernel.php, (App/Console/Kernel.php) aquí llamaremos la clase que hemos creado antes

1.- En la variable $commands ingresa el nombre de tu clase:

![cacheclen](https://user-images.githubusercontent.com/50802445/130543501-adaed8fc-6aee-4d86-ae29-50235f84a596.jpg)

¡Bien ya agregaste el comando a la linea de comandos php artisan!

2.- Lo siguiente es definir el horario en que el comando se ejecutará esto puede ser útil para probar de manera local, ya que en Cpanel no es necesario porque ahí configuras los horarios(en Cpanel), aquí en la Doc de laravel puedes ver las frecuencias ó horarios ya predifinidos: [schedule-frequency-options](https://laravel.com/docs/6.x/scheduling#schedule-frequency-options);

![schedule](https://user-images.githubusercontent.com/50802445/130544127-4a61e58b-7dd8-46c4-92fd-ed4d2dae45fc.jpg)

Puede definir todas tus tareas programadas en la función **schedule** del **App\Console\Kernel**, es la encargada de realizarlo. 

---

**Hora de ejecutarlo**

De manera local puedes probar php artisan: y verás que el comando que creaste ya esta en la lista.

Puedes ejecutarlo usando el comando de horario

`php artisan schedule:run`

ó llamarlo por su nombre:

`php artisan clean:cache`

![clr](https://user-images.githubusercontent.com/50802445/130544931-f0909e95-c733-4cef-ba71-7e5c48eed03b.jpg)

**Si estas usando Laravel 8, puedes ejecutar php artisan schedule:list y ver una lista de los comandos que has creado.**

**Y si quieres que trabaje de forma local en automático con Laravel 8 ejecuta php artisan:work y estara trabajando en 2do plano de forma auto.**

**Esos comandos solo estan disponibles en Laravel 8 en 7 e inferior solo tenemos php artisan schedule:run pero es suficiente.**

¡Seguimos! comprobaré en mi archivo.txt si se ejecutó el comando que mencioné antes:

![okaa](https://user-images.githubusercontent.com/50802445/130545228-67c36d65-53ef-467e-a17c-20fe59182439.jpg)

---

**Cpanel configuración**

Lo siguiente es crear un cronjob en Cpanel, solo configuraremos el horario, solo eso;

![cronpanel](https://user-images.githubusercontent.com/50802445/130545635-a7eaa8f2-b046-4640-b083-3af69cba3cd4.jpg)

Aparece una vista con **Cron Email**

Ahí es opcional ingresar un email, cuando se ejecute la tarea te enviará un correo a modo de notificación.

---

**Agregar trabajo cron**

Aqui definiremos el horario en que se ejecutará de manera automática.

En mi caso lo definiré a las 7:00 am todos los días:

![config1](https://user-images.githubusercontent.com/50802445/130546160-bedd6f85-2d64-4a2a-8599-dd53099901ed.jpg)

**En la parte de mando es importante definir la ruta de su proyecto, pueden tomar de ejemplo la siguiente linea, de igual forma se encuentra en el mismo cPanel en la parte de arriba** :

`/usr/local/bin/php /home/hostserver/public_html/path/to/cron/script`

En mi caso solo necesito que pueda ver la carpeta **App**

`/usr/local/bin/php /home/myhost/ && php artisan schedule:run >> /dev/null 2>&1`

1.- La linea anterior se resume en ruta `/usr/local/bin/php /home/myhost/`, 

2.- Ejecutar el comando `&& php artisan schedule:run`.

3.- Y si existe un correo a enviar notificación `/dev/null` de lo contrario se queda null.

4.- Finalmente una condición verdadera para correr el comando si ó si:  `2>&1`.

![cronjobokaaa](https://user-images.githubusercontent.com/50802445/130546992-83fb26a7-c2bb-4793-93d1-c6da3aa06d97.jpg)

Y listo ya tienes el cron listo trabajando en automático, si quieres verificar que funciona ejecuta php artisan schedule:run ó php artisan elnombre:detucomando en la terminal;

Y ya habrá quedado;

![Cron OK](https://user-images.githubusercontent.com/50802445/130547171-460ec6d0-f244-4d6f-8e76-9ab257fc5c0f.jpg)

Listo hasta aquí concluye!! 🧑 😎

Referencias visuales en YT:

**Las diferencias más notorias entre Win y Linux son ejecutar este comando al final**:

`* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1`

Linux:
https://www.youtube.com/watch?v=OVr6K3Lr3DY&t=395s

Windows:
https://www.youtube.com/watch?v=0uG0B5HqiuA

Docu de Laravel:
https://laravel.com/docs/8.x/scheduling

Laravel jobs: configuracion de colas y jobs:

https://onlinecode.org/laravel-7-send-bulk-mail-in-background-using-queue/

https://dokov.bg/laravel-loves-cpanel
