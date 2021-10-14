# SECUAZ

## Objetivos de diseño
- Generación automática de cuentas de usuario a través de ficheros
CSV.
- Generación automática de cursos en classroom y asociación
de cursos con profesores
- Generación automática de matrículas de alumnos en cursos de classroom.
- Generación automática de usuarios de moodle.
- Generación automática del árbol de cursos de moodle.

### Mejoras deseables con respecto a versiones previas.
- Actualización de datos de usuarios sin necesidad de configuración
previa.
- Generación automática de ficheros de salida a unidades de almacenamiento
en nube.
- Monitorización activa de ficheros de entrada alojados en almacenamiento
de nube.
- Containerización.
- Interfaz web amigable.

# Documentación


## Habilitación de API's

Para cada API de google que queramos utilizar en nuestro proyecto
tendremos que garantizar que esté habilitada.

![alt Página del tutorial sobre las APIS de google](./doc/img/DirectoryAPI.png)

* Spreadsheet API: https://developers.google.com/sheets/api/quickstart/python
* Directory API Quickstart: https://developers.google.com/admin-sdk/directory/v1/quickstart/python?refresh=1
* Classroom API Quickstart: https://developers.google.com/classroom/quickstart/python
* Classroom Python client API reference: https://developers.google.com/resources/api-libraries/documentation/classroom/v1/python/latest/index.html
* Docs API Quickstart: https://developers.google.com/docs/api/quickstart/python
* Docs API Overview: https://developers.google.com/docs/api/how-tos/overview
* Raíz de la documentación de Docs API: https://developers.google.com/docs/api/
* Cómo escribir en documentos de Docs: https://developers.google.com/docs/api/how-tos/move-text
* Cómo hacer un merge de documentos de Docs: https://developers.google.com/docs/api/how-tos/merge



A continuación, por cada servicio, tendremos que descargarnos un archivo
json con sus credenciales.

![alt Diálogo para la descarga del archivo de credenciales](./doc/img/DownloadClientConfiguration.png)


## Sobre la generación de TOKENS.
Hay que tener en cuenta que, si bien, cada servicio de google genera
su propio fichero de credenciales json, se puede tener un único
token.pickle para todos los servicios siempre y cuando, el SCOPE que se
pase durante la autenticación los abarque a todos.

Cada módulo tiene su autenticación individual a efectos de desarrollo.
En su momento habrá que refactorizar ese código y dejar un único punto
de entrada que autentique contra los servicios necesarios unificando
todos los SCOPES.


## Archivos externos de configuración.
Para no tener que tocar el código cuando se necesite adaptar la herramienta
a otros centros, cursos escolares, etc... He optado por integrar prettyconf

Prettyconf es una librería de gestión de configuraciones bastante potente,
no voy a usar funciones excesivamente avanzadas de la misma.

Para que nuestra aplicación funcione correctamente deberás personalizar
los valores de configuración en un archivo denominado secuaz.env que se
encontrará siempre dentro del directorio config.

Tienen un fichero de configuración de ejemplo en config/sample-config.env

## Proceso de sincronización.
A la hora de llevar a cabo el proceso de sincronización, lo primero que haremos será
comprobar que en el conjunto de datos original no se hayan llevado a cabo modificaciones.

Para lograrlo, convertiremos los datos, tal cual se obtienen de la hoja de
cálculo de Drive, en una cadena de texto y calcularemos su hash.

En la primera ejecución se almacenará dicho hash en un fichero en disco.
En las sucesivas ejecuciones, se comprobará el valor almacenado en el fichero para
ver si, el conjunto de datos ha variado o no.

Si el conjunto de datos no ha variado se detendrá la ejecución.
Si el conjunto de datos ha variado se procederá a comprobar, registro a registro,
cuál ha sido modificado.

# Como ejecutar los tests.
Debes ejecutar desde el directorio src.

python -m unittest discover -s tests

# Mocking de servicios de google API.
Para poder hacer un mocking necesitas los archivos de definición de servicios del Google Discovery API.
Tienes que descargarlos en formato JSON.

* Descarga de definiciones discovery: (https://developers.google.com/discovery/v1/reference/apis/getRest?apix_params=%7B%22api%22%3A%22drive%22%2C%22version%22%3A%22v3%22%7D)

## Interesante para ver los logs de git
[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
lg = !"git lg1"