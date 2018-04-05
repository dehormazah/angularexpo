# Tutorial paso a paso para entender y completar una aplicación de Angular a partir de una plantilla

## Introducción

<p align="justify">
Angular es un framework de desarrollo para JavaScript creado por Google. La finalidad de Angular es facilitarnos el desarrollo de aplicaciones web SPA y además darnos herramientas para trabajar con los elementos de una web de una manera más sencilla y óptima.</p><br>

<p align="justify">
El propósito de este tutorial es explicar de manera sencilla e ilustrativa los diferentes elementos que conforman una aplicación construida sobre <b>Angular</b> como los componentes y sus atributos, los módulos, las directivas, los servicios, entre otros.</p>

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
 
## Entendiendo Angular
<p align="justify">
Como puede observar, la aplicación no es muy atractiva por ahora, apenas contiene un menú de navegación para ir entre las vistas Inicio, Iniciar Sesión y Registrarse así como algunos formularios para introducir texto en las dos últimas vistas mencionadas. Nuestra tarea es completar la plantilla base de dicha aplicación para visualizar el resultado final y poder realizar acciones como registrar un usuario nuevo, iniciar sesión, modificar el perfil de un usuario y cerrar sesión sin perder la información de un usuario registrado para tener la posibilidad de volver a entrar a la aplicación luego de ello.</p>

### Estructura de un componente

Un componente está compuesto por tres partes fundamentales: 

- Un template
- Una clase
- Una función decoradora

<p align="justify">Las dos primeras partes corresponden con capas de lo que conocemos como MVC (Modelo-Vista-COntrolador). El template será lo que se conoce como vista y se escribe en HTML y lo que correspondería con el controlador se escribe en Javascript por medio de una clase (de programación orientada a objetos).

Por su parte, tenemos el decorador, que es una especie de registro del componente y que hace de "pegamento" entre el Javascript y el HTML.</p>


Para confirmar lo anterior, puede navegar entre las carpetas ubicadas en ```src/app/``` que definen los componentes de la aplicación: ```auth, home y settings```. Como podrá ver, en cada una de ellas hay al menos dos archivos con extensiones: ```.ts y .html```, en algunas de ellas hay incluso otro archivo ```.css``` (opcional) que define los estilos vinculados a los elementos HTML. El archivo ```.ts``` define la clase del componente y contiene ciertas funciones, métodos y otras declaraciones necesarias para el funcionamiento correcto del componente y su respectiva vista. Para profundizar en lo anterior y ver la organización de un componente con un ejemplo concreto, vamos a estudiar el código en ```src/app/home/home.component.ts```: <br>

```typescript
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';

import { UserService } from '../shared';

@Component({
  selector: 'app-home-page',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  constructor(
    private router: Router,
    private userService: UserService
  ) {}

  isAuthenticated: boolean;

  ngOnInit() {
    this.userService.isAuthenticated.subscribe(
      (authenticated) => {
        this.isAuthenticated = authenticated;

      }
    );
  }
}

```

### Decorador de un componente y Servicios

En primer lugar vemos que hay tres sentencias que comienzan con la palabra ```import```, las cuales se encargan de importar otros componentes que son necesarios para que el componente actual tenga acceso a ciertas funciones y otros elementos. En este caso, se importan las clases ```Component``` y ```Router``` y la interfaz ```OnInit```, además del servicio ```UserService```. 
Los <b>servicios</b> son clases TypeScript. Su propósito es contener lógica de negocio, clases para acceso a datos o utilidades de infraestructura. Estas clases son perfectamente instanciables desde cualquier otro fichero que las importe. La particularidad de las clases de servicios está en su decorador: ```@Injectable()```. Esta función viene en el ```@angular/core``` e indica que esta clase puede ser inyectada dinámicamente a quien la demande.
<br>
Avanzando por el código encontramos el decorador  ```@Component```. Angular usa los decoradores para registrar un componente, añadiendo información para que éste sea reconocido por otras partes de la aplicación. La forma de un decorador es la siguiente: 
```typescript
@Component({
  selector: 'app-home-page',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
```
En el decorador estamos agregando diversas propiedades específicas del componente. Esa información en este caso concreto se conoce como "anotación" y lo que le entregamos son unos "metadatos" que no hacen más que describir al componente que se está creando. En este caso son los siguentes:

- selector: este es el nombre de la etiqueta nueva que crearemos cuando se procese el componente. Es la etiqueta que usarás cuando quieras colocar el componente en cualquier lugar del HTML.
- templateUrl: es el nombre del archivo .html con el contenido del componente, en otras palabras, el que tiene el código de la vista.
- styleUrls: es un array con todas las hojas de estilos CSS que deben procesarse como estilo local para este componente. Como ves, podríamos tener una única declaración de estilos, o varias si lo consideramos necesario.


 
