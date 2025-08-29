<img  align="left" width="150" style="float: left;" src="https://www.upm.es/sfs/Rectorado/Gabinete%20del%20Rector/Logos/UPM/CEI/LOGOTIPO%20leyenda%20color%20JPG%20p.png">
<img  align="right" width="60" style="float: right;" src="https://www.dit.upm.es/images/dit08.gif">

<br/><br/><br/>

# Práctica 6: Express CV

Versión: 3 de Abril de 2025

## Objetivos
* Afianzar los conocimientos obtenidos sobre el uso de Express para desarrollar servidores web.
* Aprender a completar en el esqueleto una aplicación de servidor basada en MVC (Modelo-Vista-Controlador) con vistas parciales EJS.

## Descripción de la práctica

En esta práctica se desarrollará un servidor web con el siguiente interfaz REST: 

```
GET /        // muestra la página home de bienvenida.
GET /author  // muestra la página de créditos con el nombre, la foto y cv del autor.
```

El esqueleto inicial del servidor web se generará con el paquete **express-generator**. Este esqueleto se adaptará para atender las primitivas anteriores. Se seguirá el patrón MVC, creando los modelos, controladores, rutas y vistas necesarios.


Para las vistas se usarán plantillas **EJS**. Se usará el paquete **express-partials** para añadir soporte de vistas parciales y poder definir un marco común de aplicación (**layout.ejs**) para todas las vistas. El acceso a cada una de las vistas se hará usando una barra de navegación en el marco de la aplicación.


## Descargar el código del proyecto

