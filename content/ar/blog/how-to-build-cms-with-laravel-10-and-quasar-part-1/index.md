---
title: "كيفية بناء نظام إدارة محتوى بإستخدام Laravel 10 & Quasar Frameweork (1)"
date: 2023-05-01
draft: true
slug: "how-to-build-cms-with-laravel-10-and-quasar-in-arabic-part-1"
cascade:
  showReadingTime: true
series: ['كيفية بناء نظام إدارة محتوى بإستخدام Laravel 10 و Quasar Framework']
series_order: 1
categories: ['Development', 'Full Stack']
tags: ['laravel', 'vue', 'quasar', 'laravel api', 'spa']
---
## البنية الأساسية والبسيطة لنظام إدارة المحتوى CMS

{{< mermaid >}}
%%{
  init: {
    'theme': 'base',
    'themeVariables': {
      'primaryColor': '#BB2528',
      'primaryTextColor': '#fff',
      'primaryBorderColor': '#7C0000',
      'lineColor': '#F8B229',
      'secondaryColor': '#006100',
      'tertiaryColor': '#fff'
    }
  }
}%%
  graph TD
    A{نظام إدارة المحتوى} --> B(لوحة التحكم)
    A --> C(الصفحة الرئيسية)
    B --> M[المصادقة]
    B --> L[المستخدمين]
    B --> F[الأقسام]
    B --> E[المقالات]
    B --> D[التعليقات]
    C --> I[عرض المقالات]
    C --> H[عرض الأقسام]
    C --> G[عرض المقال مع التعليقات]
    D --> J[/الصور/]
    E --> J[/الصور/]
    F --> J[/الصور/]
    L --> J[/الصور/]
    M --> N[تسجيل مستخدم جديد]
    M --> O[تسجيل الدخول]
    M --> P[تسجيل الخروج]
{{< /mermaid >}}

كما نرى من الرسم التوضيحى أن لوحة التحكم تتكون حاليا من 5 موديلات
- Authantication ( المصادقة )
    - Register ( تسجيل مستخدم جديد )
    - Login ( تسجيل الدخول )
    - Logout ( تسجيل الخروج )
- المستخدم
- القسم
- المنشور
- التعليق
- الوسائظ (الصور)

## المتطلبات الأساسية
- تحميل سيرفر محلى مثل Xampp أو [Laragon](https://laragon.org/download/)
- تحميل [Composer](https://getcomposer.org/)
- تحميل محرر الأكواد مثل [Vscode](https://code.visualstudio.com/)
- تحميل بديل لموجه الأوامر الخاص بأنظمة التشغيل [Cmder](https://cmder.app/)
- PHP 8.1

## التأكد من إعدادات برنامج Laragon

1. افتح برنامج لاراجون وتوجه الى الإعدادات.  

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_menu.png"
alt="Laragon Menu"
caption="Laragon Menu"
>}}

{{< alert >}}
والتأكد من أنك قمت بنفس الإعدادات الموجودة فى الصورة التالية.
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_settings.png"
alt="Laragon Settings"
caption="Laragon Settings"
>}}

2. تفعيل شهادة SSL فى Laragon (إختيارى)
{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_ssl.png"
alt="Laragon SSL"
caption="Laragon SSL"
>}}

3. بعد ذلك قم بالضغط على start ثم terminal وستفتح معك نافذة موجه الأوامر Cmder إذا كنت قد قمت بتحميلها

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon.png"
alt="Laragon Window"
caption="Laragon Window"
>}}

{{< alert >}}
سيتم فتح نافذة جديدة على المجلد الأساسى ***`www`***
وهو الذى سيتم وضع فيه جميع مشاريعنا القادمة بإذن الله
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Cmder.png"
alt="Cmder"
caption="Cmder"
>}}

## إنشاء مشروع لارافيل جديد
هناك عدة طرق للقيام بهذا الأمر

1. عن طريق برنامج لاراجون نفسه
{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_quick_app.png"
alt="Laragon Quick App"
caption="Laragon Quick App"
>}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Quick_app_name.png"
alt="Quick App Name"
caption="Quick App Name"
>}}

{{< alert >}}
ولكن هذه الطريقة ترتبط بإصدار PHP المفعل فى برنامج Laragon
حيث يمكنك أن تقوم بتنزيل أكثر من إصدار من PHP حتى تتمكن من تشغيل أكثر من مشروع كلا على حسب إصداراه
وهذه الميزه هى التى يفتقدها برنامج Xammp.
{{< /alert >}}

