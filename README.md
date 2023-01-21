# odoo

![odoo](https://upload.wikimedia.org/wikipedia/commons/a/a7/Odoo_Official_Logo.png)

## 1. Introduccion 


[**Odoo**](https://es.wikipedia.org/wiki/Odoo) es un software de [ERP](https://es.wikipedia.org/wiki/Planificaci%C3%B3n_de_recursos_empresariales) integrado que incluye CRM, sitio web y comercio electrónico, facturación, contabilidad, fabricación, gestión de almacenes y proyectos, e inventario entre otros[^1]. 

El módulo del servidor está escrito en el lenguaje [**Python**](https://es.wikipedia.org/wiki/Python) y emplea  [**PostgreSQL**](https://es.wikipedia.org/wiki/PostgreSQL) como sistema gestor de base de datos. 


## 2.Instalando odoo 


Para instalar odoo emplearemos docker compose. Esto nos permitira crear un entorno ideal para hacer pruebas gracias a las facilidades que ofrecen los contenedores de docker. 

Creamos una carpeta vacía para almacenar nuestro ***docker-compose.yml***, que es lo que emplearemos para construir nuestro wordpress. El contenido del archivo es el siguiente:

```MiniYAML
version: '3.1'
services:
  mydb:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
    ports:
      - "5430:5432"
    volumes:
     - dam22_odoo:/var/lib/postgresql/data
  
  odoo:
    image: odoo:14.0
    container_name: dam22_odooklk
    ports:
      - "8069:8069"
    depends_on:
      - mydb
volumes:
  dam22_odoo:
```

Lo siguiente es instalar docker compose, lo cual hacemos mediante el siguiente comando:

```bash
$sudo apt-get install docker-compose-plugin
```

Una vez instalado el docker compose, lanzamos el siguiente comando ***en el mismo directorio donde está nuestro docker-compose.yml***:

```bash
$docker compose up -d
```

Listo, con esto nuestros contenedores están creados y listos para su utilización. En caso de que salte error con los puertos, más adelante se detalla como proceder.

## 3. Problemas con la disponibilidad de puertos

Cuando intentamos iniciar el compose vemos que nos da el siguiente error:


![Error](https://github.com/AlexFerMar/Imagenes/blob/main/Odoo/Captura%20desde%202023-01-19%2011-17-55.png)




Esto se debe a que el equipo donde estamos trabajando tiene postgres ya instalado, para solucionar este error tenemos varias opciones:

 - Desinstalar postgres del equipo. De esta forma el puerto quedará disponible para el postgres del docker.

 - Parar postgres en el equipo. Si simplemente paramos postgres el puerto estara disponible hasta que volvamos a iniciarlo
  
 - Cambiar el puerto al que esta mapeado el postgres del docker. Si cambiamos el puerto por defecto a uno que este disponible ya no nos dara problemas.
 

La opción que escogeremos en este caso es parar postgres temporalmente, dado que es la más sencilla. 


Para ello emplearemos el comando:

```bash
$systemctl stop postgresql
```

Esto parara postgres y nos permitira levantar el contenedor de docker sin cambiar el puerto asignado.

Una vez terminemos con docker podemos volver a arrancar postgres reiniciando el sistema o empleando el siguiente comando:

```bash
$systemctl start postgresql
```


## 4.Accediendo a la base de datos 

Después de solucionar los problemas de disponibilidad de puertos, podemos acceder a nuestra base de datos.

En este caso emplearemos [**pycharm**](https://www.jetbrains.com/es-es/pycharm/) en su edición profesional ya que cuenta con una pestaña para realizar conexiones a base de datos. 

Una vez introducimos la dirección y las credenciales, el programa se conectará a la base de datos y nos dejará acceder y manejar sus tablas.


![postgres](https://github.com/AlexFerMar/Imagenes/blob/main/Odoo/Captura%20desde%202023-01-20%2014-44-12.png)



## 5.Accediendo a odoo


Comprobando que la base de datos funciona, es momento de revisar odoo. Para ello nos conectaremos a ***http://localhost:8069/*** en el navegador.

Debería salirnos la siguiente imagen: 


![menu_odoo](https://github.com/AlexFerMar/Imagenes/blob/main/Odoo/Captura%20desde%202023-01-20%2014-48-35.png)



Con esto [**Odoo**](https://es.wikipedia.org/wiki/Odoo) ya está listo para realizar pruebas y operaciones con el.


[^1]: Descripcion sacada de [Wikipedia](https://es.wikipedia.org/wiki/Odoo).

