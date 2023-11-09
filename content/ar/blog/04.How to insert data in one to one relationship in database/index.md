---
title: "كيفية إدراج البيانات في علاقة واحد لواحد في قاعدة البيانات؟"
date: 2023-08-12
draft: false
slug: "how-to-insert-data-in-one-to-one-relationship-in-database-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 3
---

![كيفية إدراج البيانات في علاقة واحد لواحد في قاعدة البيانات؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-insert-data-in-one-to-one-relationship-in-database.png "كيفية إدراج البيانات في علاقة واحد لواحد في قاعدة البيانات؟")

بعد أن أنشأنا علاقة ***واحد لواحد*** بين جدول ***المستخدمين*** وجدول ***ملفات تعريف*** ، وأضفنا ***`hasOne()`*** method إلى نموذج ***User*** ، و method ***belongsTo()*** إلى نموذج ***Profile*** ، حان الوقت لمعرفة كيفية حفظ البيانات في قاعدة البيانات عندما نستخدم هذه العلاقة. وما هي الأساليب المتبعة في ذلك؟

تنقسم هذه الطرق إلى ثلاث طرق رئيسية:
1. بدون استخدام **method profile**.
2. باستخدام **method profile**.
3. باستخدام العلاقة العكسية **method user**.

تعتمد أفضل طريقة للاستخدام على الاحتياجات المحددة للتطبيق الخاص بك.
إذا كنت تحتاج فقط إلى حفظ ملف التعريف المرتبط بالمستخدم ، فإن الطريقة الأولى هي الخيار الأبسط.
إذا كنت بحاجة إلى الحصول على ملف التعريف أو تحديثه أو حذفه ، فإن الطريقة الثانية هي خيار أفضل.
إذا كنت بحاجة إلى الحصول على المستخدم أو تحديثه أو حذفه ، فإن الطريقة الثالثة هي الخيار الأفضل.
### 1. بدون استخدام **method profile**.
* نذهب أولاً إلى ملف ***`routes/web.php`*** ونضيف مسارًا جديدًا حتى نتمكن من اختبار هذه الطريقة.
```PHP
use App\Models\Profile;
use App\Models\User;
---
Route::get('/one-to-one', method () {
    $user = User::create(['username' => 'John Doe']);
    Profile::create([
        'user_id' => $user->id,
        'firstname' => 'John',
        'lastname' => 'Doe',
        'birthday' => '08-11-1991'
    ]);
    return response()->json([
        'username' => $user->username,
        'firstname' => $user->profile->firstname,
        'lastname' => $user->profile->lastname,
    ]);
});
```

* نقوم بفتح المتصفح وانانتقال إلى هذا الرابط ***`http://127.0.0.1:8000/one-to-one`***. لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```

### 2. باستخدام **method profile**.
* نذهب أولاً إلى ملف ***`routes/web.php`*** ونقوم بتعديل هذا المسار.
```PHP
Route::get('/one-to-one', method () {
    $user = User::create(['username' => 'Tom Cruz']);
    $user->profile()->create([
      'firstname' => 'Tom',
      'lastname' => 'Cruz',
      'birthday' => '01-02-2000'
    ]);
    return response()->json([
        'username' => $user->username,
        'firstname' => $user->profile->firstname,
        'lastname' => $user->profile->lastname,
    ]);
});
```

* نقوم بفتح المتصفح وانانتقال إلى هذا الرابط ***`http://127.0.0.1:8000/one-to-one`***. لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
```

### 3. باستخدام العلاقة العكسية **method user**.
* نذهب أولاً إلى ملف ***`routes/web.php`*** ونقوم بتعديل هذا المسار.
```PHP
Route::get('/one-to-one', method () {
    $user = User::create(['username' => 'Adam Smith']);
    $profile = new Profile([
        'firstname' => 'Adam',
        'lastname' => 'Smith',
        'birthday' => '01-01-1999'
    ]);

    $profile->user()->associate($user)->save();
    return response()->json([
        'username' => $profile->user->username,
        'firstname' => $profile->firstname,
        'lastname' => $profile->lastname,
    ]);
});
```

* نقوم بفتح المتصفح وانانتقال إلى هذا الرابط ***`http://127.0.0.1:8000/one-to-one`***. لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

- يمكنك العثور على repo لهذه السلسلة على github هنا:
---
{{< github repo="laravelspa/laravel-relations" >}}
