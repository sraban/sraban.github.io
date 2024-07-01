# Angular Cheat Sheet

## Basic Concepts

### Angular Modules
- **Root Module**: `AppModule`
- **Feature Modules**: For encapsulating functionality by domain.
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Components
- **Declaration**: `@Component`
- **Template**: HTML content.
- **Styles**: Scoped CSS.
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'my-angular-app';
}
```

### Services
- **Provider**: Singleton service.
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  getData() {
    return ['Data1', 'Data2', 'Data3'];
  }
}
```

### Data Binding
- **Interpolation**: `{{ property }}`
- **Property Binding**: `[property]="value"`
- **Event Binding**: `(event)="handler($event)"`
- **Two-way Binding**: `[(ngModel)]="property"`

### Directives
- **Structural Directives**: `*ngIf`, `*ngFor`
- **Attribute Directives**: `[ngClass]`, `[ngStyle]`
```html
<div *ngIf="condition">Conditionally displayed content</div>
<div *ngFor="let item of items">{{ item }}</div>
```

### Pipes
- **Usage**: `{{ value | pipeName }}`
- **Built-in Pipes**: `date`, `uppercase`, `json`
```html
<p>{{ birthday | date:'longDate' }}</p>
```

### Routing
- **Define Routes**:
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
- **Router Outlet**: `<router-outlet></router-outlet>`
- **Router Link**: `<a routerLink="/about">About</a>`

### Forms
- **Template-driven Forms**:
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-form',
  template: `
    <form #form="ngForm" (ngSubmit)="onSubmit(form)">
      <input name="name" ngModel>
      <button type="submit">Submit</button>
    </form>
  `
})
export class FormComponent {
  onSubmit(form: any) {
    console.log(form.value);
  }
}
```
- **Reactive Forms**:
```typescript
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  template: `
    <form [formGroup]="form" (ngSubmit)="onSubmit()">
      <input formControlName="name">
      <button type="submit">Submit</button>
    </form>
  `
})
export class ReactiveFormComponent {
  form = new FormGroup({
    name: new FormControl('')
  });

  onSubmit() {
    console.log(this.form.value);
  }
}
```

## Angular CLI Commands

### Basic Commands
- **New Project**: `ng new project-name`
- **Serve Project**: `ng serve`
- **Generate Component**: `ng generate component component-name`
- **Generate Service**: `ng generate service service-name`
- **Build Project**: `ng build`
- **Run Tests**: `ng test`
- **Lint Project**: `ng lint`

### Advanced Commands
- **Update Angular**: `ng update @angular/cli @angular/core`
- **Add Library**: `ng add library-name`
- **E2E Testing**: `ng e2e`

## Advanced Topics

### Angular Material
- **Install**: `ng add @angular/material`
- **Import Module**:
```typescript
import { MatButtonModule } from '@angular/material/button';

@NgModule({
  imports: [MatButtonModule]
})
export class MyModule { }
```

### Lazy Loading
- **Feature Module**: 
```typescript
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) }
];
```

### State Management with NgRx
- **Install NgRx**: `ng add @ngrx/store`
- **Setup Store**:
```typescript
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './counter.reducer';

@NgModule({
  imports: [StoreModule.forRoot({ count: counterReducer })]
})
export class AppModule { }
```
- **Reducer**:
```typescript
import { createReducer, on } from '@ngrx/store';
import { increment, decrement } from './counter.actions';

export const initialState = 0;

const _counterReducer = createReducer(
  initialState,
  on(increment, (state) => state + 1),
  on(decrement, (state) => state - 1)
);

export function counterReducer(state: number | undefined, action: Action) {
  return _counterReducer(state, action);
}
```

## Performance Optimization

### OnPush Change Detection
- **Component Change Detection**:
```typescript
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MyComponent { }
```

### Lazy Loading
- Use lazy loading for modules to improve load times and performance.

### Optimize Bundle Size
- Use AOT (Ahead-of-Time) compilation and minification.
```bash
ng build --prod
```

### Avoid Direct DOM Manipulation
- Use Angular's `Renderer2` for safe and platform-independent DOM manipulation.

## Common Problems and Solutions

### Error: Can't bind to 'ngModel' since it isn't a known property
- **Solution**: Import `FormsModule` in your module.
```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [FormsModule]
})
```

### Dependency Injection Errors
- **Solution**: Ensure the service is correctly provided either in `@Injectable` or in a module.

### Lifecycle Hook Errors
- **Solution**: Ensure hooks like `ngOnInit` or `ngOnDestroy` are correctly implemented.

## Best Practices

### Component and Module Organization
- Organize components and modules by feature or domain.

### Use Angular CLI
- Use Angular CLI for generating components, services, and other Angular constructs to maintain consistency.

### Keep Business Logic out of Components
- Use services to handle business logic, keeping components focused on presentation.

### Avoid Memory Leaks
- Unsubscribe from observables and detach event listeners to avoid memory leaks.

### Follow Angular Style Guide
- Adhere to Angular’s official style guide for consistent and maintainable code.

## Debugging Tips

### Angular DevTools
- Use Angular DevTools for in-depth debugging and performance profiling.

### Enable Strict Mode
- Enable strict mode for better type checking.
```typescript
ng new my-angular-app --strict
```

### Use `ng serve` with `--prod` for Performance Testing
```bash
ng serve --prod
```

## Angular Versions and Features

### Angular 17 Features
- **Signals**: Experimental feature for managing state.
- **Standalone Components**: Components without NgModule dependencies.
- **Improved SSR**: Enhanced Server-Side Rendering with hydration.
- **ESM Output**: Modern module compatibility with ESM output.

## Helpful Resources

- **Official Documentation**: [Angular Docs](https://angular.io/docs)
- **Angular GitHub**: [Angular Repo](https://github.com/angular/angular)
- **Angular Blog**: [Angular Blog](https://blog.angular.io/)

---

This cheat sheet covers essential concepts and practices for Angular development. For more detailed information, always refer to the [official Angular documentation](https://angular.io/docs).

Certainly! Here’s a comprehensive cheat sheet for Angular 17, covering its core concepts, features, and updates. This cheat sheet is designed to provide quick reference and helpful insights for developers working with Angular 17.

---

# Angular 17 Cheat Sheet

## Getting Started

### Install Angular CLI
```bash
npm install -g @angular/cli
```

### Create a New Angular Project
```bash
ng new my-angular-app
cd my-angular-app
ng serve
```

### Update to Angular 17
```bash
ng update @angular/cli @angular/core
```

## Core Concepts

### Modules
- **Root Module**: `AppModule`
- **Feature Modules**: Organize code by features.
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Components
- **Declaration**: `@Component`
- **Template**: HTML within a component.
- **Styles**: CSS scoped to the component.
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'my-angular-app';
}
```

