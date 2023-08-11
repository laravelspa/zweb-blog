---
title: "Laravel Eloquent One-To-Many Relationship - Ultimate Guide 2023 In Arabic"
date: 2023-08-07
draft: true
slug: "laravel-eloquent-one-to-many-relationship-ultimate-guide-2023-in-arabic"
description: ""
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel Eloquent Relationships']
series_order: 2
---

بعد أن تعرفنا على أنواع العلاقات داخل __Laravel__ فى الحزء السابق.
وقمنا بتناول أول نوع من هذه العلاقات وهى علاقة One-To-One.

اليوم نستكمل السلسلة  التى بدأناها فى تعلم __Laravel Eloquent Relationships__.

و نتكلم عن النوع الثانى والذى يطلق عليه __One-To-Many__ او __hasMany__.

{{< figure
src="/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/laravel10-one-to-one-relationship.png"
alt="laravel10 one to one relationship"
caption="laravel10 one to one relationship"
>}}

## كيف تنشئ علاقة One-To-Many فى Laravel؟
 تعتير علاقة One-To-Many من أهم أنواع العلاقات الموجودة داخل __Laravel Eloquent__.
 وعرفنا أيضا فى الدرس السابق أنها عبارة عن ارتباط صف من الجدول الأول بأكثر من صف واحد من الجدول الثانى.

واستكمالاً للتطبيق العملى (نظام إدارة المحتوى) الذى بدأناه فى الدرس السابق.
وقمنا بإنشاء علاقة __One-To-One__ بين المستخدم وملف التعريف الضخصى.

اليوم سنقوم بإنشاء علاقة __One-To-Many__ بين المستخدم و المنشور.
فكل مستخدم يستطيع أن يمتلك منشور او أكثر.

* نقوم بإنشاء ***`Post Model`*** مع الجدول الخاص به.
```bash
php artisan make:model Post -m
```

* نقوم بالتوجه الى هذا المسار ***`database/migrations`*** والتعديل على جدول المنشورات بإضافة بعض الأعمدة كما يلى:
```PHP
Schema::create('posts', function (Blueprint $table) {
  $table->id();
  $table->string('title');
  $table->text('body');
  $table->foreignId('user_id')->constrained();
  $table->timestamps();
});
```

* نقوم بتعديل ملف ***`Post.php`***.
```PHP
protected $fillable = [
  'user_id',
  'title',
  'body',
];
```

* نقوم بتنفيذ هذا الأمر لتحديث قاعدة البيانات وإضافة جدول المتشورات.
```bash
php artisan migrate
```

* نقوم بالتوجه الى ملف ***`User.php`*** وتعيين علاقة ***`hasMany`***.
```PHP
public function posts() {
    return $this->hasMany(Post::class);
}
```


> لنتعرف كيف تعمل ***`hasMany`***
```PHP
$this->hasMany(Post::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* نقوم بالتوجه الى ملف ***`Post.php`*** وتعيين العلاقة العكسية ***`belongsTo`***.
```PHP
public function user() {
    return $this->belongsTo(User::class);
}
```

> ولقد قمنا بشرح ***`belongsTo`*** فى هذا الجزء من المقال السابق ونحن نقوم بشرح علاقة __One-To-One__.

## كيفية حفظ علاقة one to Many فى قاعدة البيانات؟
بعد أن قمنا بإنشاء علاقة One-To-Many بين كلا من جدول المستخدمين وجدول المنشورات وأضفنا ***`hasMany`*** داخل ***`User Model`***, وأيضا قمنا بإضافة العلاقة العكسية داخل ***`Post Model`*** بإضافة ***`belongsTo`*** إليه.

 جاء الوقت لمعرفة كيف يتم حفظ البيانات فى قاعدة البيانات أثناء إستخدامنا لهذه العلاقة.
وما هى الطرق المستخدمة فى ذلك


تنقسم هذه الطرق الى ثلاث طرق اساسية:
1. بدون إستخدام **function post**.
2. عن طريق إستخدام **function post**.
3. عن طريق إستخدام العلاقة العكسية **function user**.

### 1. بدون إستخدام **function post**.

هنا يوجد سيناريوهان:
* أولا: إضافة منشور واحد فقط للمستخدم.
* ثانيا: إضافة أكثر من منشور للمستخدم.

#### أولا: إضافة منشور واحد فقط للمستخدم.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نضيف رابط جديد حتى نستطيع أن نختبر هذه الطرق.
```PHP
Route::get('/one-to-many', function () {
    $user = \App\Models\User::find(1);
    \App\Models\Post::create([
        'user_id' => $user->id,
        'title' => 'Post title 1',
        'body' => 'Post body 1',
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts
    ]);
});
```

* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-many`*** لنجد أنه تم إضافة منشور واحد بنجاح الى المستخدم صاخبId رقم 1.
```json
{
  "username": "John Doe",
  "post": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-08-05T03:18:22.000000Z",
      "updated_at": "2023-08-05T03:18:22.000000Z"
    }
  ]
}
```

