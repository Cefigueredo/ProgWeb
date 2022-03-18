### Instalación

Desde una consola ingrese el comando `npm install -g @angular/cli`.

Para crear un nuevo proyecto Angular ingrese `ng new News`. 

Asegúrese de indicar que no desea agregar una ruta angular y deje CSS como el formato para las hojas de estilo.

Para ejectutar, vaya a la carpeta News e ingrese `ng serve`. Esto iniciará un servidor web que escucha peticiones en el puerto 4200.

### Sobre la aplicación

Se va a construir una aplicación que mostrará un conjunto de fuentes de noticias. Cuando el usuario haga clic sobre una fuente, se debe desplegar el listado de noticias de esa fuente.

Para ello se usará la API provista por `https://newsapi.org`. [Regístrese en la página newsapi.org](https://newsapi.org) y obtenga su _ApiKey_.

### Pasos

Modifique el archivo `src/index.html` y coloque allí la _starter template_ provista por bootstrap. Asegúrese de incluir luego de la etiqueta `<body>` la etiqueta  `<app-root></app-root>`. Este será el lugar que ocupe el componente principal de la página.

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
	<base href="/">
    <title>News App</title>
  </head>
  <body>
    <app-root></app-root>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
  </body>
</html>
```
Cree un nuevo módulo denominado `Source`. Para ello, desde la consola ingrese `ng generate module source`.

Cree un nuevo componente para listar las fuentes. Para ello, desde la consola ingrese `ng generate component source/source-list`.

Modifique el archivo `src/app/app.component.html` para que en la página principal se renderize el componente de listar.

```
<app-source-list></app-source-list>
```
Este cambio debe mostar un error por consola indicando que `'app-news-list' is not a known element`.

Para corregir el error, importe el módulo SourceModule en `src/app/app.module.ts` y agregue el módulo al arreglo `imports` así:

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { SourceModule } from "./source/source.module";

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule, SourceModule, HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

Luego, exporte el componente `SourceListComponent` en el archivo `src/app/source/source.module.ts` así:

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { SourceListComponent } from './source-list/source-list.component';

@NgModule({
  declarations: [SourceListComponent],
  imports: [
    CommonModule
  ],
  exports: [SourceListComponent]
})
export class SourceModule { }
```
Ahora se requiere obtener los datos de la Api. Para ello se debe crear un servicio. Ejectue `ng generate service source/source`.

Modifique el archivo `src/source/source.service.ts` así:

```typescript
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";

const key = ""; //Coloque aquí la Key que obtuvo al registrarse
const sourceEndpoint = "https://newsapi.org/v2/sources?apiKey=" + key;

@Injectable({
  providedIn: "root"
})
export class SourceService {

  constructor(private http: HttpClient) { }

  getSources(): Observable<any> {
    return this.http.get(sourceEndpoint);
  }
}
```
No olvide colocar en la constante key la llave que obtuvo al registrarse en la pagina NewsApi.

Modifique el componente `src/app/source/source-list.component.ts` para que pueda conectarse al servicio y traer los datos.

```typescript
import { Component, OnInit } from "@angular/core";

import { SourceService } from "../source.service";

@Component({
  selector: "app-source-list",
  templateUrl: "./source-list.component.html",
  styleUrls: ["./source-list.component.css"]
})
export class SourceListComponent implements OnInit {

  constructor(private sourceService: SourceService) { }

  sources: any[];

  getSources() {
   this.sourceService.getSources().subscribe( data => {
    this.sources = data.sources.slice(0, 9);
   });
  }

  ngOnInit() {
  	this.getSources();
  }
}
```

Finalmente, se debe modificar la vista para mostrar los datos obtenidos desde el servicio. Edite el archivo `src/app/source/source-list/source-list.component.html`.

```html
<div class="container">
  <div class="row">
    <div class="col-sm-6">
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">Name</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody *ngFor="let source of sources">
          <tr>
            <td> {{source.name}} </td>
            <td> {{source.description}} </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
```

### Mostrar detalles

Para mostar las noticias de una fuente en particular se require crear un nuevo componente: `ng generate component source/source-detail`.

Modifique el archivo `src/cource/source-list/source-list.component.html` para incluir la directiva `<router-outlet></router-outlet>`. En esta directiva se mostrarán las noticias de la fuente seleccionada.

```html
<div class="container">
  <div class="row">
    <div class="col-sm-6">
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">Name</th>
            <th scope="col">Description</th>
          </tr>
        </thead>
        <tbody *ngFor="let source of sources">
          <tr>
            <td>
              <a routerLink="/source/{{source.id}}" >{{source.name}} </a> 
            </td>
            <td> {{source.description}} </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="col-sm-6">
      <router-outlet></router-outlet>
    </div>
  </div>
</div>
```

Modifique `src/app/app.module.ts` para incluir el detalle de las rutas.

```typescript
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { HttpClientModule } from "@angular/common/http";
import { RouterModule, Routes} from "@angular/router";


import { AppComponent } from "./app.component";
import { SourceModule } from "./source/source.module";
import { SourceDetailComponent } from "./source/source-detail/source-detail.component";

const appRoutes: Routes = [
  { path: "source/:id", component: SourceDetailComponent }
];

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule, SourceModule, HttpClientModule, RouterModule.forRoot(
      appRoutes,
      { enableTracing: false } // <-- debugging purposes only
    )
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Modifique `src/app/source/source.module.ts` para que pueda usar la directiva `routerLink`:

```typescript
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { SourceListComponent } from "./source-list/source-list.component";
import { SourceDetailComponent } from "./source-detail/source-detail.component";
import { RouterModule} from "@angular/router";

@NgModule({
  declarations: [SourceListComponent, SourceDetailComponent],
  imports: [
    CommonModule, RouterModule
  ],
  exports: [SourceListComponent]
})
export class SourceModule { }
```

Modifique el servicio para crear un nuevo método que obtenga las noticias de una fuente específica.

```typescript
import { Injectable } from "@angular/core";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";

const key = ""; //Coloque aquí la Key que obtuvo al registrarse
const sourceEndpoint = "https://newsapi.org/v2/sources?apiKey=" + key;
const headlinesEndpoint = "https://newsapi.org/v2/top-headlines?apiKey=" + key;


@Injectable({
  providedIn: "root"
})
export class SourceService {

  constructor(private http: HttpClient) { }

  getSources(): Observable<any> {
    return this.http.get(sourceEndpoint);
  }

  getDetails(id: string): Observable<any> {
    return this.http.get(headlinesEndpoint + "&sources=" + id);
  }
}
```

Incluya el siguiente código en el archivo `src/app/source/source-detail/source-detail.component.ts`:

```typescript
import { Component, OnInit } from "@angular/core";
import { ActivatedRoute, ParamMap } from "@angular/router";

import { SourceService} from "../source.service";
import { Observable } from "rxjs";

@Component({
  selector: "app-source-detail",
  templateUrl: "./source-detail.component.html",
  styleUrls: ["./source-detail.component.css"]
})
export class SourceDetailComponent implements OnInit {

  constructor(
  	private route: ActivatedRoute, private service: SourceService) { }

  detail: any = {};

  ngOnInit() {
    this.route.params.subscribe(routeParams => {
    	this.service.getDetails(routeParams.id).subscribe(cls => {
    		console.log(cls);
    		this.detail = cls;
    	});
    });
  }
}
```

Finalmente modifique src/source/source-detail/source-detail.component.html

```html
<h3>News</h3>
<div *ngFor="let article of detail.articles">
	<div class="card" style="width: 18rem;">
	  <img src="{{article.urlToImage}}" class="card-img-top" alt="...">
	  <div class="card-body">
	    <h5 class="card-title">{{article.title}}</h5>
	  </div>
	</div>
</div>
```
