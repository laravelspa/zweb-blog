---
title: "كيفية تحميل Vue 3 داخل مشروع Laravel 10 بإستخدام Vite"
date: 2023-05-08
draft: false
slug: "how-to-install-vue3-in-laravel10-with-vite-in-arabic"
description: "كيفية تحميل Vue 3 داخل مشروع Laravel 10 بإستخدام Vite"
cascade:
  showReadingTime: true
categories: ['Laravel', Vuejs]
tags: ['laravel10', 'vue3', 'full stack', 'spa']
---
## عرض المشروع
{{< figure
src="/img/how-to-install-vue3-in-laravel10-with-vite/laravel10_vue3.png"
alt="Laravel 10 Vue 3"
caption="Laravel 10 Vue 3"
>}}

{{< youtube id="dvI5Qu-XXk8" title="How To Install Vue 3 in Laravel 10" >}}
## [ما هو إطار عمل Laravel](https://laravel.com/docs/10.x#meet-laravel)
- Laravel هو إطار عمل لتطبيق ويب ذو بناء مرتب وأنيق.
- يوفر إطار عمل الويب هيكلًا ونقطة بداية لإنشاء تطبيقك ، مما يسمح لك بالتركيز على إنشاء شيء مذهل بينما تهتم بالتفاصيل.


## [ما هى Vue js!](https://vuejs.org/guide/introduction.html)
- Vue هو إطار عمل JavaScript لبناء واجهات المستخدم.
- يعتمد على HTML و CSS و JavaScript.
- يساعدك على تطوير واجهات المستخدم بكفاءة ، سواء كانت بسيطة أو معقدة.


## [ما هى Vite js!](https://vitejs.dev/guide/why.html)
Vite هي أداة بناء تهدف إلى توفير تجربة تطوير أسرع وأكثر رشاقة لمشاريع الويب الحديثة. وهذه المشاريع تتكون من جزئين رئيسيين:
- خادم مطور يوفر تحسينات غنية بالميزات على وحدات ES الأصلية ، على سبيل المثال استبدال الوحدة النمطية السريعة للغاية (HMR).

- أمر بناء يجمع التعليمات البرمجية الخاصة بك مع Rollup ، مهيأ مسبقًا لإنتاج أصول ثابتة محسّنة للغاية للإنتاج.

{{< alert >}}
ونستنتج من السابق أن أهم شئ بيميز vite هى السرعة فى بداية السيرفر وأيضا عمل bundle لملفات المشروع عند الانتهاء من المشروع.
{{< /alert >}}


## الخطوة الأولى: إنشاء مشروع Laravel جديد
```bash
composer create-project laravel/laravel laravel10-vue3
```

## الخطوة الثانية: كيفية تحميل Vue 3 فى مشروع Laravel 10
```bash
cd laravel10-vue3
npm install
npm install vue@next vue-loader@next
```
## الخطوة الثالثة: تحميل Plugin Vue من Vite
```bash
npm i @vitejs/plugin-vue
```

## الخطوة الرابعة: قم بتعديل ملف ***`vite.config.js`***
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

## الخطوة الخامسة: قم بتعديل ملف ***`app.js`*** داخل مجلد ***`resources/js`***
```js
import {createApp} from 'vue'

import App from './App.vue'

createApp(App).mount("#app")
```

## الخطوة السادسة: قم بإنشاء ملف ***`app.blade.php`*** داخل مجلد ***`resources/views`***
{{< alert >}}
تأكد من إضافة ملف css و javascript كما هو موضح وأيضا div ب id=app
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

## الخطوة السابعة: قم بإنشاء ملف ***`App.vue`*** داخل مجلد ***`resources/js`***
```HTML
<template>
    <h1>
        How To Install Vue 3 in Laravel 10 : Laravel SPA :)
    </h1>
</template>
```

## الخطوة الثامنة: قم بتعديل ملف ***`web.php`*** داخل مجلد ***`routes`***
```PHP
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('app');
})
->name('application');
```

## الخطوة التاسعة: قم بتشغيل السيرفر المحلى
```bash
php artisan serve
```

## الخطوة العاشرة: فم بتشغيل السيرفر المحلى Node 
```bash
npm run dev
```
{{< alert >}}
قم بالذهاب الى ***`http://127.0.0.1:8000/`*** ستجد ما يلى
{{< /alert >}}

{{< figure
src="/img/how-to-install-vue3-in-laravel10-with-vite/laravel10_vue3.png"
alt="Laravel 10 Vue 3"
caption="Laravel 10 Vue 3"
>}}


- ستجد repo الخاصة بهذا المشروع على github هنا
{{< github repo="laravelspa/laravel10-vue3" >}}