2. عن طريق Laravel Installer
```bash
composer global require laravel/installer
laravel new project-name
```

3. عن طريق Composer وهذه الطريقة هى التى سنستخدمها

```bash
composer create-project laravel/laravel example-app
```

{{< alert >}}
سنقوم بتغيير ***`example-app`*** الى أسم المشروع الخاص بنا وسيكون ***`blog`***
{{< /alert >}}


{{< alert >}}
قم بإيقاف برنامج لاراجون وإعادة فتحه ثم افتح المتصفح وتوجه الى ***`blog.test`***
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laravel_in_browser.png"
alt="Laravel In Browser"
caption="Laravel In Browser"
>}}

## قاعدة البيانات
### إنشاء قاعدة البيانات
هناك طريفتين لإنشاء قاعدة البيانات:

1. عن طريق واجهة برنامج لاراجون

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_create_database.png"
alt="Laragon Create Database"
caption="Laragon Create Database"
>}}


{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/Laragon_database_name.png"
alt="Laragon Database Name"
caption="Laragon Database Name"
>}}

2. عن طريق موجه الأوامر Cmder

```bash
mysql -u root -p
```
- وبعد ذلك قم بالنقر على Enter
سيظهر لك enter password

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/mysql_access.png"
alt="Mysql Access"
caption="Mysql Access"
>}}

- لا تقم بكتابة شئ وأنقر Enter مرة  أخرى
{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/mysql_terminal.png"
alt="Mysql Terminal"
caption="Mysql Terminal"
>}}

- ثم سنقوم بإنشاء قاعدة البيانات عن طريق الأمر التالى
```bash
create database blog;
```
{{< alert >}}
يمكنك تسمية قاعدة البيانات بالأسم الذى تريده. وهنا ستكون ***`blog`***
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/mysql_database_created.png"
alt="Mysql Database Created"
caption="Mysql Database Created"
>}}

### ربط قاعدة البيانات مع المشروع
- سنقوم بالدخول الى مجلد المشروع

```bash
cd blog
```

- ونقوم بنسخ ملف ***`.env.example`*** الى ***`.env`***

```bash
cp .env.example .env
```
- ثم سنقوم بفتح المشروع عن طريق vscode

```bash
code .
```

- أذهب الى ملف ***`.env`*** وقم بتعديل بيانات قاعدة البيانات
```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=
```

- لنقم بكتابة هذا الأمر ونرى إذا كان قاعدة البيانات تم ربطها أم لا
```bash
php artisan migrate
```

{{< alert >}}
هذا الأمر يقوم بعمل migrate أى إنشاء الجداول فى قاعدة البيانات.
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/php_artisan_migrate.png"
alt="php artisan migrate"
caption="php artisan migrate"
>}}


## المكتبات المطلوبة
سنستخدم فى لوحة التحكم عدة مكتبات وأطر عمل أهمها

- Frontend
    - [Vue js v3.]({{< ref "#install-vue3" >}})
    - [Quasar Framework.]({{< ref "#install-quasar" >}})
    - [Vue Router.]({{< ref "#install-vue-router" >}})
    - [Vuex.]({{< ref "#install-vuex" >}})
    - [Axios.]({{< ref "#install-axios" >}})
    - [Vuelidate.]({{< ref "#install-vuelidate" >}})
    - [Filepond.]({{< ref "#install-filepond" >}})
- Backend
    - [Laravel Modules.]({{< ref "#install-laravel-modules" >}})
    - [Laravel Sanctum.]({{< ref "#install-laravel-sanctum" >}})
    - [Spatie Media Library.]({{< ref "#install-spatie-media-library" >}})
    - [Spatie Permission.]({{< ref "#install-spatie-permission" >}})

### تحميل فيو 3 داخل مشروع لارافيل {#install-vue3}
1. قم بتنفيذ الأوامر الأتية فى موجه الأوامر Cmder
```bash
npm install
npm install vue@next vue-loader@next
npm i @vitejs/plugin-vue
```

2. قم بتعديل ملف ***`vite.config.js`***
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

3. قم بتعديل ملف ***`app.js`*** داخل مجلد ***`resources/js`***
```js
import {createApp} from 'vue'

import App from './App.vue'

createApp(App).mount("#app")
```

4. قم بإنشاء ملف ***`App.vue`*** داخل مجلد ***`resources/js`***
```HTML
<template>
  <h1>Dashboard</h1>
</template>
```