#### ثانيا: إضافة أكثر من منشور للمستخدم.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-many', function () {
   $user = \App\Models\User::find(2);
    \App\Models\Post::insert(
        [
            [
                'user_id' => $user->id,
                'title' => 'Post title 2',
                'body' => 'Post body 2',
            ],
            [
                'user_id' => $user->id,
                'title' => 'Post title 3',
                'body' => 'Post body 3',
            ],
        ]
    );
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts
    ]);
});
```

* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-many`*** لنجد أنه تم إضافة عدد 2 منشور الى المستخدم صاحب Id رقم 2 بنجاح.
```json
{
  "username": "Tom Cruz",
  "post": [
    {
      "id": 2,
      "title": "Post title 2",
      "body": "Post body 2",
      "user_id": 2,
      "created_at": null,
      "updated_at": null
    },
    {
      "id": 3,
      "title": "Post title 3",
      "body": "Post body 3",
      "user_id": 2,
      "created_at": null,
      "updated_at": null
    }
  ]
}
```

### 2. عن طريق إستخدام **function post**.
هنا أيضا يوجد سيناريوهان:
أولا: إضافة منشور واحد فقط للمستخدم
ثانيا: إضافة أكثر من منشور للمستخدم


#### أولا: إضافة منشور واحد فقط للمستخدم.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-many', function () {
    $user = \App\Models\User::find(3);
    $user->posts()->create([
      'title' => 'Post title 4',
      'body' => 'Post body 4',
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts,
    ]);
});
```

* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-many`*** لنجد أنه تم إضافة منشور واحد الى المستخدم صاحب Id رقم 3 بنجاح.
```json
{
  "username": "Adam Smith",
  "post": [
    {
      "id": 4,
      "title": "Post title 4",
      "body": "Post body 4",
      "user_id": 3,
      "created_at": "2023-08-05T03:37:55.000000Z",
      "updated_at": "2023-08-05T03:37:55.000000Z"
    }
  ]
}
```

#### ثانيا: إضافة أكثر من منشور للمستخدم.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-many', function () {
   $user = \App\Models\User::find(1);
    $user->posts()->createMany([
        [
            'title' => 'Post title 5',
            'body' => 'Post body 5',
        ],
        [
            'title' => 'Post title 6',
            'body' => 'Post body 6',
        ]
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts,
    ]);
});
```

* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-many`*** لنجد أنه تم إضافة عدد 2 منشور الى المستخدم صاحب Id رقم 1 بنجاح.

{{< alert >}}
بالإضافة الى تلك المنشورات هناك منشور أخر تم إضافته فى خطوة سابقة وبالتالى فالمفروض وجود عدد 3 منشورات لهذا الستخدم. وهذه بالفعل البيانات التى تم الحصول عليها من قاعدة البيانات فى الإستجابة التالية. 
{{< /alert >}}

```json
{
  "username": "John Doe",
  "post": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-08-05T03:18:22.000000Z",
      "updated_at": "2023-08-05T03:18:22.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 3,
      "created_at": "2023-08-05T03:42:27.000000Z",
      "updated_at": "2023-08-05T03:42:27.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 3,
      "created_at": "2023-08-05T03:42:27.000000Z",
      "updated_at": "2023-08-05T03:42:27.000000Z"
    }
  ]
}
```