Instrucciones [aquí](https://github.com/CORE-UPM/Instrucciones_Practicas/blob/main/README.md#descargar-el-c%C3%B3digo-del-proyecto).


## Tareas

### Tarea 1 - Crear el esqueleto de la aplicación

El proyecto clonado solo contiene los ficheros necesarios para ejecutar el autocorrector.
El alumno debe crear un subdirectorio nuevo en el que desarrollará la practica. 
Debe usar el paquete **express-generator** para crear ese subdirectorio de trabajo con el esqueleto inicial de la práctica.

Primero hay que instalar el paquete **express-generator**. Para ello, el alumno debe ejecutar:

```
$ npm install express-generator
```

Este paquete proporciona un programa llamado **express** que sirve para crear el esqueleto inicial de una aplicación web, es decir, los ficheros que implementan una version inicial del servidor.

Para crear este esqueleto inicial, el alumno debe ejecutar el comando:

```
$ npx express --view ejs blog
```

Este comando crea el subdirectorio **blog** con los ficheros del esqueleto inicial de la práctica. 

Para arrancar y probar el servidor, primero hay que cambiarse al directorio **blog** e instalar los paquetes de los que depende.

```
$ cd blog
$ npm install
```

Ahora el servidor puede lanzarse ejecutando el comando:

```
$ npm start
```

**start** es un script definido en **package.json** para que ejecute **"node bin/www"** (nota: El servidor también podría lanzarse ejecutando directamente este comando).

El servidor lanzado atiende las peticiones en el puerto **3000**. Hay que ejecutar un navegador (Chrome, Firefox, Safari, ...) y conectarse a la URL **http://localhost:3000**. El navegador mostrará la página principal que ofrece el servidor que hemos creado.

### Tarea 2 - Instalar supervisor (opcional)

Cada vez que se modifica algún fichero (javascript) de nuestro proyecto, hay que detener el servidor y relanzarlo para los nuevos cambios se apliquen. 
Para no tener que hacer este proceso manualmente, pueden instalarse programas que lo hacen automáticamente por nosotros (**forever**, **supervisor**, ...). 
Estos programa vigilan los ficheros de nuestra aplicación, y si detectan que hay cambios, detienen la aplicación y la relanzan de nuevo.

El alumno debe usar **supervisor**. Para instalarlo debe ejecutar el siguiente comando en el terminal estando en el directorio `blog`

```
$ npm install supervisor
```

Para usar **supervisor**, el servidor se debe lanzar ejecutando el comando "**npx supervisor bin/www**". Por razones de comodidad, se puede añadir un script en **package.json**.

El alumno debe modificar la sección **scripts** de **package.json** (del directorio `blog`) para que quede así:

```
  "scripts": {
    "start": "node ./bin/www",
    "super": "supervisor ./bin/www"
  }
```

Se ha añadido el script **super** para lanzar el servidor usando **supervisor**. 
El comando para ejecutar el script que lanza el servidor usando **supervisor** es:

```
$ npm run super
```

### Tarea 3 - Modificar la página Home

Se pide modificar el fichero principal que sirve el servidor: 

* Modificar la etiqueta **title** del head para que el título de la página sea **Blog**.
* Cambiar el título **h1** del body para sea **Welcome to My Blog**.

*Nota*: tenga en cuenta que cuando añada los marcos de la aplicación con express-partials puede que este test deje de pasar en el autocorector hasta que aplique los cambios necesarios en el paso correspondiente a los marcos.


### Tarea 4 - Crear el marco de aplicación

Vamos a usar el paquete **express-partials** para crear un marco de aplicación común para nuestro proyecto. 
El marco de aplicación proporciona la página HTML que se usará para mostrar cualquiera de las vistas del servidor.

El paquete **express-partials** proporciona un middleware que modifica el método **res.render** proporcionado por **Express**. 
Por defecto, la nueva versión de **res.render** inyecta las vistas EJS a mostrar dentro del fichero **views/layout.ejs**, 
que es el fichero que implementa el marco de aplicación. 
El marco tiene una sentencia **\<%- body %\>** que es donde se inserta la vista a mostrar.

El alumno debe instalar este paquete ejecutando:

```
$ npm install express-partials
```

En la documentación del paquete se detalla cómo debe instalarse. 
En **app.js** hay que requerir el paquete y usar **app.use** para instalar el middleware que proporciona.

```
...
var logger = require('morgan');
var partials = require('express-partials');    \<--- Añadir

var indexRouter = require('./routes/index');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));
app.use(partials());           \<--- Añadir

app.use('/', indexRouter);
...
```

El alumno debe crear el fichero **views/layout.ejs** conteniendo el marco de aplicación. 
Es la página HTML5 que se enviará al cliente (el navegador) para todas las peticiones que realice. 
Esta página debe utilizar las marcas de HTML5 habituales: 

- **\<header\>**: cabecera donde se suele incluir el nombre del portal.
- **\<nav\>**: barra de navegación con los botones para solicitar las vista que se desea ver.
- **\<main\>**: contenido principal de la página.
- **\<footer\>**: pié de página.

El fichero **views/layout.ejs** debe incluir la sentencia **\<%- body %\>** en el punto donde se va a insertar la vista a mostrar (el método **res.render** asigna el código HTML de la vista a la variable **body**).

Finalmente, el alumno debe retocar la vista **views/index.ejs** que implementa la vista home para que se integre correctamente con 
el marco **views/layout.ejs**, usando un elemento HTML de tipo **\<section\>**. 
Elimine de **views/index.ejs** todos los elementos HTML que ya proporciona **views/layout.ejs**, y cree una sección **\<section\>** que 
contenga solo el contenido específico de esta vista (el encabezado y el párrafo de bienvenida).

### Tarea 5 - Crear los elementos MVC de la primitiva GET /author. 

El alumno debe modificar el fichero **routes/index.js** para añadir la nueva ruta para **GET /author**.

Esta vista muestra el CV del autor.

La definición de esta ruta es muy parecida a la definición de **GET /**.
Use **router** con su método **get**. 
El primer parámetro es el path de la ruta, es decir, **"/author"**. 
El segundo parámetro es una función middleware que debe renderizar la vista con el CV del autor.

La vista con el CV del autor se debe implementar en el fichero **views/author.ejs**. 
Es un fichero **EJS**. 
Debe contener un elemento HTML de tipo **\<section\>**. 
Esta vista debe mostrar el título de la página, el nombre del autor de la práctica, su foto y un breve texto sobre el autor.

La fotografía del autor que muestra en esta vista debe incluirse en el directorio de imágenes **public/images** y llamarse **foto.jpg**. Puede ser ficticia. 

Para probar este desarrollo, el alumno puede conectarse con el navegador a la URL **http://localhost:3000/author**. 
Debe mostrar la vista con el CV del autor dentro del marco de aplicación.


## Pruebas con el autocorector

Instrucciones [aquí](https://github.com/CORE-UPM/Instrucciones_Practicas/blob/main/README.md#pruebas-con-el-autocorector).

## Pruebas manuales y capturas de pantalla

Instrucciones [aquí](https://github.com/CORE-UPM/Instrucciones_Practicas/blob/main/README.md#pruebas-manuales-y-capturas-de-pantalla).

Capturas a entregar con esta práctica: 

- Captura 1: Captura de la pantalla Home donde se vea el diseño de la pantalla (marco de aplicación).
<kbd>
<img src="https://user-images.githubusercontent.com/716928/218464013-42c704e9-212d-4ae4-bde7-82e471bfd885.png" alt="captura de pantalla" width="500"/>
</kbd>

- Captura 2: Captura de la pantalla de autor donde se vea el diseño de la pantalla (marco de aplicación). El diseño de pantalla debe ser igual al de la captura anterior.
<kbd>
<img src="https://user-images.githubusercontent.com/716928/218464044-4220c58d-6fe5-4955-b836-b87ab0c05a87.png" alt="captura de pantalla" width="500"/>
</kbd>

## Instrucciones para la Entrega y Evaluación.

Instrucciones [aquí](https://github.com/CORE-UPM/Instrucciones_Practicas/blob/main/README.md#instrucciones-para-la-entrega-y-evaluaci%C3%B3n).

## Rúbrica

Se puntuará el ejercicio a corregir sumando el % indicado a la nota total si la parte indicada es correcta:

- **20%:** Petición / con elementos adecuados title y h1
- **5%:** Funciona la petición /users
- **35%:** Uso correcto de las plantillas express-partials
- **20%:** Petición /author
- **20%:** Se muestra correctamente la foto 


Si pasa todos los tests se dará la máxima puntuación.