5. قم بتعديل ملف ***`web.php`*** داخل مجلد ***`routes/web.php`***
```PHP
// Main Blog
Route::get('/', function () {
    return view('blog');
})
->name('home');

// Dashboard
Route::get('/dashboard/{any?}', function () {
    return view('dashboard');
})
->where('any', '^(?!api).*')
->name('dashboard');
```

6. قم أيضا بإضافة ملف ***`dashboard.blade.php`*** داخل مجلد ***`resources/views`***
{{< alert >}}
تأكد من إضافة ملف css و javascript الخاصة بمكتبة vite كما هو موضح وأيضا div ب id=app
{{< /alert >}}
```HTML {hl_lines=["7", "11-12"]}
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Dashboard</title>
        @vite('resources/css/app.css')
    </head>
    <body>
        <div id="app"></div>
        @vite('resources/js/app.js')
    </body>
</html>
```

6. قم أيضا بإضافة ملف ***`blog.blade.php`*** داخل مجلد ***`resources/views`***
```HTML
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>My Blog</title>
    </head>
    <body>
        <h1>My Blog</h1>
    </body>
</html>
```

- الأن سنذهب الى المتصفح ونرى النتائج
    - ***`blog.test`*** => الصفحة الرئيسية للمدونة
    - ***`blog.test/dashboard`*** => لوحة التحكم

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/my_blog.png"
alt="My Blog"
caption="My Blog"
>}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/dashboard.png"
alt="Dashboard"
caption="Dashboard"
>}}


### تحميل Quasar Framework داخل لارافيل {#install-quasar}
1. قم بتنفيذ الأوامر الأتية فى موجه الأوامر Cmder
```bash
npm install quasar @quasar/extras
npm install -D @quasar/vite-plugin sass@1.32.12
```

2. قم بتعديل ملف ***`vite.config.js`***
```js {hl_lines=["4", "7-16", "18-28"]}
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";
import vue from "@vitejs/plugin-vue";
import { quasar, transformAssetUrls } from "@quasar/vite-plugin";

export default defineConfig({
    build: {
        chunkSizeWarningLimit: 1000,
    },
    // Commented In Production Build
    server: {
        hmr: {
            protocol: "ws",
            host: "localhost",
        },
    },
    plugins: [
        vue({
            template: {
                transformAssetUrls: {
                    base: null,
                    includeAbsolute: false,
                },
            },
        }),
        quasar({
            sassVariables: "resources/sass/quasar-variables.sass",
        }),
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
    ],
});
```
3. قم بإنشاء ملف ***`quasar-variables.sass`*** داخل مجلد ***`resources/sass`***
```css
$primary: #9C27B0
$secondary: #26A69A
$accent: #1976D2

$dark: #1d1d1d
$dark-page: #15181e

$positive: #21BA45
$negative: #C10015
$info: #31CCEC
$warning: #F2C037
```
4. قم بتعديل ملف ***`App.vue`*** ولنقم بإضافة الزر الموجود قى Quasar لنرى هل سيظهر خطأ أم لا.
```HTML {hl_lines=["3"]}
<template>
  <h1>Dashboard</h1>
  <q-btn color="primary" icon="check" label="OK" />
</template>
```

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/quasar_btn_added.png"
alt="Quasar Btn Added"
caption="Quasar Btn Added"
>}}


### تحميل Vue Router {#install-vue-router}
1. قم بتنفيذ الأمر التالى فى موجه الأوامر Cmder
```bash
npm install vue-router@4
```

2. قم بإنشاء ملف ***`index.js`*** داخل ملف المجلد ***`resources/js/router`***
```js
import { createRouter, createWebHistory } from 'vue-router'

const routes = [];

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

3. قم بتعديل ملف ***`app.js`*** داخل محلد ***`resources/js`***
```js {hl_lines=["13-14", "18"]}
import "./bootstrap";

import { createApp } from "vue";
import { Quasar } from "quasar";
import App from "./App.vue";

// Import icon libraries
import '@quasar/extras/material-icons/material-icons.css'

// Import Quasar css
import 'quasar/src/css/index.sass'

// Import Vue Router
import router from './router'

const myApp = createApp(App)

myApp.use(router);
myApp.use(Quasar);
myApp.mount("#app");
```

4. قم بتعديل ملف ***`App.vue`*** داخل مجلد ***`resources/js`***
```HTML {hl_lines=["2"]}
<template>
  <RouterView />
