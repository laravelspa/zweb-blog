---
title: "كيفية بناء نظام إدارة محتوى بإستخدام Laravel 10 & Quasar Frameweork (2)"
date: 2023-05-01
draft: true
slug: "how-to-build-cms-with-laravel-10-and-quasar-in-arabic-part-2"
cascade:
  showReadingTime: true
series: ['كيفية بناء نظام إدارة محتوى بإستخدام Laravel 10 و Quasar Framework']
series_order: 2
categories: ['Development', 'Full Stack']
tags: ['laravel', 'vue', 'quasar', 'laravel api', 'spa']
---
## Part 2
1. ملف ***`Module.vue`*** وهو الملف الأب للموديول
```HTML
<!-- Module.vuw -->
<template>
    <q-page padding>
        <RouterView />
    </q-page>
</template>
```

2. سنقوم بإنشاء داخل مجلد ***`js/resources/modules/user/views`*** بعض الملفات
    - ملف ***`Create.vue`***
    - ملف ***`Edit.vue`***
    - ملف ***`List`***
```HTML
<!-- Create.vue -->
<template>
    <div>Create</div>
</template>
```

```HTML
<!-- Edit.vue -->
<template>
    <div>Edit</div>
</template>
```

```HTML
<!-- List.vue -->
<template>
    <div>List</div>
</template>
```

3. سنقوم أيضا بإنشاء ملف ***`Form.vue`*** داخل مجلد ***`js/resources/modules/user/components`***
```HTML
<!-- Form.vue -->
<template>
    <div>Form</div>
</template>
```

4. ملف ***`routes.js`*** وهو الذى يحتوى على جميع الروابط الخاصة بهذا الموديول
```js
// routes.js
const moduleRoutes = {
  path: "/dashboard/users",
  component: () => import('./Module.vue'),
  children: [
    {
      path: '',
      name: 'users.list',
      component: () => import('./views/List.vue'),
      meta: {
        layout: 'content',
        auth: true,
        title: 'user',
      },
    },
    {
      path: 'create',
      name: 'user.create',
      component: () => import('./views/Create.vue'),
      meta: {
        layout: 'content',
        auth: true,
        title: 'user.create',
      },
    },
    {
      path: ':id/edit',
      name: 'user.edit',
      component: () => import('./views/Edit.vue'),
      meta: {
        layout: 'content',
        auth: true,
        title: 'user.edit',
      },
    },
  ]
};

export default router => {
  router.addRoute(moduleRoutes)
}
```

5. سنقوم بإنشاء داخل مجلد ***`js/resources/modules/user/store`*** بعض الملفات
    - ملف ***`state.js`***
    - ملف ***`actions.js`***
    - ملف ***`mutations.js`***
    - ملف ***`index.js`***

```js
// state.js
export default () => ({
    users: [],
    user: {},
    pagination: {
        sortBy: "id",
        descending: true,
        page: 1,
        rowsPerPage: 25,
        rowsNumber: 0,
    }
})
```

```js
// actions.js
export const fetchUsers = ({ commit }, options) => {
  return new Promise((resolve, reject) => {
    
  })
};

export const fetchUser = ({ commit }, id) => {
  return new Promise((resolve, reject) => {
    
  })
};

export const createUser = async (_, user) => {
  return new Promise((resolve, reject) => {
    
  })
};

export const updateUser = async (_, user) => {
  return new Promise((resolve, reject) => {

  })
};

export const destroyUser = (_, id) => {
  return new Promise((resolve, reject) => {

  })
};

export const bulkDestroyUsers = (_, ids) => {
  return new Promise((resolve, reject) => {

  })
};
```

```js
// mutations.js
export const SET_USERS = (state, users) => {}

export const SET_USER = (state, user) => {}

export const SET_PAGINATION = (state, meta) => {}
```

```js
// index.js
import state from './state'
import * as mutations from './mutations'
import * as actions from './actions'

export default {
    namespaced: true,
    state,
    mutations,
    actions,
}
```

6. ملف ***`index.js`*** داخل مجلد ***`js/resources/modules/user`***
```js
// index.js
import store from "./store";
import router from "./router";

export default {
  store,
  router
}
```

7. ثم سنحتاج الى إنشاء ملف ***`User.js`*** داخل مجلد ***`resources/js/models`***
```js
// User.js
import request from "../utils/request";
import Resource from "../utils/resource";

class User extends Resource {
    constructor() {
        super("users");
    }
}

export { User as default };
```

{{< alert >}}
Class User هيورث Methods الموجودة فى ملف ***`resource.js`***.
{{< /alert >}}

8. وأخيرا لكى يتم تسجيل هذا الموديول داخل المشروع يجب أن نقوم بتعديل ملف ***`app.js`***
```js {hl_lines=["21-22", "24-26"]}
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

// Import User Module
import userModule from './modules/user'

registerModules({
  user: userModule
})

const myApp = createApp(App)

myApp.use(router);
myApp.use(store);
myApp.use(Quasar);
myApp.mount("#app");
```


{{< alert >}}
لو قمت بفتح المتصفح الأن وذهبت الى ***`blog.test/dashboard/users`*** ستجد ما يلى.
{{< /alert >}}

{{< figure
src="/img/how-to-build-a-blog-with-laravel-10-and-vue-3/app_view.png"
alt="App View"
caption="App View"
>}}
