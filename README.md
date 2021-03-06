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

En caso de querer cambiar de usuario (cerrar sesión del usuario actual) se llama a la función ```purgeAuth```, la cual elimina el token del usuario actual, le da al atributo ```currentUser``` el valor de Objeto vacío y al atributo ```isAuthenticated``` el valor ```false```, para finalmente cerrar la sesión previa y permitir que otro usuario pueda ingresar a la aplicación. <br>

Cuando se quiere actualizar la información de un usuario (nombre, biografía, contraseña, etc. <b>VER archivo correspondiente al modelo de</b> ```User``` <b>ubicado en</b> ```src/app/shared/models/user.model.ts```) se emplea el método ```update```, el cual realiza una petición tipo PUT a la API con la respectiva información que quiere actualizarse para el usuario que está en sesión actualmente. 

### Directivas y su funcionalidad

Para que el usuario sepa si se encuentra "loggeado" o no, se necesita mostrar diferentes partes de la interfaz de usuario cuando los usuarios inicien sesión (botones de iniciar/ cerrar sesión, etc.) Para ello se crea una directiva, la cual nos permite mostrar u ocultar elementos HTML según ciertos parámetros. 

Las <b>directivas</b> de Angular son básicamente funciones que son invocadas cuando el DOM (Document Object Model) es compilado por el framework de Angular. Se podría decir que las directivas están ligadas a sus correspondientes elementos del DOM cuando el documento es cargado.  La finalidad de una directiva es modificar o crear un comportamiento totalmente nuevo. En Angular, una directiva se define como una clase que utiliza el decorador ```@Directive```. 
<br>

Para entender mejor lo anterior, lo ilustraremos con nuestra plantilla de ejemplo. Vamos a darle un vistazo al archivo ```header.component.html``` ubicado en ```src/app/shared/layout/header.component.html```. Dicho archivo corresponde a la vista definida para el componente ```HeaderComponent```:

```html

 <nav class="navbar navbar-light">
  <div class="container">

    <!-- Esto se muestra para los usuarios que no han iniciado sesion -->
    <ul *appShowAuthed="false"
      class="nav navbar-nav pull-xs-right">

      <li class="nav-item">
        <a class="nav-link"
          routerLink="/">
          Inicio
        </a>
      </li>

      <li class="nav-item">
        <a class="nav-link"
          routerLink="/login"
          routerLinkActive="active">
          Iniciar Sesión
        </a>
      </li>

      <li class="nav-item">
        <a class="nav-link"
          routerLink="/register"
          routerLinkActive="active">
          Registrarse
        </a>
      </li>

    </ul>

    <!-- Esto se muestra para usuarios que iniciaron sesion-->
    <ul *appShowAuthed="true"
      class="nav navbar-nav pull-xs-right">

      <li class="nav-item">
        <a class="nav-link"
          routerLink="/"
          routerLinkActive="active"
          [routerLinkActiveOptions]="{ exact: true }">
          Inicio
        </a>
      </li>


      <li class="nav-item">
        <a class="nav-link"
          routerLink="/settings"
          routerLinkActive="active">
          <i class="ion-gear-a"></i>&nbsp;Perfil
        </a>
      </li>

      <li class="nav-item">
        <a class="nav-link"
          [routerLink]="['/profile', currentUser.username]"
          routerLinkActive="active">
          <img [src]="currentUser.image" *ngIf="currentUser.image" class="user-pic" />
          {{ currentUser.username }}
        </a>
      </li>

    </ul>

  </div>
</nav>
  ```
Esta vista corresponde al header de nuestra aplicación, es decir, el menú de navegación que usted puede observar en la parte superior de la interfaz de la aplicación. Como nos interesa comprender de qué manera funcionan las directivas, observemos el código. Como se puede ver arriba, hay dos secciones distintas a mostrar en el header (vea los comentarios que indican cuál sección es para un usuario que ha iniciado sesión y cuál para uno que no lo ha hecho). Ahora bien, la pregunta es cómo se decide cuál de las dos secciones mostrar en la interfaz. Fíjese en la directiva definida por ```*appShowAuthed```:

```html

    <!-- Esto se muestra para los usuarios que no han iniciado sesion -->
    <ul *appShowAuthed="false"
      class="nav navbar-nav pull-xs-right">
```
Esta es una directiva de tipo estructural. Estas directivas, que se diferencian fácilmente al ser precedidas por un asterisco, alteran el layout añadiendo, eliminando o reemplazando elementos en el DOM. Un par de ejemplos de dichas directivas son:

- *ngIf: si la condición se cumple, su elemento se inserta en el DOM, en caso contrario, se elimina del DOM. 
- *ngFor: repite su elemento en el DOM una vez por cada item que hay en el iterador que se le pasa, siguiendo una sintaxis de ES6. 
 
 Volviendo a nuestro ejemplo,cuando ```*appShowAuthed``` sea igual a ```true``` se mostrará la sección que debe ver un usuario que ha iniciado sesión y cuando sea igual a ```false``` se mostrarán los elementos que debe visualizar un usuario que aun no lo ha hecho. 
 
 Examinemos la clase de la directiva en mención. Para ello nos dirigimos al archivo ```show-authed.directive.ts``` en 