### Services
- **Provider**: Singleton instance available throughout the app.
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  getData() {
    return ['Data1', 'Data2', 'Data3'];
  }
}
```

### Dependency Injection
- Use `@Injectable()` and specify `providedIn`.
```typescript
@Injectable({
  providedIn: 'root'
})
export class ExampleService { }
```

## Routing

### Define Routes
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

### Router Outlet and Links
```html
<router-outlet></router-outlet>
<a routerLink="/about">About</a>
```

## Forms

### Template-Driven Forms
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-form',
  template: `
    <form #form="ngForm" (ngSubmit)="onSubmit(form)">
      <input name="name" ngModel>
      <button type="submit">Submit</button>
    </form>
  `
})
export class FormComponent {
  onSubmit(form: any) {
    console.log(form.value);
  }
}
```

### Reactive Forms
```typescript
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  template: `
    <form [formGroup]="form" (ngSubmit)="onSubmit()">
      <input formControlName="name">
      <button type="submit">Submit</button>
    </form>
  `
})
export class ReactiveFormComponent {
  form = new FormGroup({
    name: new FormControl('')
  });

  onSubmit() {
    console.log(this.form.value);
  }
}
```

## HttpClient

### Import and Setup
```typescript
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule { }
```

### Making HTTP Requests
```typescript
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  constructor(private http: HttpClient) { }

  getData() {
    return this.http.get('https://api.example.com/data');
  }
}
```

## Angular CLI Commands

### Common Commands
- **Generate Component**: `ng generate component my-component`
- **Generate Service**: `ng generate service my-service`
- **Build Project**: `ng build`
- **Run Tests**: `ng test`
- **Lint Project**: `ng lint`

## Angular 17 Features and Updates

### Signals (Experimental Feature)
- Signals are a new way to manage state in Angular components.
```typescript
import { signal, computed } from '@angular/core';

@Component({
  selector: 'app-signal-example',
  template: `<p>{{ countSignal() }}</p>`
})
export class SignalExampleComponent {
  countSignal = signal(0);

  increment() {
    this.countSignal(this.countSignal() + 1);
  }
}
```

### Angular Standalone Components
- Components without the need for NgModule.
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-standalone',
  templateUrl: './standalone.component.html',
  styleUrls: ['./standalone.component.css'],
  standalone: true
})
export class StandaloneComponent { }
```

### Improved SSR with Hydration
- Enhanced Server-Side Rendering (SSR) with client-side hydration support.
```typescript
import { provideServerRendering } from '@angular/platform-server';
import { AppComponent } from './app.component';

@NgModule({
  providers: [provideServerRendering(AppComponent)],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Updated RxJS
- RxJS 7 support for better performance and usability.

### ESM Module Output
- Angular CLI now produces ESM output by default, ensuring modern module compatibility.

## Best Practices

### Use Angular Material for UI Components
```bash
ng add @angular/material
```
- Import components from Angular Material for a consistent UI.

### Lazy Loading Modules
- Use lazy loading to optimize application performance.
```typescript
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) }
];
```

### Avoid Direct DOM Manipulation
- Use Angular’s Renderer2 for DOM manipulation.
```typescript
import { Renderer2 } from '@angular/core';

constructor(private renderer: Renderer2) { }

changeColor(element: ElementRef) {
  this.renderer.setStyle(element.nativeElement, 'color', 'blue');
}
```

### Use OnPush Change Detection
- For performance improvements, use OnPush strategy where applicable.
```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### Prefer Reactive Forms for Complex Forms
- Use reactive forms for better scalability and control.

## Debugging Tips

### Angular DevTools
- Use Angular DevTools for in-depth debugging and performance profiling.

### Enable Strict Mode
- Enable strict mode for better type checking.
```typescript
ng new my-angular-app --strict
```

### Use `ng serve` with `--prod` for Performance Testing
```bash
ng serve --prod
```

## Common Issues and Troubleshooting

### Dependency Injection Errors
- Ensure services are provided in the correct module or component.

### Lifecycle Hooks
- Use appropriate lifecycle hooks for component initialization and destruction.
```typescript
ngOnInit() {
  // Component initialization
}

ngOnDestroy() {
  // Component cleanup
}
```

### Router Issues
- Ensure paths and components are correctly specified in the routing module.

---

This cheat sheet provides a snapshot of key concepts and features in Angular 17. For more detailed information, always refer to the [official Angular documentation](https://angular.io/docs).