### 3. عن طريق إستخدام العلاقة العكسية **function user**.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-many', function () {
    $user = \App\Models\User::find(2);
    $post = new \App\Models\Post([
        'title' => 'Post title 7',
        'body' => 'Post body 7',
    ]);

    $post->user()->associate($user)->save();
    return response()->json([
        'username' => $post->user->username,
        'title' => $post->title,
        'body' => $post->body,
    ]);
});
```

* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-many`*** لنجد أنه تم إضافة منشور الى المستخدم صاحب Id رقم 2 بنجاح.
```json
{
  "username": "Tom Cruz",
  "title": "Post title 7",
  "body": "Post body 7"
}
```

## كيف تحصل على البيانات في علاقة One-To-One في Laravel؟
رأينا كيف يتم حفظ البيانات بطرق مختلفة ومتعددة داخل قاعدة البيانات بإستخدام علاقة One-To-Many فى Laravel.
ولكن كيف يتم جلب البيانات من قاعدة البيانات؟
هناك عدة طرق يمكن من خلالها جلب البيانات من قاعدة البيانات.
وتنقسم هذه الطرق الى طريقتين أساسيتين:

### من خلال المستخدم ***`User Model`***
تنقسم هذه الطريقة الى طريقتين فرعيتين تعتمد على كيفية تنظيم البيانات بعد جلبها من قاغدة البيانات

#### بدون إستخدام ***`API Resources`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  حتى نستطيع أن نختبر هذه الطرق ونقوم بالتعديلات التالية.
```PHP
Route::get('/users', function () {
    $users = \App\Models\User::with(['profile', 'posts'])->get();
    return view('users.list', compact('users'));
});
```
 هنا يمكننا اختيار الاعمده الخاصه بكل علاقة بالشكل التالي.
```PHP
Route::get('/users', function () {
    $users = \App\Models\User::with(['profile:id,firstname,lastname,user_id', 'posts:title,user_id'])->get();
    return view('users.list', compact('users'));
});
```

ولكى نعرف الفرق بين تحديدنا او عدم تحديدنا للأعمدة من قاعدة البيانات تعالوا نرى الأستجابة الخاصة لكل حالة
* الخالة الأولى مع جلب البيانات جميعها بدون إستثناء نجد أن ***`$users`*** يحتوى على البيانات تلك.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe",
      "birthday": "08-11-1991",
      "user_id": 1,
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    },
    "posts": [
      {
        "id": 1,
        "title": "Post title 1",
        "body": "Post body 1",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      },
      {
        "id": 5,
        "title": "Post title 5",
        "body": "Post body 5",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      },
      {
        "id": 6,
        "title": "Post title 6",
        "body": "Post body 6",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 2,
      "firstname": "Tom",
      "lastname": "Cruz",
      "birthday": "01-02-2000",
      "user_id": 2,
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    },
    "posts": [
      {
        "id": 2,
        "title": "Post title 2",
        "body": "Post body 2",
        "user_id": 2,
        "created_at": null,
        "updated_at": null
      },
      {
        "id": 3,
        "title": "Post title 3",
        "body": "Post body 3",
        "user_id": 2,
        "created_at": null,
        "updated_at": null
      },
      {
        "id": 7,
        "title": "Post title 7",
        "body": "Post body 7",
        "user_id": 2,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 3,
      "firstname": "Adam",
      "lastname": "Smith",
      "birthday": "01-01-1999",
      "user_id": 3,
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    },
    "posts": [
      {
        "id": 4,
        "title": "Post title 4",
        "body": "Post body 4",
        "user_id": 3,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
  }
]
```

* أما فى الحالة الثانية عند تحديد الأعمدة المطلوبة بالظبط من قاعدة البيانات نجد أن الإستجابة كالتالى.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe",
      "user_id": 1
    },
    "posts": [
      {
        "title": "Post title 1",
        "user_id": 1
      },
      {
        "title": "Post title 5",
        "user_id": 1
      },
      {
        "title": "Post title 6",
        "user_id": 1
      }
    ]
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 2,
      "firstname": "Tom",
      "lastname": "Cruz",
      "user_id": 2
    },
    "posts": [
      {
        "title": "Post title 2",
        "user_id": 2
      },
      {
        "title": "Post title 3",
        "user_id": 2
      },
      {
        "title": "Post title 7",
        "user_id": 2
      }
    ]
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "id": 3,
      "firstname": "Adam",
      "lastname": "Smith",
      "user_id": 3
    },
    "posts": [
      {
        "title": "Post title 4",
        "user_id": 3
      }
    ]
  }
]
```

> وهنا نرى الفرق بين الحالتين فى حجم البيانات التى تمت معالجتها ويزداد الأمر إذا كانت البيانات حجمها أكبر من ذلك بكثير.
> يجب عند إختيار إعمدة محددة فى العلاقات أن تختار ***foreignKey*** لأن بدون إختيارك له لن تعود البيانات بشكل صحيح من قاعدة البيانات.

2. نقوم بالذهاب الى المسار التالى ***`resources/views/users`*** ونقوم بتعديل هذا الملف ***`list.blade.php`*** لعرض المستخدمين وملفات التعريف الخاصة بهم وأيضا ما يهمنا هنا هو عرض المنشورات الخاصة لكل مستخدم.

```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
            <th style="text-align: center">Posts</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($users as $user)
            <tr>
                <td>{{ $user->username }}</td>
                <td>{{ $user->profile->firstname }}</td>
                <td>{{ $user->profile->lastname }}</td>
                <td>
                    <ul>
                        @foreach ($user->posts as $post)
                            <li>{{ $post->title }}</li>
                        @endforeach
                    </ul>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}


#### عن طريق إستخدام ***`API Resources`***.
* ما هى API Resources?
ببساطة هى طبقة وسيطة بين __Eloquent__ وما بين الإستجابة الخاصة ب API وتحويل تلك البيانات التى تم جلبها من ثاعدة البيانات الى JSON مع امكانية تحديد بيانات محددة دون غيرها أو تغيير نوع تلك البيانات.
1. سنقوم بإنشاء API Resource لكل من User, Profile ,Post عن طريق تنفيذ هذا الأمر فى موجه الأوامر.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
php artisan make:resource PostResource
```
2. تذهب الى المسار التالى ***`App/Http/Resources`*** ونبدأ بالتعديل على كلا من:

* ***ProfileResource.php***.
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class ProfileResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'firstname' => $this->firstname,
            'lastname' => $this->lastname,
        ];
    }
}
```

* ***PostResource.php***.
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'body' => $this->body,
        ];
    }
}
```

