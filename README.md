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

### Decorador de un componente, declaraciones import y export, constructor del componente y Servicios

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

- selector: este es el nombre de la etiqueta nueva que crearemos cuando se procese el componente. Es la etiqueta que se usará cuando se quiera colocar el componente en cualquier lugar del HTML.
- templateUrl: es el nombre del archivo .html con el contenido del componente, en otras palabras, el que tiene el código de la vista.
- styleUrls: es un array con todas las hojas de estilos CSS que deben procesarse como estilo local para este componente. Es posible tener una única declaración de estilos, o varias si se considera necesario.

Luego tenemos el segmento de código:

```typescript
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
En el cual se exporta la clase del componente actual, que hará las veces de controlador y además se implementa la interfaz ```OnInit```, que simplemente indica que dentro de la clase del componente vamos a definir la funcion ```ngOnInit()```, donde podremos colocar código a ejecutar cuando se tenga la certeza que el componente ha sido inicializado ya. A continuación se encuentra el constructor del componente. El <b>constructor</b> es un método por defecto de la clase del componente que se ejecuta cuando la clase es instanciada y asegura una inicialización correcta de los campos en la clase y sus subclases.
<br>
Además encontramos la declaración de una variable usada por la clase llamada ```isAuthenticated``` de tipo ```boolean``` y la función ```ngOnInit()``` de la cual ya habíamos hablado anteriormente.

### Servicios y sus funciones en la aplicación de ejemplo

<p align="justify">
Como lo habíamos mencionado al inicio, la aplicación se comunica con un servicio de backend (API) para consumir la información allí almacenada y poder realizar distintas operaciones a partir de esta. La API está desplegada de forma remota y es capaz de recibir distintas peticiones para crear usuarios, autenticarlos y realizar cambios en sus perfiles.</p>

Para ver las peticiones que pueden hacerse a la API vamos a inspeccionar el archivo ```api.service.ts``` el cual se encuentra ubicado en ```src/app/shared/services/api.service.ts```. El código que nos interesa es el siguiente:

```typescript
  get(path: string, params: HttpParams = new HttpParams()): Observable<any> {
    return this.http.get(`${environment.api_url}${path}`, { params })
      .pipe(catchError(this.formatErrors));
  }

  put(path: string, body: Object = {}): Observable<any> {
    return this.http.put(
      `${environment.api_url}${path}`,
      JSON.stringify(body)
    ).pipe(catchError(this.formatErrors));
  }

  post(path: string, body: Object = {}): Observable<any> {
    return this.http.post(
      `${environment.api_url}${path}`,
      JSON.stringify(body)
    ).pipe(catchError(this.formatErrors));
  }

  delete(path): Observable<any> {
    return this.http.delete(
      `${environment.api_url}${path}`
    ).pipe(catchError(this.formatErrors));
  }
  ```
Allí se encuentran, descritas por dichas funciones, las peticiones HTTP básicas para realizar operaciones de tipo CRUD con los usuarios almacenados en la base de datos de la API. Básicamente, las funciones construyen la petición correspondiente a partir de un atributo ```path``` de tipo ```string``` y un Objeto ```body``` que define el cuerpo de la petición como se hace tradicionalmente. 
<br>
Ahora, veremos el servicio utilizado para acceder a los usuarios, autenticarlos y llevar la información del usuario que se encuentra en sesión activa en cierto momento. Este es el ```UserService```, ubicado en ```src/app/shared/services/user.service.ts```, estudiaremos las funciones que encontramos allí, para comprender de qué manera se hace la autenticación de usuarios y el manejo de sesiones.

```typescript

  populate() {
    // If JWT detected, attempt to get & store user's info
    if (this.jwtService.getToken()) {
      this.apiService.get('/user')
      .subscribe(
        data => this.setAuth(data.user),
        err => this.purgeAuth()
      );
    } else {
      // Remove any potential remnants of previous auth states
      this.purgeAuth();
    }
  }

  setAuth(user: User) {
    // Save JWT sent from server in localstorage
    this.jwtService.saveToken(user.token);
    // Set current user data into observable
    this.currentUserSubject.next(user);
    // Set isAuthenticated to true
    this.isAuthenticatedSubject.next(true);
  }

  purgeAuth() {
    // Remove JWT from localstorage
    this.jwtService.destroyToken();
    // Set current user to an empty object
    this.currentUserSubject.next({} as User);
    // Set auth status to false
    this.isAuthenticatedSubject.next(false);
  }

  attemptAuth(type, credentials): Observable<User> {
    const route = (type === 'login') ? '/login' : '';
    return this.apiService.post('/users' + route, {user: credentials})
      .pipe(map(
      data => {
        this.setAuth(data.user);
        return data;
      }
    ));
  }

  getCurrentUser(): User {
    return this.currentUserSubject.value;
  }

  // Update the user on the server (email, pass, etc)
  update(user): Observable<User> {
    return this.apiService
    .put('/user', { user })
    .pipe(map(data => {
      // Update the currentUser observable
      this.currentUserSubject.next(data.user);
      return data.user;
    }));
  }}
  ```

La función ```attemptAuth``` determina si un usuario ya está registrado o no y según esto, determina una petición de tipo POST a la API a una ruta determinada por ```route```. Si el usuario ya está registrado en la aplicación la ruta para la petición terminará en ```/users/login```, de lo contrario la ruta para hacer POST y crear un nuevo usuario terminará en ```/users```. <br>

Al interior de dicha función también se hace un llamado a ```setAuth``` para realizar la autenticación del usuario. En esa función, usando el servicio ```JwtService``` se obtiene y guarda el token del usuario y se selecciona el nuevo usuario que estará en sesión en la aplicación, además de darle el valor de ```true``` a la variable ```isAuthenticated``` para validar la autenticación del usuario.<br>

Teniendo el token, se realiza una petición tipo POST a la API para crear un usuario o registrar su inicio de sesión (si el usuario ya existe). En caso de que las credenciales ingresadas no correspondan con un usuario creado se muestra un mensaje de error alertando al usuario de ello. Si el usuario ya se ha registrado anteriormente o ingresó las credenciales válidas para registrarse, se obtiene y almacena la información de dicho usuario para mostrar las respectivas vistas en la aplicación. La sesión persiste aunque se recargue la página, debido a la manera en que se hace el control de la misma.<br>

