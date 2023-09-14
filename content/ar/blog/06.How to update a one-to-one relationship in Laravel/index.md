---
title: "كيفية تحديث علاقة one-to-one في Laravel؟"
date: 2023-08-14
draft: false
slug: "how-to-update-a-one-to-one-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 5
---
![كيفية تحديث علاقة one-to-one في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-update-one-to-one-relationship-in-laravel.png "كيفية تحديث علاقة one-to-one في Laravel؟")
### تحديث البيانات باستخدام نموذج المستخدم.

1. باستخدام ***`push method`***.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/users/update', method () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe Updated';
    $user->profile->lastname = 'Doe Updated';
    $user->push();
    return response()->json($user);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المستخدم والملف الشخصي بنجاح.
```json
{
  "id": 1,
  "username": "John Doe Updated",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T04:44:19.000000Z",
  "profile": {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe Updated",
    "birthday": "08-11-1991",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-10T04:44:19.000000Z"
  }
}
```

2. بإستخدام ***`update method`***.
* نذهب أولاً إلى الملف ***`routes/web.php`*** ونقوم بتعديل هذا المسار.
```PHP
Route::get('/users/update', method () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe';
    $user->save();
    $user->profile->update([
        'lastname' => 'Doe'
    ]);
    return response()->json($user);
]);
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المستخدم والملف الشخصي بنجاح.
```json
{
  "id": 1,
  "username": "John Doe",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T04:46:11.000000Z",
  "profile": {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe",
    "birthday": "08-11-1991",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-10T04:46:11.000000Z"
  }
}
```

### تحديث البيانات باستخدام نموذج الملف الشخصي.
1. باستخدام ***`push method`***.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/profiles/update', method () {
    $profile = Profile::with('user')->find(1);
    $profile->firstname = 'John Updated';
    $profile->lastname = 'Doe Updated';
    $profile->user->username = 'John Doe Updated';
    $profile->push();
    return response()->json($profile);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/profiles/update`*** لنجد أنه تم تحديث المستخدم والملف الشخصي بنجاح.
```json
{
  "id": 1,
  "firstname": "John Updated",
  "lastname": "Doe Updated",
  "birthday": "08-11-1991",
  "user_id": 1,
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:02:31.000000Z",
  "user": {
    "id": 1,
    "username": "John Doe Updated",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-10T05:02:31.000000Z"
  }
}
```

2. بإستخدام ***`update method`***.
* نذهب أولاً إلى الملف ***`routes/web.php`*** ونقوم بتعديل هذا المسار.
```PHP
Route::get('/profiles/update', method () {
    $profile = Profile::with('user')->find(1);
    $profile->firstname = 'John';
    $profile->lastname = 'Doe';
    $profile->save();
    $profile->user->update([
        'username' => 'Joun Doe'
    ]);
    return response()->json($profile);
]);
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/profiles/update`*** لنجد أنه تم تحديث المستخدم والملف الشخصي بنجاح.
```json
{
  "id": 1,
  "firstname": "John",
  "lastname": "Doe",
  "birthday": "08-11-1991",
  "user_id": 1,
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:07:38.000000Z",
  "user": {
    "id": 1,
    "username": "Joun Doe",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-10T05:07:38.000000Z"
  }
}
```

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}