* ***UserResource.php***.
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class UserResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'username' => $this->username,
            'profile' => ProfileResource::make($this->whenLoaded('profile')),
            'postss' => PostResource::collection($this->whenLoaded('posts')),
        ];
    }
}
```


3. نقوم بالتوجه أولا لملف ***`routes/web.php`***  لنضيف رابط جديد.
```PHP
Route::get('/users-resource', function () {
    $users = \App\Models\User::with(['profile', 'posts'])->get();
    $usersResource = \App\Http\Resources\UserResource::collection($users);
    return response()->json($usersResource);
});
```

4. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users-resource`*** لنرى ما هى النتائج التى ستظهر.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "profile": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe"
    },
    "postss": [
      {
        "user_id": 1,
        "title": "Post title 1"
      },
      {
        "user_id": 1,
        "title": "Post title 5"
      },
      {
        "user_id": 1,
        "title": "Post title 6"
      }
    ]
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "profile": {
      "id": 2,
      "firstname": "Tom",
      "lastname": "Cruz"
    },
    "postss": [
      {
        "user_id": 2,
        "title": "Post title 2"
      },
      {
        "user_id": 2,
        "title": "Post title 3"
      },
      {
        "user_id": 2,
        "title": "Post title 7"
      }
    ]
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "profile": {
      "id": 3,
      "firstname": "Adam",
      "lastname": "Smith"
    },
    "postss": [
      {
        "user_id": 3,
        "title": "Post title 4"
      }
    ]
  }
]
```

> ونرى هنا أيضا أننا نجلب البيانات المطلوبة فقط.


### من خلال المنشور ***`Post Model`***
تنقسم هذه الطريقة الى طريقتين فرعيتين تعتمد على كيفية تنظيم البيانات بعد جلبها من قاغدة البيانات

#### بدون إستخدام ***`API Resources`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  حتى نستطيع أن نختبر هذه الطرق ونقوم بإضافة هذا الرابط الجديد.
```PHP
Route::get('/posts', function () {
    $posts = \App\Models\Post::with('user', 'user.profile')->get();
    return view('posts.list', compact('posts'));
});
```
 هنا يمكننا اختيار الاعمده الخاصه بكل علاقة بالشكل التالي.
```PHP
Route::get('/posts', function () {
    $posts = \App\Models\Post::with('user:username,id', 'user.profile:firstname,lastname,user_id')->get();
    return view('posts.list', compact('posts'));
});
```

ولكى نعرف الفرق بين تحديدنا او عدم تحديدنا للأعمدة من قاعدة البيانات تعالوا نرى الأستجابة الخاصة لكل حالة
* الخالة الأولى مع جلب البيانات جميعها بدون إستثناء نجد أن ***`$posts`*** يحتوى على البيانات تلك.
```json
[
  {
    "id": 1,
    "title": "Post title 1",
    "body": "Post body 1",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 2,
    "title": "Post title 2",
    "body": "Post body 2",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 3,
    "title": "Post title 3",
    "body": "Post body 3",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 4,
    "title": "Post title 4",
    "body": "Post body 4",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 3,
      "username": "Adam Smith",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 5,
    "title": "Post title 5",
    "body": "Post body 5",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 6,
    "title": "Post title 6",
    "body": "Post body 6",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 7,
    "title": "Post title 7",
    "body": "Post body 7",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  }
]
```

* أما فى الحالة الثانية عند تحديد الأعمدة المطلوبة بالظبط من قاعدة البيانات نجد أن الإستجابة كالتالى.
```json
[
  {
    "id": 1,
    "title": "Post title 1",
    "body": "Post body 1",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 2,
    "title": "Post title 2",
    "body": "Post body 2",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  },
  {
    "id": 3,
    "title": "Post title 3",
    "body": "Post body 3",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  },
  {
    "id": 4,
    "title": "Post title 4",
    "body": "Post body 4",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "Adam Smith",
      "id": 3,
      "profile": {
        "firstname": "Adam",
        "lastname": "Smith",
        "user_id": 3
      }
    }
  },
  {
    "id": 5,
    "title": "Post title 5",
    "body": "Post body 5",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 6,
    "title": "Post title 6",
    "body": "Post body 6",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 7,
    "title": "Post title 7",
    "body": "Post body 7",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  }
]
```

> يجب عند إختيار إعمدة محددة فى العلاقات أن تختار ***id*** وأنت تختار علاقة ***user*** لأن بدون إختيارك له لن يعود المستخدم مع المنشور. 

2. نقوم بالذهاب الى المسار التالى ***`resources/views/users`*** ونقوم بتعديل هذا الملف ***`list.blade.php`*** لعرض المستخدمين وملفات التعريف الخاصة بهم وأيضا ما يهمنا هنا هو عرض المنشورات الخاصة لكل مستخدم.

```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
            <th style="text-align: center">Posts</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($users as $user)
            <tr>
                <td>{{ $user->username }}</td>
                <td>{{ $user->profile->firstname }}</td>
                <td>{{ $user->profile->lastname }}</td>
                <td>
                    <ul>
                        @foreach ($user->posts as $post)
                            <li>{{ $post->title }}</li>
                        @endforeach
                    </ul>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}



