# CronJob Laravel v6, v7, v8

Como crear un cronJob para Laravel y no morir en el intento **OS Linux** ⚠️ ⚠️

_Pd: Si estas desde Windows puedes seguir estos pasos, pero cuando inicie el llamado al Cron con Windows debes utilizar archivos .bat para llamar tus tareas programadas._

<h3>Introducción:</h3>

1.- Configuraremos el  **programador de tareas** para este, solo se necesita una sola entrada Cron en su **servidor**. 

2.- Su horario de tareas se define en el método **app/Console/Kernel.php**  en la función _schedule_ desde ahí puedes programar cada cuando y cuando ejecutar las tareas. 

![1](https://user-images.githubusercontent.com/50802445/130148051-6a5d1e10-054f-4bc9-a91a-0680e7a25a4d.jpg)

-----

<h3>Crear clase comando</h3>

**Empezemos creando una clase comandos, en esta definiremos el nombre, descripción y las acción que ejecutará el comando una vez lo llames, ejecuta este comando en la terminal para crear la clase:**

Aquí solo crearemos la **clase** donde empezaremos a definir los comandos.

1.- `php artisan make:command NombreDeLaClase`

Despues de ejecutar este comando creara una clase, que estará dentro del directorio _Commands_ que se habrá creado tras ejecutar el comando anterior.

![2](https://user-images.githubusercontent.com/50802445/130149807-7e75f134-a18a-4aa4-b869-beb1c6fb6d3f.jpg)

Dentro de la clase que acabamos de crear existen 2 variables protegidas:

Y en la segunda ($description) es solo la descripción del comando.

1.- `protected $signature   = 'command:name';` 

2.- `protected $description = 'Command description';`

Supongamos que quieres enviar una notificación a un usuario, podrías en $signature nombrarle de la siguiente manera como ejemplo:

`protected $signature   = `send:notification';` 

Y para ejecutarlo en consola sería así: `php artisan send:notification` (Puedes llamarlo desde consola de manera manual y también programarlo por tiempos).

En la `protected $description = 'Command description';` puedes agregar algo breve, como mensaje de retroalimentación al ejecutar el comando.

**Hasta aquí ya has concluido la firma del comando.**

---

<h3>Definición de tareas a ejecutar</h3>

Pasamos al archivo Kernel.php, donde agreramos nuestras clases que daran entrada a las tareas:

1.- Primero **llamaremos a la clase** que creamos cuando ejecutamos este comando `php artisan make:command NombreDeLaClase` al incio 

En mi caso lo llame `MorningCacheClean`

En la variable `protected $commands` agregas el nombre de la clase que creaste:

`Commands\NombreDeTuClaseAqui::class`

![3](https://user-images.githubusercontent.com/50802445/130152082-35e6bf22-a5c1-4cff-9f85-4a37110d785a.jpg)

¡Bien ya agregaste el comando a la linea de comandos!

2.- Lo siguiente es definir la tarea y ver cada cuando ó cuando se ejecutará:

Puede definir todas tus tareas programadas en la función **schedule** de la **App\Console\Kernel**, es la encargada de realizarlo. 

![4](https://user-images.githubusercontent.com/50802445/130152849-781bac7b-9b11-4c8a-80c2-64e87ef77cae.jpg)