```src/app/shared/show-authed.directive.ts```. El selector de esta directiva es ```appShowAuthed```, que es la directiva estructural usada en el componente header como se describió anteriormente:


```typescript
@Directive({ selector: '[appShowAuthed]' })
export class ShowAuthedDirective implements OnInit {
  constructor(
    private templateRef: TemplateRef<any>,
    private userService: UserService,
    private viewContainer: ViewContainerRef
  ) {}
```
La directiva también posee un constructor, en el cual se inicializan atributos determinados de la misma. El siguiente trozo de código se encarga de determinar el valor de ```appShowAuthed``` (```true``` o ```false```) consultando a través de la variable ```isAuthenticated``` si el usuario se ha autenticado o no.

```typescript
  ngOnInit() {
    this.userService.isAuthenticated.subscribe(
      (isAuthenticated) => {
        if (isAuthenticated && this.condition || !isAuthenticated && !this.condition) {
          this.viewContainer.createEmbeddedView(this.templateRef);
        } else {
          this.viewContainer.clear();
        }
      }
    );
  }
  ```
  
  Ahora bien, luego de haber estudiado y comprendido algunos de los elementos principales de Angular y de mostrar algunas de sus funcionalidades en una aplicación, completaremos la plantilla de código de nuestro ejemplo para verlo funcionando y poder interactuar con él.
  
### Completando la plantilla de ejemplo

En esta sección nos encargaremos de agregar código a nuestra plantilla para completar las vistas de la aplicación y poder hacer uso de su interfaz y funcionalidades.
<br>
En primer lugar, editaremos el archivo ```home.component.html``` ubicado en ```src/app/home/home.component.html```. Esta es la vista del inicio de nuestra aplicación, la cual está definida por el selector ```app-home-page```:

```html
<div class="home-page">

  <div class="banner" *appShowAuthed="false">
    <div class="container">


      
    </div>

  </div>


</div>
```
vamos a editar el contenido de las etiquetas ```div``` para mostrar el nombre y un "slogan" de nuestra aplicación. El resultado sería el siguiente:

```html
<div class="home-page">

  <div class="container">
      <h1 class="title">NOMBRE DE LA APP</h1>
      <h3 class="phrase"><i>Slogan</i></h3>
    </div>

  </div>


</div>
```
Tras haber guardado dichos cambios, usted podrá visualizar la aplicación en ```http://localhost:4200/``` con la nueva vista para la sección de Inicio.

Ahora agregaremos código en la vista definida por el archivo ```auth.component.html``` ubicado en ```src/app/auth/auth.component.html```, vista que corresponde a las secciones de Registrarse e Iniciar Sesión. Usted puede remplazar el contenido existente en dicho archivo por el siguiente código, que pasaremos a explicar a continuación: 

 ```html
<div class="auth-page">
  <div class="container page">
    <div class="row">

      <div class="col-md-6 offset-md-3 col-xs-12">
        <h1 class="text-xs-center">{{ title }}</h1>
        <p class="text-xs-center">
          <a [routerLink]="['/login']" *ngIf="authType == 'register'">Tiene una cuenta?</a>
          <a [routerLink]="['/register']" *ngIf="authType == 'login'">Cree una cuenta nueva</a>
        </p>
        <app-list-errors [errors]="errors"></app-list-errors>
        <form [formGroup]="authForm" (ngSubmit)="submitForm()">
          <fieldset [disabled]="isSubmitting">
            <fieldset class="form-group">
              <input
                formControlName="username"
                placeholder="nombre de usuario"
                class="form-control form-control-lg"
                type="text"
                *ngIf="authType == 'register'" />
            </fieldset>
            <fieldset class="form-group">
              <input
                formControlName="email"
                placeholder="correo electrónico"
                class="form-control form-control-lg"
                type="text" />
            </fieldset>
            <fieldset class="form-group">
              <input
                formControlName="password"
                placeholder="contraseña"
                class="form-control form-control-lg"
                type="password" />
            </fieldset>
            <button class="btn btn-lg btn-primary pull-xs-right" [disabled]="!authForm.valid" type="submit">
              {{ title }}
            </button>
          </fieldset>
        </form>
      </div>

    </div>
  </div>
</div>
```
Como podemos ver, la vista está conformada por etiquetas de tipo ```div```, así como formularios compuestos por distintos campos en los cuales el usuario podrá ingresar sus credenciales (nombre de usuario, email, contraseña). El valor de la variable ```title``` que se encuentra en la primera etiqueta ```h1``` de arriba hacia abajo es definido en la función ```ngOnInit``` en la clase ```auth.component.ts``` según la ruta a la que se dirija el usuario (Iniciar Sesión o Registro). La directiva ```[routerLink]``` determina el link que debe mostrarse según la ruta en la que se encuentre el usuario. Si dicha ruta corresponde al login, el texto del link será "Tiene una cuenta?", en caso contrario: "Cree una cuenta nueva". Cuando se hayan completado todos los campos necesarios, el botón que se encuentra descrito en la última parte del código llamará a la función ```submitForm``` que es empleada por el método que ya consideramos anteriormente ```attemptAuth```, que se encarga de validar la autenticación del usuario a partir de las credenciales ingresadas por este en los campos de texto.
<br><br>
Cuando termine de hacer esto, usted podrá ver que puede navegar entre las opciones de Iniciar Sesión y Registrarse, y ahora puede crear una "cuenta" con un nickname, un correo electrónico y una contraseña. Sin embargo, aun no puede ver su perfil de usuario ni comprobar que su cuenta ha sido registrada y autenticada por la aplicación, y que puede cerrar sesión y luego volver a ingresar al sistema sin ningún problema.<br><br>
Para esto, editaremos el código del archivo ```settings.component.html``` ubicado en ```src/app/settings/settings.component.html```. Usted puede remplazar el contenido existente en dicho archivo por el siguiente código, que estudiaremos en seguida:

