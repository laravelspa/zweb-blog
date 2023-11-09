---
title: "كيفية إدراج البيانات في علاقة واحد الى كثير في قاعدة البيانات؟"
date: 2023-08-17
draft: false
slug: "how-to-insert-data-in-one-to-many-relationship-in-database-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 8
---
## كيفية إدراج البيانات في علاقة واحد الى كثير في قاعدة البيانات؟
![كيفية إدراج البيانات في علاقة واحد لكثير في قاعدة البيانات؟](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/ar/how-to-insert-data-in-one-to-many-relationship-in-database.png "كيفية إدراج البيانات في علاقة واحد لكثير في قاعدة البيانات؟")
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
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
    $user = User::find(1);
    Post::create([
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
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
   $user = User::find(2);
    Post::insert(
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
use App\Models\User;
---
Route::get('/one-to-many', function () {
    $user = User::find(3);
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
use App\Models\User;
---
Route::get('/one-to-many', function () {
   $user = User::find(1);
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
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
    $user = User::find(2);
    $post = new Post([
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

- يمكنك العثور على repo لهذه السلسلة على github هنا:
---
{{< github repo="laravelspa/laravel-relations" >}}