#### عن طريق إستخدام ***`API Resources`***.

1. تذهب الى المسار التالى ***`App/Http/Resources`*** ونبدأ بالتعديل على ملف ***`PostResource.php`***:
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'body' => $this->body,
            'user' => UserResource::make($this->whenLoaded('user')),
        ];
    }
}
```

2. نقوم بالتوجه أولا لملف ***`routes/web.php`***  لنضيف رابط جديد.
```PHP
Route::get('/profiles-resource', function () {
    $profiles = \App\Models\Profile::with(['user'])->get();
    $profilesResource = \App\Http\Resources\ProfileResource::collection($profiles);
    return response()->json($profilesResource);
});
```

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/profiles-resource`*** لنرى ما هى النتائج التى ستظهر.
```json
[
  {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe",
    "user": {
      "id": 1,
      "username": "John Doe"
    }
  },
  {
    "id": 2,
    "firstname": "Tom",
    "lastname": "Cruz",
    "user": {
      "id": 2,
      "username": "Tom Cruz"
    }
  },
  {
    "id": 3,
    "firstname": "Adam",
    "lastname": "Smith",
    "user": {
      "id": 3,
      "username": "Adam Smith"
    }
  }
]
```

> ونرى هنا أيضا أننا نجلب البيانات المطلوبة فقط.


## How to update data in one-to-many relationship in Laravel?
