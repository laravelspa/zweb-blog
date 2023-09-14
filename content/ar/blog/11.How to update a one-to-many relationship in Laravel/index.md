---
title: "كيفية تحديث علاقة one to many فى Laravel؟"
date: 2023-08-19
draft: false
slug: "how-to-update-a-one-to-many-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 10
---
![كيفية تحديث علاقة one-to-many في Laravel؟](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/ar/how-to-update-one-to-many-relationship-in-laravel.png "كيفية تحديث علاقة one-to-many في Laravel؟")
### تحديث البيانات باستخدام نموذج المستخدم.

1. باستخدام ***`push method`***.
* نذهب أولاً إلى ملف ***`routes/web.php`*** ونقوم بتعديل هذا الرابط:
```PHP
Route::get('/users/update', method () {
    $user = User::with('posts')->find(1);
    $post = $user->posts()->whereId(1)->first();
    $post->title = 'Post title 1 updated';
    $post->push();
    return response()->json($user);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المنشور بنجاح.
```json
{
  "id": 1,
  "username": "John Doe",
  "created_at": "2023-09-06T17:24:02.000000Z",
  "updated_at": "2023-09-06T17:25:58.000000Z",
  "posts": [
    {
      "id": 1,
      "title": "Post title 1 updated",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-09-06T17:28:58.000000Z",
      "updated_at": "2023-09-06T18:37:30.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    }
  ]
}
```

2. بإستخدام ***`update method`***.
* نذهب أولاً إلى الملف ***`routes/web.php`*** ونقوم بتعديل هذا الرابط.
```PHP
Route::get('/users/update', method () {
    $user = User::with('posts')->find(1);
    $post = $user->posts()->whereId(1)->first();
    $post->title = 'Post title 1';
    $post->update();
    return response()->json($user);
]);
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المنشور بنجاح.
```json
{
  "id": 1,
  "username": "John Doe",
  "created_at": "2023-09-06T17:24:02.000000Z",
  "updated_at": "2023-09-06T17:25:58.000000Z",
  "posts": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-09-06T17:28:58.000000Z",
      "updated_at": "2023-09-06T18:41:35.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    }
  ]
}
```

### تحديث البيانات باستخدام نموذج المنشور.
1. باستخدام ***`push method`***.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا الرابط:
```PHP
Route::get('/posts/update', method () {
    $post = Post::with('user')->find(1);
    $post->title = 'Post title 1 updated';
    $post->user->username = 'John Doe Updated';
    $post->push();
    return response()->json($post);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/posts/update`*** لنجد أنه تم تحديث المنشور بنجاح.
```json
{
  "id": 1,
  "title": "Post title 1 updated",
  "body": "Post body 1",
  "user_id": 1,
  "created_at": "2023-09-06T17:28:58.000000Z",
  "updated_at": "2023-09-06T18:50:30.000000Z",
  "user": {
    "id": 1,
    "username": "John Doe Updated",
    "created_at": "2023-09-06T17:24:02.000000Z",
    "updated_at": "2023-09-06T18:49:54.000000Z"
  }
}
```

2. بإستخدام ***`update method`***.
* نذهب أولاً إلى الملف ***`routes/web.php`*** ونقوم بتعديل هذا الرابط.
```PHP
Route::get('/posts/update', method () {
    $post = Post::with('user')->find(1);
    $post->user->username = 'John Doe';
    $post->update([
        'title' => 'Post title 1'
    ]);
    return response()->json($post);
]);
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/posts/update`*** لنجد أنه تم تحديث المنشور بنجاح.
```json
{
  "id": 1,
  "title": "Post title 1",
  "body": "Post body 1",
  "user_id": 1,
  "created_at": "2023-09-06T17:28:58.000000Z",
  "updated_at": "2023-09-06T18:55:45.000000Z",
  "user": {
    "id": 1,
    "username": "John Doe",
    "created_at": "2023-09-06T17:24:02.000000Z",
    "updated_at": "2023-09-06T18:49:54.000000Z"
  }
}
```

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}
