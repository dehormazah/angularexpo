# Tutorial paso a paso para entender y completar una aplicación de Angular a partir de una plantilla

## Introducción

<p align="justify">
Angular es un framework de desarrollo para JavaScript creado por Google. La finalidad de Angular es facilitarnos el desarrollo de aplicaciones web SPA y además darnos herramientas para trabajar con los elementos de una web de una manera más sencilla y óptima.</p><br>

<p align="justify">
El propósito de este tutorial es explicar de manera sencilla e ilustrativa los diferentes elementos que conforman una aplicación construida sobre <b>Angular</b> como los componentes y sus atributos, los módulos, las directivas, los servicios, entre otros.</p><br>

## Aplicación de ejemplo

<p align="justify">
La aplicación que usted completará a través de este tutorial y visualizará funcionando al final del mismo interactúa con un servidor de backend (API) para gestionar la autenticación de usuarios y el manejo de sesiones. También permite mostrar el perfil de un usuario con varios atributos que pueden ser modificados y actualizados a partir de la interacción con elementos HTML.</p>

## Para empezar
<p align="justify">
Inicialmente, usted deberá clonar el repositorio con la plantilla de código sobre la cual se trabajará en este tutorial, e instalar el CLI de angular para poder ejecutar la aplicación en su máquina de manera local. Para ello ejecute los siguientes comandos:</p>
<br>
``` npm install -g @angular/cli ```<br>
```git init ```<br>
```git clone https://github.com/dehormazah/angularexpo```
  Luego, deberá ubicarse en el directorio creado al clonar el repositorio:
  ```cd angularexpo```<br>
 e instalar las dependencias requeridas con el comando:
 ```npm install ```<br>
 Ahora, ya puede lanzar la aplicación localmente, con el comando:
 ```ng serve --open ```<br>
 Usted podrá visualizar la aplicación en su navegador en ```http://localhost:4200/``` <br>
NOTA: Los cambios que realice sobre los archivos de la aplicación se verán reflejados allí, no es necesario volver a lanzar la aplicación para ello.
 
 
