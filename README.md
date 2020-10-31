# Crash Course : Angular basics

## Installation 

- 1st step : Install nodejs  
Download [Node.js](https://nodejs.org/en/)

- 2nd step : Install Angular command line  
```
npm i -g @angular-cli
```

- 3rd step : Create new project  
```
ng new name_of_my_app
```

  - Would you like to add Angular Routing : Yes
  - Which stylesheet format would you like to use : SCSS

## Run the project 
```
ng serve -o
```
the `-o` flag opens the default browser with hot reloading

## Structure

```
src
   app
       my-component
          my.component.html // structure
          my.component.scss // style
          my.component.spec.ts // test
          my.component.ts // logic

        app-routing.module.ts // define routes
        app.component.html
        app.component.scss
        app.component.spec.ts
        app.component.ts
        app.module.ts // declare and import all components and modules
        http.service.spec.ts
        http.service.ts
```

## Components

To create a new component

```
ng g c component_name // (e.g. ng g c home)
```






## Routing

If you want to create a new route, you have to add it in the routes array in app-routing.module.ts

```
// app-routing.module.ts

...
const routes: Routes = [
  {
    path: '', component:  HomeComponent
  },
  {
    path: 'list', component:  ListComponent
  }
];
...
...
```

Don't forget to import them first

```
// app-routing.module.ts

import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { ListComponent } from './list/list.component';


const routes: Routes = [
  {
    path: '', component:  HomeComponent
  },
  {
    path: 'list', component:  ListComponent
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

```
// home.component.html
...
<h1>Home</h1>
...
```

## Data Binding
```
// home.component.html
<div class="play-container">
    <button class="countButton" (click)="countClick()">
        You've clicked 
        <span>this</span>
    </button>
    
    <span> {{ clickCounter }} times</span>

    <div>
        <button class="resetButton" (click)="resetCounter()">
            Reset counter
        </button>
    </div>
</div>
```


```
// home.component.ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})

export class HomeComponent implements OnInit {
  clickCounter: number = 0;
  name: string = '';

  constructor() {}

  ngOnInit() {}

  countClick() {
    this.clickCounter += 1;
  }
  
  resetCounter() {
    this.clickCounter = 0;
  }
}
```

## Two-Way Data Binding

```
// home.component.html

<div class="play-container">
    <p>
        <input type="text" [ngModel]="name">
        <br>
        <strong> You said : </strong> {{ name }}
    </p>
</div>
```

```
// home.component.ts

...
export class HomeComponent implements OnInit {
  ...
  name: string = '';
  ...
}
```

### NgModel

`ngModel` will allow two-way data binding where the input property is going to both receive and send information from and to the component logic.

In order for `ngModel` to work we have to import `FormsModule`from angular forms.

```
// app.module.ts

import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';


@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ListComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Conditionals

```
// home.component.html
...
<div class="play-container">
    <ng-template [ngIf]="clickCounter > 4" [ngIfElse]="none">
        <p>The click counter <strong>IS GREATER</strong> than 4</p>
    </ng-template>

    <ng-template #none>
       <p> The click counter is <strong>NOT GREATER</strong> than 4</p> 
    </ng-template>
</div>
...
```
To use `NgIf` directivs we have to import `CommonModule`from angular common

```
// app.module.ts
...

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ListComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    CommonModule,
  ],
  providers: [],
...

```

## Style Binding

### NgStyle

```
// home.component.html
<div [ngStyle]="{'background-color': '#000',color': '#fff'}">
    <ng-template [ngIf]="clickCounter > 4" [ngIfElse]="none">
        <p>The click counter <strong>IS GREATER</strong> than 4</p>
    </ng-template>

    <ng-template #none>
       <p> The click counter is <strong>NOT GREATER</strong> than 4</p> 
    </ng-template>
</div>
```

## Class Binding

### NgClass

```
// home.component.html
<div 
    class="play-container"
    [ngStyle]="{'color': '#fff'}"
    [ngClass]="setClasses()"
    >
    <ng-template [ngIf]="clickCounter > 4" [ngIfElse]="none">
        <p>The click counter <strong>IS GREATER</strong> than 4</p>
    </ng-template>

    <ng-template #none>
       <p> The click counter is <strong>NOT GREATER</strong> than 4</p> 
    </ng-template>
</div>
```

```
// home.component.ts

import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.scss']
})

export class HomeComponent implements OnInit {
  clickCounter: number = 0;
  name: string = '';

  constructor() {}

  ngOnInit() {}

  countClick() {
    this.clickCounter += 1;
  }
  
  resetCounter() {
    this.clickCounter = 0;
  }

  setClasses() {
    let myClasses =  {
      active: this.clickCounter > 4,
      notactive : this.clickCounter <= 4
    }
    return myClasses
  }
}
```

```
// home.component.scss
...
.notactive {
    background-color: crimson;
}

.active {
    background-color: mediumseagreen;
}
```

## Services

Services are special components that are reusable throughout your app (e.g. communicating with an API to fetch some data)

To create a service

```
ng g s name_of_service (e.g. http)
```

To perform HTTP request we have to use the `HttpClient` class

```
// http.service.ts

import { Injectable } from '@angular/core';
import {HttpClient} from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class HttpService {

  constructor() { }
}
```
In order to use `HtttpClient`, we have to import it through our app.module file

```
// app.module.ts
...
import { HttpClientModule } from '@angular/common/http';
...

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    ListComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    CommonModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Then we create a function to make the call to the API

```
// http.service.ts

import { Injectable } from '@angular/core';
import {HttpClient} from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class HttpService {
  constructor(private http : HttpClient) { } 

  getBreweries () {
    return this.http.get('https://api.openbrewerydb.org/breweries');
  }
}

```

```
// list.component.ts

import { Component, OnInit } from '@angular/core';
import { HttpService } from '../http.service';

@Component({
  selector: 'app-list',
  templateUrl: './list.component.html',
  styleUrls: ['./list.component.scss']
})
export class ListComponent implements OnInit {

  
  /*
    We create an object that will hold all the result we get from the API

  */

  brews : Object;

  /*
    We create an instance of that class and make it 
    private so we can't access it outside the class

  */
  constructor(private _http: HttpService) { }

  // Lifecycle hook (load when the component is loaded)
  ngOnInit() {
    this._http.getBreweries().subscribe(data => {
      this.brews  = data;
      console.log(data); // just to make sure it's working
    });
  }
}
```

```
// list.component.html

<h1>Breweries</h1>

<ul *ngIf="brews">
    <li *ngFor="let brew of brews">
        <p class="name"> {{ brew.name }}</p>
        <p class="country"> {{ brew.country }}</p>
        <a href="{{ brew.website_url }}" class="site"> Website</a>
    </li>
</ul>
```

## Deployment

```
// Minify the app as much as possible and create a distribution folder in the project

ng build --prod
```

