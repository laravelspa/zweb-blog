---
title: "How To Install Vue 3 In Laravel 10 With Vite"
date: 2023-05-01
draft: false
slug: "how-to-install-vue3-in-laravel10-with-vite"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Vuejs']
tags: ['laravel10', 'vue3', 'full stack', 'spa']
---

## Preview
{{< figure
src="/img/how-to-install-vue3-in-laravel10-with-vite/laravel10_vue3.png"
alt="Laravel 10 Vue 3"
caption="Laravel 10 Vue 3"
>}}
## [What is Laravel](https://laravel.com/docs/10.x#meet-laravel)
- Laravel is a web application framework with expressive, elegant syntax.
- A web framework provides a structure and starting point for creating your application, allowing you to focus on creating something amazing while we sweat the details.


## [What is Vue js!](https://vuejs.org/guide/introduction.html)
- Vue is a JavaScript framework for building user interfaces.
- It builds on top of standard HTML, CSS, and JavaScript.
- It Helps you efficiently develop user interfaces, be they simple or complex.


## [What is Vite js!](https://vitejs.dev/guide/why.html)
Vite is a build tool that aims to provide a faster and leaner development experience for modern web projects. It consists of two major parts:
- A dev server that provides rich feature enhancements over native ES modules, for example extremely fast Hot Module Replacement (HMR).

- A build command that bundles your code with Rollup, pre-configured to output highly optimized static assets for production.
{{< alert >}}
We conclude from the above that the most important thing that distinguishes vite is the speed at the start of the server, as well as making a bundle of files upon completion of the project.
{{< /alert >}}


## Step 1: Create New Laravel Project
```bash
composer create-project laravel/laravel laravel10-vue3
```

## Step 2: How To Install Vue 3 on Laravel 10
```bash
npm install
npm install vue@next vue-loader@next
```
## Step 3: Install Plugin Vue From Vite
```bash
npm i @vitejs/plugin-vue
```

## Step 4: Edit File ***`vite.config.js`***
```js {hl_lines=["4", "8"]}
// vite.config.js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue'

export default defineConfig({
    plugins: [
        vue(),
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
    ],
});
```

## Step 5: Edit File ***`app.js`*** Inside Folder ***`resources/js`***
```js
import {createApp} from 'vue'

import App from './App.vue'

createApp(App).mount("#app")
```

## Step 6: Create File ***`app.blade.php`*** Inside Folder ***`resources/views`***
{{< alert >}}
Make sure to add the css file and javascript as shown and also the div with id=app
{{< /alert >}}
```HTML {hl_lines=["8", "11-12"]}
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>ًApplication</title>
        @vite('resources/css/app.css')
    </head>
    <body>
        <div id="app"></div>
        @vite('resources/js/app.js')
    </body>
</html>
```

## Step 7: Create File ***`App.vue`*** Inside Folder ***`resources/js`***
```HTML
<template>
    <h1>
        How To Install Vue 3 in Laravel 10 : ZWebCourses :)
    </h1>
</template>
```


## Step 8: Edit File ***`web.php`*** Inside Folder ***`routes`***
```PHP
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('app');
})
->name('application');
```


## Step 9: Run PHP Local Server
```bash
php artisan serve
```

## Step 10: Run Node Local Server 
```bash
npm run dev
```
{{< alert >}}
Go to the following link ***`http://127.0.0.1:8000/`*** and you will find the following
{{< /alert >}}

{{< figure
src="/img/how-to-install-vue3-in-laravel10-with-vite/laravel10_vue3.png"
alt="Laravel 10 Vue 3"
caption="Laravel 10 Vue 3"
>}}

- You will find the repo for this project on github at this [Link](https://github.com/zwebcourses/laravel10-vue3)
https://github.com/zwebcourses/laravel10-vue3