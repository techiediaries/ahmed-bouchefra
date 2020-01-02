---
layout: post
title:  "Modal Popup Example and Tutorial with Angular 9 Material"
date:   2020-1-02
---

In this tutorial, we'll build by example a modal popup using Angular 9 Material.

Angular Material provides modern UI components for building user interfaces based on the material design specification that works across the web, mobile, and desktop.


## Step 1: Creating an Angular 9 Project

Open a new command-line interface and run the following command: 

```bash
$ ng new angular-modal-example
```


## Step 2: Installing and Setting up Angular Material

Navigate inside the project's folder and begin by installing `hammerjs` as follows:

```bash
$ cd angular-modal-example
$ npm install --save hammerjs
```

Hammer.js adds support for touch support.

Next, install Angular Material and Angular Animations using the following commands:

```bash
$ npm install --save @angular/material @angular/animations @angular/cdk
```

Now, include `hammerjs` in the `angular.json` file. 

## Step 3: Creating the Custom Material Module File

Navigate to the  `src/app` folder, create one module file named `material.module.ts`:

```bash
$ cd src/app
$ touch material.module.ts
```


Open the `src/app/material.module.ts` file and update it as follows:

```ts
import { NgModule } from '@angular/core';

import { MatDialogModule, MatFormFieldModule, MatButtonModule, MatInputModule } from '@angular/material';
import { FormsModule } from '@angular/forms';

@NgModule({
  exports: [FormsModule, MatDialogModule, MatFormFieldModule, MatButtonModule, MatInputModule]
})
export class MaterialModule {}
```


## Step 4: Importing a Theme and Material Icons

Angular Material  provides many pre-built themes. Open the `styles.css` file and add:

```css
@import '~@angular/material/prebuilt-themes/indigo-pink.css';
```

Next open the `index.html` file and add:

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
```

Next, open the `src/app/app.module.ts` file and import the
 `material.module.ts` and `BrowserAnimationsModule`:

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { MaterialModule } from './material.module';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, BrowserAnimationsModule, MaterialModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}

```

## Step 5: Implementing the Angular Material Modal Dialog

Now, open the `src/app.component.ts` file, and import MatDialog, MatDialogRef, MAT_DIALOG_DATA:

```ts
import { Component, Inject } from '@angular/core';
import {MatDialog, MatDialogRef, MAT_DIALOG_DATA} from '@angular/material';

interface DialogData {
  email: string;
}

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
}
```



Next, create an Angular component by running the following command from the root of your project:

```bqsh
ng generate component modal --module app --spec=false
```


Openn the `src/app/modal/modal.component.ts` file and update it as follows:

```ts
import { Component, OnInit, Inject } from '@angular/core';
import { MatDialogRef, MAT_DIALOG_DATA} from '@angular/material';

interface DialogData {
  email: string;
}

@Component({
  selector: 'app-modal',
  templateUrl: './modal.component.html',
  styleUrls: ['./modal.component.css']
})
export class ModalComponent implements OnInit {

  constructor(
    public dialogRef: MatDialogRef<ModalComponent>,
    @Inject(MAT_DIALOG_DATA) public data: DialogData) {}

  onNoClick(): void {
    this.dialogRef.close();
  }

  ngOnInit() {
  }
}
```

Open the `src/app/modal/modal.component.html` file and add the following markup:

```
<h1 mat-dialog-title>Want to receive our emails?</h1>
<div mat-dialog-content>
  <p>What's your email?</p>
  <mat-form-field>
    <input matInput [(ngModel)]="data.email">
  </mat-form-field>
</div>
<div mat-dialog-actions>
  <button mat-button (click)="onNoClick()">No</button>
  <button mat-button [mat-dialog-close]="data.email" cdkFocusInitial>Yes</button>
</div>
```


Next, open the `src/app/app.component.ts` file and update it as follows:

```ts
import { Component, Inject } from '@angular/core';
import { MatDialog, MatDialogRef, MAT_DIALOG_DATA } from '@angular/material';
import { ModalComponent } from './modal/modal.component';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  email: string;

  constructor(public dialog: MatDialog) {}

  openDialog(): void {
    const dialogRef = this.dialog.open(ModalComponent, {
      width: '300px',
      data: {}
    });

    dialogRef.afterClosed().subscribe(result => {
      this.email = result;
    });
  }
}

```

First, we  importe the modal component in the `src/app/app.component.ts` file, next we defined the `openDialog()` method which opens the `ModalComponent`

When the user closes the modal component, the `App` component receives the value of the email entered in `ModalComponent`.

Next, open the `src/app/app.component.html` file and the following markup:

```
<div>

      <button mat-raised-button (click)="openDialog()">Open modal</button>

    <br />
    <div *ngIf="email">
      You signed up with: <p>{{email}}</p>
    </div>
</div>

```


Open the src/app/app.module.ts file and add the modal component inside the  `entryComponents` array of the module as follows:

```ts

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { MaterialModule } from './material.module';

import { AppComponent } from './app.component';
import { ModalComponent } from './modal/modal.component';

@NgModule({
  declarations: [AppComponent, ModalComponent],
  imports: [BrowserModule, BrowserAnimationsModule, MaterialModule],
  providers: [],
  bootstrap: [AppComponent],
  entryComponents: [ModalComponent]
})
export class AppModule {}

```

That' it, let's now test our modal dialog by serving our Angular appliaction from the terminal:

```bash
$ ng serve 
```

The server will be running from the `http://localhost:4200`.

## Conclusion

In this quick example, we have created a popup modal dialog with Angular Material and Angular 9.

