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

```html(~/bloc/angular-todo/src/index.html)
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

```html(~/bloc/angular-todo/src/app/app.component.html)
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

Currently we have only one component in our application, but seperating our our application logic into different components helps us keep our sanity allows for very great reusability. Once we define the component, any other component may use it in its html markup.

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

```typescript(~/bloc/angular-todo/src/app/app.module.ts)
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

```html(~/bloc/angular-todo/src/app/app.component.html)
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

```html(~/bloc/angular-todo/src/app/todo/todo.component.html)
+<p>
+  <input type="checkbox" class="filled-in" id="filled-in-box" />
+  <label for="filled-in-box">My Todo</label>
+</p>
```

If we have done everything correctly, we should be able to start our dev server with `ng serve` and see that nothing has changed. However, now if we add a second `app-todo` component in our `app-root` component, we should see two todos.



