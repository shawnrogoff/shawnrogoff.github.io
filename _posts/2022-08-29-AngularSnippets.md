---
title: Angular Code Snippets
date: 2022-08-29 10:00:00 -500  
categories: [docs]
tags: [snippets, code] # TAG names should always be lowercase
---
# Useful Angular Code Snippets I've Used:

## Create new project with routing, and no test files:
```shell_session
ng new <project-name> --routing --skip-tests
```

## Import/Add Bootstrap:
```shell_session
npm install bootstrap
```

```typescript
// in style.css
@import "~bootstrap/dist/css/bootstrap.css";
```

## Build project:
```shell_session
npm run build
```

## Add Angular Materials:
```shell_session
ng add @angular/material
```

```typescript
// Add to the app.module.ts file
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

//Angular Material Components
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';
import {MatCheckboxModule} from '@angular/material/checkbox';
import {MatButtonModule} from '@angular/material/button';
import {MatInputModule} from '@angular/material/input';
import {MatAutocompleteModule} from '@angular/material/autocomplete';
import {MatDatepickerModule} from '@angular/material/datepicker';
import {MatFormFieldModule} from '@angular/material/form-field';
import {MatRadioModule} from '@angular/material/radio';
import {MatSelectModule} from '@angular/material/select';
import {MatSliderModule} from '@angular/material/slider';
import {MatSlideToggleModule} from '@angular/material/slide-toggle';
import {MatMenuModule} from '@angular/material/menu';
import {MatSidenavModule} from '@angular/material/sidenav';
import {MatToolbarModule} from '@angular/material/toolbar';
import {MatListModule} from '@angular/material/list';
import {MatGridListModule} from '@angular/material/grid-list';
import {MatCardModule} from '@angular/material/card';
import {MatStepperModule} from '@angular/material/stepper';
import {MatTabsModule} from '@angular/material/tabs';
import {MatExpansionModule} from '@angular/material/expansion';
import {MatButtonToggleModule} from '@angular/material/button-toggle';
import {MatChipsModule} from '@angular/material/chips';
import {MatIconModule} from '@angular/material/icon';
import {MatProgressSpinnerModule} from '@angular/material/progress-spinner';
import {MatProgressBarModule} from '@angular/material/progress-bar';
import {MatDialogModule} from '@angular/material/dialog';
import {MatTooltipModule} from '@angular/material/tooltip';
import {MatSnackBarModule} from '@angular/material/snack-bar';
import {MatTableModule} from '@angular/material/table';
import {MatSortModule} from '@angular/material/sort';
import {MatPaginatorModule} from '@angular/material/paginator';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    MatCheckboxModule,
    MatButtonModule,
    MatInputModule,
    MatAutocompleteModule,
    MatDatepickerModule,
    MatFormFieldModule,
    MatRadioModule,
    MatSelectModule,
    MatSliderModule,
    MatSlideToggleModule,
    MatMenuModule,
    MatSidenavModule,
    MatToolbarModule,
    MatListModule,
    MatGridListModule,
    MatCardModule,
    MatStepperModule,
    MatTabsModule,
    MatExpansionModule,
    MatButtonToggleModule,
    MatChipsModule,
    MatIconModule,
    MatProgressSpinnerModule,
    MatProgressBarModule,
    MatDialogModule,
    MatTooltipModule,
    MatSnackBarModule,
    MatTableModule,
    MatSortModule,
    MatPaginatorModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Save Angular Materials icons locally so internet isn't necessary
```shell_session
npm install material-design-icons -save
```
```typescript
// Add to your style.css file
@import '~material-design-icons/iconfont/material-icons.css';
```

## Add a new component:
```shell_session
ng g c <componentName>
```

## Add a new module:
```shell_session
ng g m <name>
```

## Add a new service (without spec.ts file)
```shell_session
ng generate s security-watch --skip-tests
```
---
</br>

</br>

</br>

# Angular Materials Styling Examples:
```css


.mat-button-toggle {
    background-color: lightgray;
    color: rgb(43, 43, 43);
    font-weight: normal;
}

.mat-button-toggle-checked {
    background-color: #5882b9;
    color: #fff;
    font-weight: normal;
}

::ng-deep .mat-focused .mat-form-field-label {
    /*change color of label*/
    color: #343a40 !important;
   }

::ng-deep .mat-form-field-label {
  /*change color of label*/
  color: #343a40 !important;
  }

::ng-deep.mat-form-field-underline {
    /*change color of underline*/
    background-color: #343a40 !important;
  }

::ng-deep.mat-form-field-ripple {
   /*change color of underline when focused*/
   background-color: #343a40 !important;
  }

::ng-deep.mat-input-element.mat-form-field-autofill-control {
  background-color: transparent !important;
}

::ng-deep.mat-select-arrow{
  color: #343a40 !important;
}

::ng-deep.mat-form-field-underline:before{
    background-color: #343a40 !important;
  }

.mat-option {
  color: #545454 !important;
  background: lightgray !important;
}

.mat-option.mat-active {
  background: #343a40 !important;
  color: #FFF !important;
}

:host /deep/ .mat-select-value-text {
  color: #343a40 !important;
}
```

---
</br>

</br>

</br>

# Angular Guides/Walkthroughs:

## Send update a variable's value in parent component from the child component:

### In child component .ts file:
* Import the Output and EventEmitter interfaces from @angular/core
* Add this line:

```typescript
@Output() <eventName> = new EventEmitter<eventValueType>();
// eventValueType would be string, number, class, boolean, etc.
```
* Add a function to emit the event:
```typescript
newRequestEventFunction(value: <eventValueType>) {
  this.<eventName>.emit(value);
}
```
### In child component .html file:
For text inputs:
```html
<input type="text" #templateReferenceVariable>
<button (click)="newRequestEventFunction(templateReferenceVariable.value)"></button>
```
In my original case I wanted to toggle a parent variable from true to false, so I just did a click event like:
```html
<button (click)="newRequestEventFunction(false)"></button>
```
### In parent component .ts file:
* Create a variable you're going to be updating:
```typescript
variableToUpdate = true;
```
* Create a function that updates that variable:
```typescript
updateVariableToUpdate(value: eventValueType) {
  this.variableToUpdate = value;
}
```
### In parent component .html file:

```html
<app-childComponentTag (eventName)="updateVariableToUpdate($event)"></app-childComponentTag>
```

### <a src="https://www.youtube.com/watch?v=qspoPXaF_Aw&t=575s" target="_blank">Useful Youtube Video On This Subject</a>

</br>

</br>

</br>

# Angualar Debugging:
### <u>I added a service to a constructor, and now my page doesn't render:</u>
Try adding HttpClientModule to app.module.ts (import at the top, and include in imports inside @NgModule()).