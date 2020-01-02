---
layout: post
title:  "Angular 9 Example: Import HttpClientModule and Send Ajax Requests to JSON REST API Servers"
date:   2020-1-02
---

[`HttpClientModule`](https://angular.io/api/common/http/HttpClientModule) 
configures the  [dependency injector](https://angular.io/guide/glossary#injector)  for  [`HttpClient`](https://angular.io/api/common/http/HttpClient) with supporting services for XSRF.

In this example, we'll see how to import `HttpclientModule` in Angular and use `HttpClient` to send an Ajax GET request to JSON REST API servers.


## What is HttpClient and how it Relates to Ajax?

`HttpClient` is a service for handling HTTP requests which is built on top of `XMLHttpRequest` which the legacy API for doing Ajax. 

### What About Ajax?

Ajax stands for Asynchronous JavaScript and XML and is a technique for requesting data from the server without doing a full page refresh, and using the XML result to re-render the related part of the page.

In the modern web, Ajax refers to any asynchronous request sent to a server from client-side JavaScript. Typically the response is JSON, or HTML fragments instead of XML.

### How to Use `HttpClient`?

It can be injected in other services and components. It exports methods for making HTTP Ajax requests such as `get()`, `post()`, `put()`, and `delete()` and return http responses with various types such as json, text and blob. 

[`HttpClient`](https://www.ahmedbouchefra.com/blog/angular-tutorial-example-upload-files-with-formdata-httpclient-rxjs-and-material-progressbar/) provides many benefits such as testability, typed request and response objects, request and response interception, Observable APIs, and streamlined error handling.

The methods of `HttpClient` are based on RxJS observables, so we need to be aware of these points when using them:

1.  You need to subscribe to the observable returned from the method to actually send the http request to the server,
2.  If you subscribe multiple times to the returned observable, multiple HTTP requests will be sent to the server,
3.  The returned observable is a single-value stream which means it will emit only one value and complete.

## Step 1 - Creating an Angular 9 Project

If ou are new to these quick how-to posts, you need to [install Angular CLI and scaffold a new project](https://www.ahmedbouchefra.com/blog/install-angular-cli-and-create-project-with-routing/)
 
## Step 2 - Importing `HttpClientModule`?

Before you can use `HttpClient` in your Angular 9 application, you need to import `HttpClientModule`.

Open the `src/app/app.module.ts` file and import `HttpClientModule` from `@angular/common/http`:

```ts
import { NgModule }         from '@angular/core';  
import { BrowserModule }    from '@angular/platform-browser';  
import { HttpClientModule } from '@angular/common/http';
```

Next, you need to add it `HttpClientModule` as follows:

```ts
@NgModule({  
imports: [  
BrowserModule,  
// import HttpClientModule after BrowserModule.  
HttpClientModule,  
],  
declarations: [  
AppComponent,  
],  
bootstrap: [ AppComponent ]  
})  
export class AppModule {} 
```

## Step 3 - Using Angular `HttpClient` to Send Ajax GET Requests 

After you imported `HttpClientModule`, you can send http requests using the `HttpClient` service which you can inject in any service or component.

Open the `src/app/app.component.ts` file and start by importing `HttpClient` as follows:

```ts
import { HttpClient } from '@angular/common/http';
```

Next, inject  `HttpClient`  via the constructor of the component as follows:

```ts
  constructor(public httpClient: HttpClient){}
```

Next, define the following example method:

```ts
    sendGetRequest(){
        this.httpClient.get('https://jsonplaceholder.typicode.com/users').subscribe((res)=>{
            console.log(res);
        });
    }

```

This method sends a GET request to the server endpoint and subscribe to the returned observable.

Next, open the `src/app/app.component.ts` file and add a button to call the  `sendGetRequest()`  method as follows:

```html
    <button (click)="sendGetRequest()">GET Users</button>
```

## References

- [https://angular.io/api/common/http/HttpClientModule](https://angular.io/api/common/http/HttpClientModule),
- [https://www.techiediaries.com/angular-by-example-httpclient-get](https://www.techiediaries.com/angular-by-example-httpclient-get/)
- [https://angular.io/api/common/http/HttpClient](https://angular.io/api/common/http/HttpClient)

## Conclusion

In this quick how-to post, we have seen how to import `HttpClientModule` in Angular and used the `HttpClient` service to send an example Ajax GET request to a REST API server server. 

