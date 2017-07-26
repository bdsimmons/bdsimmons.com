---
title: 'Angular Todo: Components'
layout: post
date: '2017-07-25 18:14:55 -0500'
image: "/assets/img/"
description: Angular 4 todo application built with CLI
tags:
- angular
- anguar 4
- web
- todo
- tutorial
categories:
- Angular
twitter_text: 'Angular Todo: Components - Angular 4 todo application built with CLI'
---

## Overview and Purpose

In this checkpoint we will build a basic todo application with Angular.

## Objectives

* Create a Todo Application with Angular
* Evaluate ________
* Analyze _________
* Understand _________

## Setting up the Project

Just like our simple counter application, we will use the [Angular CLI](https://cli.angular.io/) to setup the scaffolding for our todo list app. Head over to your terminal and create the project:

```bash
$ ng new angular-todo
$ cd angular-todo
$ npm install
$ ng serve -o
```

If everything went right, your new app should have compiled and opened a new browser window showing the default angular root component. The `-o` flag is what allowed your browser window to open automatically. Pretty neat, eh?

## Including Styles

Since we are using Angular, a Google project, I think it is only fitting that we use Google's [Material Design framework](https://material.io/) to give our application some style. There are several options in that arena, but we will take a simple approach and just include the css framework [Materialize CSS](http://materializecss.com/) through a CDN. We can do this quite simply by adding a few lines to our `index.html`.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>AngularTodo</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
+ <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.100.1/css/materialize.min.css">
</head>
<body>
  <app-root></app-root>
+ <script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>
+ <script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.100.1/js/materialize.min.js"></script>
</body>
</html>
```

We have to include jQuery because it is a dependency of the Materialize CSS framework.

## Styling our List

Open up your newly created project and head into the `src` directory. We need to clean up some of the boilerplate code and setup a clean slate for our todo app. Navigate to `src/app/app.component.html` and replace the contents of that file with the following markup.

```html
<div class="container">
  <h2 class="center-align">Bloc List</h2>
  <div class="row">
    <div class="col s4 offset-s4">
      <p>
        <input type="checkbox" class="filled-in" id="filled-in-box" />
        <label for="filled-in-box">My Todo</label>
      </p>
    </div>
  </div>
</div>
```

The weird looking classes like `col s4 offset-s4` are from Materialize CSS and just help us organize our page into something nice and neat. Don't worry too much about it, but if you are interested, you can check out the documentation of the [Materialize CSS](http://materializecss.com/) website.

Now if you head back to your browser, you should see a clean looking list with one item in it. It doesn't really do anything yet, but we will get to that shortly.

## Components

As we have seen, Angular is primarily a hierarchy of web components that we define. At the root of the application, we have the `app-root` component that looks like `<app-root></app-root>` in the `index.html`. This tells Angular to go look up the definition of the component named `app-root` and to render and run any markup, styles and javascript that is associated with this component. In this situation, the javascript logic is defined in `src/app/app.component.ts`, the markup in `src/app/app.component.html`, and the styles in `src/app/app.component.css`. See a pattern?

The Angular CLI follows the above pattern with every component that we ask it to make, and we are about to make another one and include it within the `app-root` component. This parent/child relationship is how Angular components work, with `app-root` being the original parent.

## The Todo Component

Currently we have only one component in our application, but seperating out our application logic into different components helps us keep our sanity allows for very great reusability. Once we define the component, any other component may use it in its html markup.

Let's go ahead and take the todo part of our `app-root` component and make it a component itself. We can do this with the Angular CLI `generate` command.

Stop your development server if it's running with `ctrl + c` and run the following:

```bash
$ ng generate component todo
```

You should see an output that is something like:

```bash
installing component
  create src/app/todo/todo.component.css
  create src/app/todo/todo.component.html
  create src/app/todo/todo.component.spec.ts
  create src/app/todo/todo.component.ts
  update src/app/app.module.ts
```

Lets take a look at what the CLI did for us. First, it created a new directory `todo` inside the `app` directory and then created four files inside it. The four files should look familiar as they are the same files that made up our `app-root` component: `css`, `html`, `spec.ts`, and `ts`. These are the files that make up a component, but there is also one more step that has to take place. We have to tell our Angular app about our new component. This is done in the `src/app/app.module.ts` file, and the CLI already handled it for us. Sweeet! Those Angular devs sure are cool cats.

Take a look at your `app.module.ts` and you will notice the new line under `declarations`.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { TodoComponent } from './todo/todo.component';

@NgModule({
  declarations: [
    AppComponent,
+   TodoComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

OK, so we have this component and all but how do we use it?

Easy! We just include it in another component, in this case the `app-root` component.

Head over to `src/app/app.component.ts` and lets switch out the todo html for our new component tag.

```html
<div class="container">
  <h2 class="center-align">Bloc List</h2>
  <div class="row">
    <div class="col s4 offset-s4">
+     <app-todo></app-todo>
-     <p>
-       <input type="checkbox" class="filled-in" id="filled-in-box" />
-       <label for="filled-in-box">My Todo</label>
-     </p>
    </div>
  </div>
</div>
```

Now lets take the markup for a todo and add it to `src/app/todo/todo.component.html`

```html
+<p>
+  <input type="checkbox" class="filled-in" id="filled-in-box" />
+  <label for="filled-in-box">My Todo</label>
+</p>
```

If we have done everything correctly, we should be able to start our dev server with `ng serve` and see that nothing has changed. However, now if we add a second `app-todo` component in our `app-root` component, we should see two todos.

## The Todo List Component

Our app structure at this point doesn't make that much semantic sense. We have an `app-todo` component and this lives just inside the root component of our application `app-root`. Really, this should be organized just like a physical todo list is in the real world. We should create a todo-list component that contains many todo components. This way, we could have many todo lists if we wanted simply by including our new todo-list compinent. Well, what are you waiting for? Head on over to your terminal and let's get crackin!

```bash
$ ng generate component todo-list
```

This will create another directory located at `src/app/todo-list` containing our new component files. This component will be easy to implement in terms of markup. Soon, we will pass data to this component and dynamically set the number of todos, but for now let's just set up a few todos in here to see what it will look like. 

```html
<app-todo></app-todo>
<app-todo></app-todo>
<app-todo></app-todo> 
```

Next we have to include our new `app-todo-list` component in the `app-root` component.

```html
<div class="container">
  <h2 class="center-align">Bloc List</h2>
  <div class="row">
    <div class="col s4 offset-s4">
-     <app-todo></app-todo>
+     <app-todo-list></app-todo-list>
    </div>
  </div>
</div>
```

We should now have three todos rendered to the screen with a component hierarchy like this: `app-root` -> `app-todo-list` -> `app-todo` where any component may contain multiple copies of its children if we so desire. (i.e. `app-root` may contain more than one `app-todo-list`, etc).

## Services: Sharing Data between components

So far, we have no real todo data in this application. We have scaffolded out a nice component architecture, but nothing really does anything yet. This just won't do. 

In Angular, Services are how we insert data into our components. A service is simply a typescript class that is specially allowed by Angular to be injected into components. This is accomplished through the `@Injectable` decorator much like the way components become components through using the `@component` decorator. However, the Angular team has of course provided us a way to make this easier on us through using the CLI.

```bash
$ ng generate service todo
```

The above command creates two new files for us, a spec and a service file located right in `src/app`. However, if we look at the output from this terminal command there is one very important piece of information:

```bash
installing service
  create src/app/todo.service.spec.ts
  create src/app/todo.service.ts
  WARNING Service is generated but not provided, it must be provided to be used
```

The bit that is important is `WARNING Service is generated but not provided, it must be provided to be used`. This means that, although the CLI provided us a nice boilerplate file for our service, it did not inject it for us. We have to do this manually, and the first step here is to head over to our module file at `src/app/app.module.ts`. We need to add an import statement that imports `TodoService` to the file and then we need to include it in our list of `providers` for our module.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { TodoComponent } from './todo/todo.component';
import { TodoListComponent } from './todo-list/todo-list.component';

+import { TodoService } from './todo.service';

@NgModule({
  declarations: [
    AppComponent,
    TodoComponent,
    TodoListComponent
  ],
  imports: [
    BrowserModule
  ],
+ providers: [TodoService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Now let's take a look at the service file itself. You will notice that this is a pretty simple file to digest. It is a simple typescript class with that one caveat of the `@Injectable` component that we have already mentioned. 

Let's go ahead and add in some data.

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class TodoService {
+ todos: object[]; // add a todos instance variable that is an array of objects

  constructor() {
    // Populate our todos instance variable with actual todo objects
+   this.todos = [
+     {
+       id: 1,
+       text: 'Learn Angular',
+       completed: false
+     },
+     {
+       id: 2,
+       text: 'Be Awesome',
+       completed: true
+     },
+     {
+       id: 3,
+       text: 'Write a world changing app',
+       completed: false
+     }
+   ];
  }
}
```

Typically, services would communicate with an external API or database to retrieve data, but here we are going to just hard code some data so that we can learn only about how we use services in Angular.

Since our service by definition is injectable, it stands to reason that in order to use it in a component, we will need to inject it into that component. The component that should receive a list of todos is, you guessed it, `todo-list`. You are super smart!



```typescript
import { Component, OnInit } from '@angular/core';

// 1. We need to import our TodoService so the TypeScript compiler knows about it
+import { TodoService } from '../todo.service';

@Component({
  selector: 'app-todo-list',
  templateUrl: './todo-list.component.html',
  styleUrls: ['./todo-list.component.css']
})
export class TodoListComponent implements OnInit {
//2. We add a todos property to store our todo Objects
+ private todos: Object[];

  constructor(
    //3. This is shorthand. It adds a todoService property to the class
    // and instantiates it to what is passed into the constructor
+   private todoService: TodoService
  ) { }

  ngOnInit() {
    //4. We populate the todo property with the todos from our service.
+   this.todos = this.todoService.todos;
  }

}
```

Now that we have our service setup and passing info to the TodoListComponent, we could do the same in any other component that needs this data. Services allow us to store data and encapsulate it away from any one component, so that it can be injected into any component that we desire, just like we did at step `#3` above.

However, we have still not used this data, or even confirmed that it is in fact present. We still have our dummy todo components rendered in the todo-list. Next we will check for the presence of our data and then display them as todos.

## Checking Our Assumptions

First things first. We need to check our assumptions and confirm that there is in fact an array of objects stored in the `todos` property of the `TodoListComponent`. Checking your assumptions is a lesson you have to learn sooner rather than later as a programmer, but only if you want to preserve your sanity.

Change the markup for our `todo-list` component to just try to output the `todos` property we just setup.

```html
{% raw %}
+<pre>{{ todos }}</pre> // We use pre tags to keep any formatting the text already has
{% endraw %}
-<app-todo></app-todo>
-<app-todo></app-todo>
-<app-todo></app-todo> 
```

Now in your browser you will see a strange output that looks something like this: `[object Object],[object Object],[object Object]`. What in tarnation is that!?

Well, when we try to print out our objects to the screen, what actually happens in the background is that the `toString` method 
is called on our objects. For objects, this returns the string `[object Object]`. Go ahead, try it in your developer tools console if you don't believe me. I'll wait.

Ok, so how do we get past this? We have objects, but can't see anything in them. We need to somehow tell angular to take the keys and values of our objects and display them on the screen. Thankfully, this presents us with the perfect opportunity to learn about another Angular tool called [`Pipes`](https://angular.io/guide/pipes).

## Pipes

You may already be familiar with the Linux computing term `pipe` or with AngularJS(1.x) `filters`. This is exactly the same concept. We call them pipes, because it can be said that we 'pipe' or transfer one thing or value from one item to another. This can be visualized as such:

Given `Function A` and `Function B` as `FA` and `FB`, `FA` returns value `x` and `x` becomes the input for `FB`. `FB` then takes the value `x` and transforms it to `y`. Therefore it can be said that, `FA|FB = y`.

Basically, we send (or pipe) the output of the first function and it becomes the input for a second function, which returns a transformed value. 

Boiled down to one sentence, this all means: We use a pipe when we want to transform the value we currently have into a new form.

All Right, we are almost there, I promise. Let's modify our `todo-list` component html file once more but this time use the  built in [`json` pipe](https://angular.io/api/common/JsonPipe) that comes with Angular.

```html
{% raw %}
+<pre>{{ todos|json }}</pre>
-<pre>{{ todos }}</pre>
{% endraw %}
```

Now we should see some nicely formatted JSON in our browser window. It should look like this:

```json
[
  {
    "id": 1,
    "text": "Learn Angular",
    "completed": false
  },
  {
    "id": 2,
    "text": "Be Awesome",
    "completed": true
  },
  {
    "id": 3,
    "text": "Write a world changing app",
    "completed": false
  }
]
```

Yay! We checked our assumptions and it turns out that we do in fact have data available to us in our html template. Now, how do we send that data, one at a time, to our todo components? Patience young padawan, patience.

## Displaying Dynamic Todos



## Directives (Add & Delete)
