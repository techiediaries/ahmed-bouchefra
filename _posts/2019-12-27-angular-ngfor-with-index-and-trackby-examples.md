---
layout: post
title:  "Angular 9/8/7 ngFor with Index and trackBy Examples"
date:   2019-12-27
---

In this post, we'll see by examples how to use the `ngFor` directive to iterate over arrays of data and even objects in Angular templates.

## What is ngFor in Angular Template Syntax?

Angular makes use of HTML for templates associated with components which eventually represent the views of your front-end application. 

A component controls a part of your application user interface and the associated template is what gets rendered in that part of the UI.

Since HTML doesn't have a built-in template language, Angular extends HTML with a powerful template syntax that includes many directives such as `ngFor` which is similar to the typical for-loops in programming languages.

The `ngFor` allows you to loop through an array of data directly in the HTML template. The array must be present in the associated component.

## How to Use ngFor in Angular Templates?

Let's now see how to use the ngFor directive by example.

Let's suppose. we have a movies component and we want to display the movies from an array in the template. Here is how:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-movies-list',
  template: `
    <ul>
      <li *ngFor="let movie of moviesArr">
        {{ movie.title }}
      </li>
    </ul>
    `
})

export class MoviesListComponent  {
    moviesArr: any[] = [
    {
      "title": "Super Man"
    },
    {
      "title": "Spider Man"
    },
    {
      "title": "Aladdin"
    }, 
    {
      "title": "Downton Abbey"
    }
  ];
}
```

The `moviesArr` array is automatically available in the component's template which is in our case an inline template.

We use the ngFor directive to iterate over the array of movies and display the title of each movie.

The directive has the `*ngFor="let movie of moviesArr"` syntax. After the `let` keyword, we add a variable, which can be any valid variable name, that will be used to reference and access each element of the array and after the `of` keyword, we add the array of data which must be present in the component.

We also need to prefix `ngFor` with a `*` before providing the iteration expression. 

## The Index of ngFor Elements

Suppose that we need to display the index of each element of the movies array. 

Angular provides the reserved `index` keyword inside the `ngFor` expression which can be used as follows:



```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-movies-list',
  template: `
    <ul>
      <li *ngFor="let movie of moviesArr">
        {{ movie.title }}
      </li>
    </ul>
    `
})

export class MoviesListComponent  { /* ... */ }
```