</template>
```

### تحميل Vuex {#install-vuex}

1. قم بتنفيذ الأمر التالى فى موجه الأوامر Cmder
```bash
npm install vuex@next --save
```

2. قم بإنشاء ملف ***`index.js`*** داخل ملف المجلد ***`resources/js/store`***
```js
import { createLogger, createStore } from 'vuex'

// Create a new store instance.
const store = createStore({
  plugins: [createLogger()],
  state: {},
  mutations: {},
  getters: {},
  modules: {}
})

export default store
```

3. قم بتعديل ملف ***`app.js`*** داخل محلد ***`resources/js`***
```js {hl_lines=["16-17", "22"]}
import "./bootstrap";

import { createApp } from "vue";
import { Quasar } from "quasar";
import App from "./App.vue";

// Import icon libraries
import '@quasar/extras/material-icons/material-icons.css'

// Import Quasar css
import 'quasar/src/css/index.sass'

// Import Vue Router
import router from './router'

// Import Vuex
import store from './store'

const myApp = createApp(App)

myApp.use(router);
myApp.use(store);
myApp.use(Quasar);
myApp.mount("#app");
``` 

### تحميل Axios {#install-axios}
1. قم بتنفيذ الأمر التالى فى موجه الأوامر Cmder
```bash
npm install axios
```

2. قم بإنشاء ملف ***`request.js`*** داخل ملف المجلد ***`resources/js/utils`***
```js
import axios from "axios";
// Create axios instance
const service = axios.create({
  baseURL: "/api",
  timeout: 10000 // Request timeout
});

// Request intercepter
service.interceptors.request.use(
  config => {},
  error => {
    // Do something with request error
    console.log("error-axios", error); // for debug
    Promise.reject(error);
  }
);

// response pre-processing
service.interceptors.response.use(
  response => {
    return response;
  },
  error => {
    return Promise.reject(error);
  }
);

export default service;
```

3. قم بإنشاء ملف ***`resource.js`*** داخل ملف المجلد ***`resources/js/utils`***
```js
// resource.js
import request from './request';

/**
 * Simple RESTful resource class
 */
class Resource {
    constructor(uri) {
        this.uri = uri;
    }
    list(query) {
        return request({
            url: "/" + this.uri,
            method: "get",
            params: query
        });
    }
    get(id) {
        return request({
            url: "/" + this.uri + "/" + id,
            method: "get"
        });
    }
    store(resource) {
        return request({
            url: "/" + this.uri,
            method: "post",
            data: resource
        });
    }
    update(id, resource) {
        return request({
            url: "/" + this.uri + "/" + id,
            method: "put",
            data: resource
        });
    }
    destroy(id) {
        return request({
            url: "/" + this.uri + "/" + id,
            method: "delete"
        });
    }
    bulk_destroy(resource) {
        return request({
            url: "/" + this.uri + "/bulk_destroy",
            method: "post",
            data: resource
        });
    }
}

export { Resource as default };
```

### تحميل Vuelidate {#install-vuelidate}
1. قم بتنفيذ الأمر التالى فى موجه الأوامر Cmder
```bash
npm install @vuelidate/core @vuelidate/validators
```
- سنقوم بإستخدام هذه المكتية لاحقا

### تحميل Filepond {#install-filepond}
1. قم بتنفيذ الأمر التالى فى موجه الأوامر Cmder
```bash
npm install vue-filepond filepond
```
- سنقوم بإستخدام هذه المكتية لاحقا

### تحميل Laravel Modules {#install-laravel-modules}
 1. قم بتنفيذ الأمر التالي فى موجه الأوامر Cmder
```bash
composer require nwidart/laravel-modules
```
- هذا الأمر اختيارى لتحميل ملف الإعدادات الخاص بالمكتبة
```bash
php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"
```
2. قم بتعديل ملف ***`composer.json`***
```json
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/",
      "Database\\Factories\\": "database/factories/",
      "Database\\Seeders\\": "database/seeders/"
  }

}
```
3. قم بتنفيذ الأمر التالى فى موحه الأوامر Cmder
```bash
composer dump-autoload
```

### تحميل Laravel Sanctum {#install-laravel-sanctum}
 1. قم بتنفيذ الأمر التالي فى موجه الأوامر Cmder
```bash
composer require laravel/sanctum
```
- هذا الأمر اختيارى لتحميل ملف الإعدادات الخاص بالمكتبة
```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```
2. قم بتنفيذ هذا الأمر لعمل تعديلات على قاعدة البيانات
```bash
php artisan migrate
```

### تحميل Spatie Media Library {#install-spatie-media-library}
 1. قم بتنفيذ الأمر التالي فى موجه الأوامر Cmder
```bash
composer show spatie/laravel-medialibrary
```
2. هذا الأمر سيقوم بتحميل ملفات قاعدة البيانات الخاصة بتلك المكتبة
```bash
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="migrations"
```
3. قم بتنفيذ هذا الأمر لعمل تعديلات على قاعدة البيانات
```bash
php artisan migrate
```

- هذا الأمر اختيارى لتحميل ملف الإعدادات الخاص بالمكتبة
```bash
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="config"
```

### تحميل Spatie Permission {#install-spatie-permission}
 1. قم بتنفيذ الأمر التالي فى موجه الأوامر Cmder
```bash
 composer require spatie/laravel-permission