```html
<div class="settings-page">
  <div class="container page">
    <div class="row">
      <div class="col-md-6 offset-md-3 col-xs-12">

        <h1 class="text-xs-center">Su información</h1>

        <app-list-errors [errors]="errors"></app-list-errors>

        <form [formGroup]="settingsForm" (ngSubmit)="submitForm()">
          <fieldset [disabled]="isSubmitting">

            <fieldset class="form-group">
              <input class="form-control"
                type="text"
                placeholder="URL de la foto de perfil"
                formControlName="image" />
            </fieldset>

            <fieldset class="form-group">
              <input class="form-control form-control-lg"
                type="text"
                placeholder="nombre de usuario"
                formControlName="username" />
            </fieldset>

            <fieldset class="form-group">
              <textarea class="form-control form-control-lg"
                rows="8"
                placeholder="escriba algo sobre usted"
                formControlName="bio">
              </textarea>
            </fieldset>

            <fieldset class="form-group">
              <input class="form-control form-control-lg"
                type="email"
                placeholder="correo electrónico"
                formControlName="email" />
            </fieldset>

            <fieldset class="form-group">
              <input class="form-control form-control-lg"
                type="password"
                placeholder="nueva contraseña"
                formControlName="password" />
            </fieldset>

            <button class="btn btn-lg btn-primary pull-xs-right"
              type="submit">
              Actualizar
            </button>

          </fieldset>
        </form>

        <!-- Boton para cerrar sesion -->
        <hr />

        <button class="btn btn-outline-danger"
          (click)="logout()">
          Cerrar Sesión
        </button>

      </div>
    </div>
  </div>
</div>
```
Este trozo de código es muy similar al que editamos en el archivo ```auth.component.html```, contiene etiquetas ```div``` así como formularios y campos para introducir texto. Esta vista corresponde al perfil del usuario. Allí, el usuario puede editar su información (nombre de usuario, email, contraseña, biografía e imagen de perfil) y puede guardar dichos cambios para actualizar la base de datos. En el campo para guardar una imagen de perfil, el usuario puede pegar la URL de una imagen en la web. Los elementos campos de texto simplemente servirán para retener la información que el usuario introduzca. Lo que nos interesa aquí es ver cómo se actualizan los datos en la API. El primer botón que encontramos, está asociado al grupo de formularios ```settingsForm```. AL hacer click sobre dicho botón, se llama a la función ```submitForm``` que le pasa a la función ```update``` del servicio ```UserService``` la información que el usuario desea actualizar en su perfil. La función ```update``` se encarga de realizar la petición tipo PUT a la API para actualizar un usuario específico en la base de datos.
<br><br>
Por último, el botón de Cerrar Sesión, hace un llamado a la función ```logout``` definida en ```UserService```, que se encarga de eliminar el token del usuario en la sesión actual, asigna al atributo ```currentUser``` el valor de Objeto vacío y al atributo ```isAuthenticated``` el valor ```false```, para finalmente cerrar la sesión anterior y permitir que otro usuario pueda ingresar a la aplicación.
<br><br>
Ahora nuestra aplicación está terminada y usted puede interactuar completamente con la misma. Puede navegar entre las secciones de Inicio, Registro e Inicio de Sesión; puede crear nuevos usuarios o loggearse con uno de ellos, además de poder editar y actualizar la información de los perfiles de usuario. Para comprobar que los usuarios creados persisten en la base de datos de la API, usted puede cerrar sesión y volver a loggearse con las credenciales de un usuario específico.
