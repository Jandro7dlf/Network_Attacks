# NETWORK ATTACKS

## SQL INJECTION

### DEFINICIÓN

Es uno de los ataques web más comunes que inserta código malicioso a una base de datos a traves de una página web.

Por ejemplo: cuando una página te pide tu usuario y contraseña pero pones un código que te devuelve los usuarios y contraseñas que tiene en la base de datos.

![Alt text](./img/Attack/10.png)

Tipos de SQL Injection:

![Alt text](./img/Attack/11.png)

- **Error Based:** Necesita que el servidor de la base de datos envíe un error para saber la estructura de la base de datos. Por eso se debería desactivar o almacenar en logs con acceso restringido.

- **Union Based:** Combina los resultados de dos o más SELECT en un solo resultado que se devuelve como respuesta.

- **Boolean Based:** Necesita que pueda enviar una consulta a la base de datos que fuerza a la aplicación devolver un resultado diferente dependiendo si la consulta devuelve un verdadero o falso.

- **Time Based:** Necesita enviar una consulta a la base de datos y la fuerza a esperar por una cantidad de tiempo antes de responder. El tiempo que tarde indicará si el resultado de la consulta es verdadero o falso.

### EJEMPLO

Antes de hacer el ataque necesitamos que la base de datos tenga una vulnerabilidad, para ello vamos a usar una técnica llamada `Google Dorking` que se trata de usar el filtro de búsqueda de google para encontrar vulnerabilidades.

![Alt text](./img/Attack/12.png)

Abrimos las herramientas para desarrolladores (F12) dentro de red, seleccionamos la que mande el `$_GET` y usaremos esa URL con la cookie de sesión que haya dentro.  
Ya tenemos nuestra víctima. Pero atacar sin el consentimiento es ilegal por lo que usaremos un docker dentro de Kali Linux. Ahora vamos a por las herramientas.

Instalaremos en nuestra máquina Kali Linux el SQLMAP.

> SQLMAP es una herramienta de pruebas de penetración que automatiza el proceso de detectar y explotar los errores con inyección SQL.

```a
sudo apt-get install sqlmap
```

> - Recuerda hacer `apt update`

Una vez instalado podemos ver todas sus funciones con ``sqlmap -h`

![Alt text](./img/Attack/13.png)

Vamos a usar un comando que nos devuelva las bases de datos que tiene.

```a
sqlmap -u <url> --cookie="<cookie de sesión>; security=low" --schema --batch
```

Y nos dirá que recomendaciones seguir según encuentre alguna vulnerabilidad.

![Alt text](./img/Attack/14.png)

El resultado son todas las tablas y su tipo de las base de datos vulneradas.

Pero, ¿y las  contraseñas?

Las vamos a dumpear desde la tabla `users` que hemos encontrado, para eso tan solo hay que modificar el comando anterior un poco.

```a
sqlmap -u <url> --cookie="<cookie de sesión>; security=low" --dump -T users --batch
```

![Alt text](./img/Attack/15.png)

### VULNERABILIDAD Y PROTECCIÓN

Este ataque afecta a todo tipo de bases de datos, consiguiendo la información que almacenan. Incluyendo credenciales que pueden ser usadas para iniciar sesión en otras plataformas.

Para protegerse de este tipo de ataques, se debe de desactivar los errores que envía la base de datos, validar los input y parametrizar las consultas.  