```
2. هذا الأمر سيقوم بتحميل ملف الإعدادات وملفات قاعدة البيانات الخاصة بتلك المكتبة
```bash
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```
3. قم بتنفيذ هذه الأوامر لعمل حذف للكاش وعمل تعديلات على قاعدة البيانات
```bash
 php artisan optimize:clear
 php artisan migrate
```

##  صفحات التخطيط الأساسية - Layouts
سنستخدم نوعين من تصميم الصفحات وسنرى هذا داخل التطبيق :
1. Blank Layout ( التصميم الفارغ )
    - صفحة تسجيل مستخدم جديد
    - صفحة تسجيل الدخول
    - لا تحتوى على ( Header - Footer - Sidebasr )
2. Content Layout ( تصميم المحتوى )
    - لوحة التحكم من الداخل
    - تحتوى على ( Header - Footer - Sidebasr )

- قم بإنشاء ملف ***`Blank.vue`*** داخل مجلد ***`resources/js/layouts`***
```HTML
<template>
  <q-layout>
    <q-page-container>
      <!-- محتوى الصفحة -->
      <slot />
    </q-page-container>
  </q-layout>
</template>
```

- قم بإنشاء ملف ***`Content.vue`*** داخل مجلد ***`resources/js/layouts`***
```HTML
<script setup>
import { ref } from "vue";
const drawer = ref(false);
</script>

<template>
    <q-layout
        view="lHh Lpr lff"
        class="shadow-2 rounded-borders"
    >
        <q-header elevated class="bg-cyan-8">
            <q-toolbar>
                <q-toolbar-title>Header</q-toolbar-title>
                <q-btn flat @click="drawer = !drawer" round dense icon="menu" />
            </q-toolbar>
        </q-header>

        <q-drawer v-model="drawer" show-if-above :width="200" :breakpoint="400">
            <q-scroll-area
                style="
                    height: calc(100% - 150px);
                    margin-top: 150px;
                    border-right: 1px solid #ddd;
                "
            >
                <q-list padding>
                    <q-item clickable v-ripple exact :to="{name: 'users.list'}">
                        <q-item-section avatar>
                            <q-icon name="people" />
                        </q-item-section>

                        <q-item-section> Users </q-item-section>
                    </q-item>
                </q-list>
            </q-scroll-area>

            <q-img
                class="absolute-top"
                src="https://cdn.quasar.dev/img/material.png"
                style="height: 150px"
            >
                <div class="absolute-bottom bg-transparent">
                    <q-avatar size="56px" class="q-mb-sm">
                        <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
                    </q-avatar>
                    <div class="text-weight-bold">Razvan Stoenescu</div>
                    <div>@rstoenescu</div>
                </div>
            </q-img>
        </q-drawer>

        <q-page-container>
            <slot />
        </q-page-container>
    </q-layout>
</template>
```

- قم بتعديل ملف ***`App.vue`*** داخل مجلد ***`resources/js`***
```HTML
<script setup>
import { computed } from "vue";
import { useRoute } from "vue-router";
import { defineAsyncComponent } from "vue";

const LayoutBlank = defineAsyncComponent(() => import("./layouts/Blank.vue"));
const LayoutContent = defineAsyncComponent(() =>
    import("./layouts/Content.vue")
);

const route = useRoute();
const layouts = {
    blank: LayoutBlank,
    content: LayoutContent,
};
const resolveLayout = computed(() => {
    if (!route.name) return;

    if (route.meta.layout === "blank") return layouts["blank"];

    return layouts["content"];
});
</script>
<template>
    <component v-if="resolveLayout" :is="resolveLayout">
        <Suspense>
            <template #default>
                <RouterView />
            </template>
            <template #fallback>
                <div class="fixed-center">
                    <q-spinner-pie color="primary" size="10em" />
                </div>
            </template>
        </Suspense>
    </component>
</template>
```


## تقسيم المشروع الى Modules
1. فى الباك إند
{{< lead >}}
سنقوم بإستخدام مكتبة [nWidart/laravel-modules](#install-laravel-modules) لتنظيم المشروع الخاص بنا والتى قمنا بتتنزيلها سابقا
{{< /lead >}}

- وهذه هى تقسيمة الملفات لكل موديول
{{< alert >}}
كما تلاحظ فهى نفس تقسيمة لارافيل فكل موديول كأنه مشروع لارافيل مستقل عن الموديول الأخر.
{{< /alert >}}
```
Modules/
  ├── ModuleA/
      ├── Config/
      ├── Console/
      ├── Database/
          ├── factories/
          ├── Migrations/
          ├── Seeders/
      ├── Entities/
      ├── Http/
          ├── Controllers/
          ├── Middleware/
          ├── Requests/
      ├── Providers/
          ├── BlogServiceProvider.php
          ├── RouteServiceProvider.php
      ├── Resources/
          ├── assets/
          ├── lang/
          ├── views/
      ├── Routes/
          ├── api.php
          ├── web.php
      ├── Tests/
      ├── composer.json
      ├── module.json
      ├── package.json
      ├── webpack.mix.js
```
2. فى الفرونت إند
{{< lead >}}
لن نستخدم مكتبة ولكن سنقوم بتقسيم ملفات المشروع كما يلى:
{{< /lead >}}

```
├── resources
│   ├── js
│   │   ├── models
│   │   │   ├── ModuleA.js
│   │   │   ├── ModuleB.js
│   │   │   ├── ModuleC.js
│   │   ├── modules
│   │   │   ├── ModuleA
│   │   │   │   ├── components
│   │   │   │   │   ├── Form.vue
│   │   │   │   ├── store
│   │   │   │   │   ├── actions.js
│   │   │   │   │   ├── getters.js
│   │   │   │   │   ├── index.js
│   │   │   │   │   ├── mutations.js
│   │   │   │   │   ├── state.js
│   │   │   │   ├── views
│   │   │   │   │   ├── Create.vue
│   │   │   │   │   ├── Edit.vue
│   │   │   │   │   ├── List.vue
│   │   │   │   ├── index.js
│   │   │   │   ├── Module.vue
│   │   │   │   ├── router.js
│   │   │   ├── ModuleB
│   │   │   ├── ModuleC
│   │   ├── register_modules.js
```

## إنشاء ملف تسجيل الموديلات
{{< alert >}}
قم بإنشاء ملف ***`register_modules.js`*** داخل مجلد ***`js`***
{{< /alert >}}
```js
// register_modules.js
import router from './router'
import store from './store'

const registerModule = (name, module) => {
  if (module.store) {
    store.registerModule(name, module.store)
  }

  if (module.router) {
    module.router(router)
  }
}

export const registerModules = modules => {
  Object.keys(modules).forEach(moduleKey => {
    const module = modules[moduleKey]
    registerModule(moduleKey, module)
  })
}
```
- وظيفة هذا الملف تسجيل Routes و Store يطريقة Dynamic
- قم بعمل import للملف داخل ***`app.js`***
```js {hl_lines=["18-19"]}
// app.js
import { createApp } from "vue";
import { Quasar } from "quasar";
import App from "./App.vue";

// Import icon libraries
import '@quasar/extras/material-icons/material-icons.css'

// Import Quasar css
import 'quasar/src/css/index.sass'

// Import Vue Router
import router from './router'

// Import Vuex
import store from './store'

// Import Register Module
import { registerModules } from './register_modules'


const myApp = createApp(App)

myApp.use(router);
myApp.use(store);
myApp.use(Quasar);
myApp.mount("#app");
```

## إنشاء موديول المستخدم ( User Module )
كل موديول سينقسم الى باك إند وفرونت إند:
1. الفرونت إند
{{< lead >}}
 لنقم بإنشاء جميع المجلدات والملفات المطلوبة كما هى موضحة فى الصورة.
{{< /lead >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/frontend_module.png"
alt="Frontend Module"
caption="Frontend Module"
>}}

2. الباك إند
- الجزء الخاص بالتعامل مع قاعدة البيانات وهنا سنستخدم مكتبة Laravel Modules التى قمنا بتحميلها
- سنستخدم هذا الأمر للقيام بإنشاء موديول جديد
```bash
php artisan module:make User
```
{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/backend_module.png"
alt="Backend Module"
caption="Backend Module"
>}}
