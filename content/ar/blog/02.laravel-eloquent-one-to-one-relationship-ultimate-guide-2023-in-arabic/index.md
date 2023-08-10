---
title: "Laravel Eloquent One-To-One Relationship - Ultimate Guide 2023 In Arabic"
date: 2023-08-10
draft: false
slug: "laravel-eloquent-one-to-one-relationship-ultimate-guide-2023-in-arabic"
description: "نظرًا لكونها أول علاقة أساسية وأبسطها يقدمها Laravel , فإنهم يربطون جدولين بطريقة بحيث يرتبط صف واحد من الجدول الأول بصف واحد فقط من الجدول الآخر."
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel Eloquent Relationships']
series_order: 1
---

غالبًا ما يحتاج المطور أن يتفاعل مع قواعد البيانات. وإذا كنت تستخدم إطار عمل __Laravel__ فيجب علبك أن تتعرف على أهم مميزات __لارافيل__ وهو ما يسمى __Eloquent__ , مخطط علاقات الكائنات (ORM) الذى يجعل هذه العملية بسيطة وسهلة.

يعد __Laravel Eloquent__ أحد الميزات الرئيسية في إطار عمل __لارافل__. ويرجع ذلك إلى دعمه الرائع فى تحديد وإنشاء وإدارة العلاقات بين جداول البيانات المختلفة.
 سأوضح لك في هذه السلسلة من المقالات كيفية إنشاء واستخدام علاقات __Eloquent__. 
 
 مع ملاحظة أنه يمكنك البدء دون أي معرفة سابقة بالعلاقات.

{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023.png"
alt="laravel one to one relationship"
caption="laravel one to one relationship"
>}}

من الضروري كمبرمج محترف أن تدرك أنواع العلاقات 
ولكن قبل هذا يجب أن تسأل نفسك سؤالاً هاماً ما هى العلاقات من الأساس؟

## ما هي العلاقات فى قواعد البيانات؟
عند العمل مع جداول في قاعدة بيانات يوجد بينها علاقات , يمكننا وصف هذه العلاقات على أنها اتصالات بين تلك الجداول. يساعدك هذا في تنظيم البيانات وهيكلتها دون عناء مما يتيح إمكانية قراءة ومعالجة للبيانات بشكل أسرع.

## ما هي أنواع العلاقات الموجودة فى Laravel؟
 هناك ثلاثة أنواع رئيسية من العلاقات قى قواعد البيانات تظهر عند الممارسة:
 * __one-to-one__: وهو ارتباط سجل واحد فقط بسجل أخر. ومثال على ذلك أن كل مستخدم يمتلك ملف تعريف واحد خاص به.
 * __one-to-many__: وهو ارتباط سجل واحد فقط بأكثر من سجل أخر ومثال على ذلك أن كل كاتب يمتلك أكثر من مقال.
 * __many-to-many__: وهو ارتباط أكثر من سجل بأكثر من سجل أخر ومثال على ذلك أنه يمكن لأكثر من مستخدم الانضمام إلى أكثر من دورة واحدة.

إلى جانب هذه العلاقات , تقدم __Laravel__ المزيد من العلاقات , وهي:
* __Has Many Through__
* __Polymorphic Relations__
* __Many-to-many Polymorphic__

ليصبح عدد العلاقات التى سنقوم بشرحها هم 6 أنواع.
وسنقوم ببناء نظام إدارة محتوى بسيط لشرح جميع تلك العلاقات.

هل أحتاج إلى معرفة خاصة بـ __Eloquent__ قبل قراءة هذا؟

في الأمثلة أدناه, حاولت أن أشرح كل شيء بأكبر قدر ممكن من الوضوح, دون استخدام الكثير من وظائف __Eloquent__ الصعبة والتقنيات المعقدة. هذا يعني أن المعرفة السابقة ليست ضرورية تمامًاً.

ومع ذلك من الأفضل دائمًاً تعلم الأساسيات أولاً ثم متابعة موضوعات أكثر تعقيدًاً مثل العلاقات.

## كيف تنشئ علاقة One-To-One فى لارافل؟
نظرًا لكونها أول علاقة أساسية وأبسطها تقدمها __Laravel__ , فإنه يتم ربط جدولين بحيث يرتبط صف واحد من الجدول الأول بصف واحد فقط من الجدول الآخر, أو بنفس الجدول.

لرؤية هذا كتطبيق عملى سنبدأ بإنشاء نظام إدارة المحتوى,

لنفترض أن كل مستخدم له ملف تعريف واحد خاص به. في بعض الحالات , يمكنك تخزين جميع معلومات الملف الشخصي في جدول المستخدمين.
 لكن هذا لن يكون مثاليًا.
  
في المثال الخاص بنا نريد إنشاء جدول منفصل خاص  بملفات التعريف الشخصية ,إذا أردنا لاحقًا نقل ملف تعريف من مستخدم إلى أخر فسيكون هذا متاحاً.

> افتراضيا جدول المستخدمين موجود ولا يهم الأعمدة التى سيحتويها.

* لنقل أن لدينا جدول للمستخدمين يتكون من الأعمدة التالية:
```PHP
Schema::create('users', function (Blueprint $table) {
  $table->id();
  $table->string('username');
  $table->string('email')->unique();
  $table->timestamps();
});
```

* نقوم بتعديل ملف ***`User.php`***.
```PHP
protected $fillable = ['username'];
```

* نقوم بإنشاء ***`Profile Model`*** مع الجدول الخاص به.
```bash
php artisan make:model Profile -m
```

فى علاقة ***`One-to-One`*** لدينا الحرية فى اختيار احد هاتين الطريقتين:
* إضافة ***`user_id`*** فى جدول ***`profiles`***.
* إضافة ***`profile_id`*** فى جدول ***`users`***.

> وفي الغالب يتم دائمًا إضافة هذا العمود الذي يربط بين الجدولين إلى الجدول الثاني , لذلك سنضيفه إلى جدول الملفات الشخصية على النحو التالي.
```PHP {hl_lines=["6"]} 
Schema::create('profiles', function (Blueprint $table) {
  $table->id();
  $table->string('firstname');
  $table->string('lastname');
  $table->string('birthday');
  $table->foreignId('user_id')->constrained();
  $table->timestamps();
});
```

* نقوم بتعديل ملف ***`Profile.php`***.
```PHP
protected $fillable = [
  'user_id',
  'firstname',
  'lastname',
  'birthday'
];
```

* لنقم بتنفيذ هذا الأمر لتحديث قاعدة البيانات.
```bash
php artisan migrate
```

* لنتوجه الى ملف ***`User.php`*** لنقم بتعيين العلاقة.
```PHP
public function profile() {
    return $this->hasOne(Profile::class);
}
```

> لنتعرف كيف تعمل ***`hasOne`***
```PHP
$this->hasOne(Profile::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* لنتوجه الى ملف ***`Profile.php`*** لنقم بتعيين العلاقة العكسية.
```PHP
public function user() {
    return $this->belongsTo(User::class);
}
```

> لنتعرف كيف تعمل ***`belongsTo`***.
```PHP
$this->belongsTo(User::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // OwnerKey By Default Id
);
```

> لنقل أنك تريد تسمية العلاقة بإسم أخر مثل ***`admin`*** فيجب علينا إضافة ***`foreignKey`***.
```PHP
public function admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```

> وإذا لم تقم بإضافة ***`foreignKey`*** عند تغيير أسم العلاقة سيظهر لك هذا الخطأ.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/Attempt-to-read-property-X-on-null.png"
alt="Attempt to read property X on null"
caption="Attempt to read property X on null"
>}}

## كيفية حفظ البيانات فى علاقة one to one قى Laravel؟
بعد أن قمنا بإنشاء علاقة One-To-One بين كلا من جدول المستخدمين وجدول ملفات التعريف وأضفنا ***`hasOne`*** داخل ***`User Model`***, وأيضا قمنا بإضافة العلاقة العكسية داخل ***`Profile Model`*** بإضافة ***`belongsTo`*** إليه.

 جاء الوقت لمعرفة كيف يتم حفظ البيانات فى قاعدة البيانات أثناء إستخدامنا لهذه العلاقة.
وما هى الطرق المستخدمة فى ذلك

تنقسم هذه الطرق الى ثلاث طرق اساسية:
1. بدون إستخدام **function profile**.
2. عن طريق إستخدام **function profile**.
3. عن طريق إستخدام العلاقة العكسية **function user**.

### 1. بدون إستخدام **function profile**.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نضيف رابط جديد حتى نستطيع أن نختبر هذه الطرق.
```PHP
use App\Models\Profile;
use App\Models\User;
---
Route::get('/one-to-one', function () {
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
* نقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```

### 2. بإستخدام ***`function profile`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-one', function () {
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

* نقوم بفتح المتصفح والذهاب الى هذا الرابط مرة أخرى ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
```

### 3. عن طريق إستخدام العلاقة العكسية ***`function user`*** مع ***`associate`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/one-to-one', function () {
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

* نقوم بفتح المتصفح والذهاب الى هذا الرابط مرة أخرى ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

## كيف تحصل على البيانات في علاقة One-To-One في Laravel؟
رأينا كيف يتم حفظ البيانات بطرق مختلفة ومتعددة داخل قاعدة البيانات بإستخدام علاقة One-To-One فى Laravel.

ولكن كيف يتم الحصول على البيانات من قاعدة البيانات؟
هناك عدة طرق يمكن من خلالها الحصول على البيانات من قاعدة البيانات.

وتنقسم هذه الطرق الى طريقتين أساسيتين:

### أثناء الحصول على  بيانات المستخدمين.
أثناء الحصول على بيانات المستخدمين سنقوم بالحصول على ملفات التعريف الشخصية الخاصة بهم.
بعد ذلك ينقسم الأمر فى عرض تلك البيانات بين شكلين لا ثالث لهم.
يحدد نوع التطبيق الذى تعمل عليه الشكل المستخدم:
* أولا: تطبيق يعتمد على web routes.
* ثانياً: تطبيق يعتمد على api routes.


#### أولاً: الإعتماد على ***`Web Routes`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  ونقوم بالتعديلات التالية.
```PHP
Route::get('/users', function () {
    $users = User::with(['profile'])->get();
    return view('users.list', compact('users'));
});
```
> إذا قمنا بفحص Response (الأستجابة) الخاصة بهذا الأمر سنجد أن جميع البيانات تم الحصول عليها من قاعدة البيانات.
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
    }
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
    }
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
    }
  }
]
```

 هنا يمكننا اختيار الاعمده التى نحتاجها فقط بالشكل التالي.
```PHP
Route::get('/users', function () {
    $users = User::with(['profile:firstname,lastname,user_id'])->get();
    return view('users.list', compact('users'));
});
```

> أذا قمنا الأن بالفحص مرة أخرى سنجد أن حجم البيانات تم تقليصه.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "firstname": "John",
      "lastname": "Doe",
      "user_id": 1
    }
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "firstname": "Tom",
      "lastname": "Cruz",
      "user_id": 2
    }
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "profile": {
      "firstname": "Adam",
      "lastname": "Smith",
      "user_id": 3
    }
  }
]
```

> وهنا نرى الفرق بين الحالتين فى حجم البيانات التى تمت معالجتها ويزداد الأمر إذا كانت البيانات حجمها أكبر من ذلك بكثير.
> يجب عند إختيار إعمدة محددة من العلاقات أن تختار عمود ***foreignKey*** لأن بدون إختيارك له لن تعود البيانات بشكل صحيح من قاعدة البيانات.

2. داخل فولدر ***`views`*** نقوم بإضافة فولدر أخر أسمه ***`users`*** وبداخله نقوم بإضافة ملف ***`list.blade.php`*** ونقوم بإضافة هذا الجدول البسيط لعرض المستخدمين بداخله.
```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($users as $user)
            <tr>
                <td>{{ $user->username }}</td>
                <td>{{ $user->profile->firstname }}</td>
                <td>{{ $user->profile->lastname }}</td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. قم بفتح المتصفح والذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png"
alt="Laravel One To One Relationship - Users Table"
caption="Users Table"
>}}

#### ثانياً: الإعتماد على ***`Api Routes`***.

> ما هو API Resources?

ببساطة هى طبقة وسيطة بين __Eloquent__ وما بين الإستجابة الخاصة ب API وتحويل تلك البيانات التى تم الحصول علىها من قاعدة البيانات الى JSON مع امكانية تحديد بيانات محددة دون غيرها أو التلاعب تلك البيانات.

1. سنقوم بإنشاء API Resource لكل من User, Profile قم تنفيذ هذا الأمر فى موجه الأوامر.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
```
2. قم بالذهاب الى المسار التالى ***`App/Http/Resources`*** والتعديل على كلا من:

* ملف ***`ProfileResource.php`***.
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

* ملف ***`UserResource.php`***.
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
        ];
    }
}
```

3. قم بالتوجه ألى ملف ***`routes/api.php`***  وإضافة رابط جديد.
```PHP
use App\Models\User;
use App\Http\Resources\UserResource;
---
Route::get('/users', function () {
    $users = User::with(['profile'])->get();
    $usersResource = UserResource::collection($users);
    return response()->json($usersResource);
});
```

4. قم بفتح المتصفح والذهاب الى الرابط التالى ***`http://127.0.0.1:8000/api/users`*** لنرى ما هى النتائج التى ستظهر.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "profile": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe"
    }
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "profile": {
      "id": 2,
      "firstname": "Tom",
      "lastname": "Cruz"
    }
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "profile": {
      "id": 3,
      "firstname": "Adam",
      "lastname": "Smith"
    }
  }
]
```

> ونرى هنا أيضا أننا قمنا نالحصول على البيانات المطلوبة والمحددة داهل ملفات API Resources فقط.

### أثناء الحصول على بيانات ملفات التعريف الشخصية.
أثناء الحصول على بيانات الملفات الشخصية سنقوم بالحصول على كل مستخدم مرتبط بتلك الملفات.
بعد ذلك ينقسم الأمر فى عرض تلك البيانات بين شكلين لا ثالث لهم.
يحدد نوع التطبيق الذى تعمل عليه الشكل المستخدم:
* أولا: تطبيق يعتمد على web routes.
* ثانياً: تطبيق يعتمد على api routes.


#### أولاً: الإعتماد على ***`Web Routes`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  ونقوم بالتعديلات التالية.
```PHP
Route::get('/profiles', function () {
    $profiles = Profile::with('user')->get();
    return view('profiles.list', compact('profiles'));
});
```

> إذا قمنا بفحص Response (الأستجابة) الخاصة بهذا الأمر سنجد أن جميع البيانات تم الحصول عليها من قاعدة البيانات.
```json
[
  {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe",
    "birthday": "08-11-1991",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 2,
    "firstname": "Tom",
    "lastname": "Cruz",
    "birthday": "01-02-2000",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 3,
    "firstname": "Adam",
    "lastname": "Smith",
    "birthday": "01-01-1999",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "id": 3,
      "username": "Adam Smith",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  }
]
```

 هنا يمكننا اختيار الاعمده التى نحتاجها فقط بالشكل التالي.
```PHP
Route::get('/profiles', function () {
    $profiles = Profile::with('user:username,id')->get();
    return view('profiles.list', compact('profiles'));
});
```

> أذا قمنا الأن بالفحص مرة أخرى سنجد أن حجم البيانات تم تقليصه.
```json
[
  {
    "id": 1,
    "firstname": "John",
    "lastname": "Doe",
    "birthday": "08-11-1991",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1
    }
  },
  {
    "id": 2,
    "firstname": "Tom",
    "lastname": "Cruz",
    "birthday": "01-02-2000",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "username": "Tom Cruz",
      "id": 2
    }
  },
  {
    "id": 3,
    "firstname": "Adam",
    "lastname": "Smith",
    "birthday": "01-01-1999",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:03.000000Z",
    "updated_at": "2023-08-07T06:23:03.000000Z",
    "user": {
      "username": "Adam Smith",
      "id": 3
    }
  }
]
```

> يجب عند إختيار إعمدة محددة فى العلاقات أن تختار ***id*** وأنت تختار علاقة ***user*** لأن بدون إختيارك له لن يعود المستخدم مع ملف التعريف.

2. داخل فولدر ***`views`*** نقوم بإضافة فولدر أخر أسمه ***`profiles`*** وبداخله نقوم بإضافة ملف ***`list.blade.php`*** ونقوم بإضافة هذا الجدول البسيط لعرض الملفات الضخصية بداخله.
```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($profiles as $profile)
            <tr>
                <td>{{ $profile->user->username }}</td>
                <td>{{ $profile->firstname }}</td>
                <td>{{ $profile->lastname }}</td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. قم بفتح المتصفح والذهاب الى الرابط التالى ***`http://127.0.0.1:8000/profiles`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png"
alt="Laravel One To One Relationship - Profiles Table"
caption="Profiles Table"
>}}


#### ثانياً: الإعتماد على ***`Api Routes`***.

1. قم بالذهاب الى المسار التالى ***`App/Http/Resources`*** والتعديل على ملف ***`ProfileResource.php`***:
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
            'title' => $this->title,
            'body' => $this->body,
            'user' => UserResource::make($this->whenLoaded('user')),
        ];
    }
}
```

2. قم بالتوجه ألى ملف ***`routes/api.php`***  وإضافة رابط جديد.
```PHP
use App\Models\Profile;
use App\Http\Resources\ProfileResource;
---
Route::get('/profiles-resource', function () {
    $profiles = Profile::with(['user'])->get();
    $profilesResource = ProfileResource::collection($profiles);
    return response()->json($profilesResource);
});
```

3. قم بفتح المتصفح والذهاب الى الرابط التالى ***`http://127.0.0.1:8000/api/profiles`*** لنرى ما هى النتائج التى ستظهر.
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

## كيفية تحسين استعلامات Eloquent في Laravel؟
عند التعامل مع قاعدة بيانات كبيرة بداخلها الكثير من البيانات هنا لتنظر للأمر بنظرة مختلفة.

فالأمر ليس مجرد الحصول على البيانات ولكن كم من الوقت سيستغرق للحصول على تلك البيانات.
وكم عدد الإستعلامات التى ستنفذ فى كل صفحة.

ولإختبار هذا الأمر سنقوم بتنزيل مكتبة مشهورة جدا اسمها 
[Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). ستساعدنا فى معرفة جميع الإستعلامات التى تنفذ فى كل صفحة.

* لنقم بتنفيذ هذا الأمر.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> وتأكد أن ***`APP_DEBUG=true`*** داخل ملف ***`.env`***.

* الفرق بين الحالتين:
```PHP
$users = User::all();
$users = User::with('profile')->get();
```

* نقوم بفتح المتصفح والذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر فى الشريط الخاص بالمكتبة.

1. __(Lazy Loading)__ - الحصول على البيانات بدون إستخدام ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png"
alt="laravel debugbar lazy loading"
caption="laravel debugbar lazy loading"
>}}

 سنجد الأن أسفل الصفحة شريط يخص مكتبة ***`Laravel Debugbar`*** عند فتحه سنجد أنه يعملنا بكثير من البيانات.
الأمر الذى يهمنا هنا هو عدد الإستعلامات الموجوده فى هذه الصفحة وكما هو موضح فهم __4__ إستعلامات.

ونحن نفوم بجلب 3 مستخدمين فقط, تخيل معى لو داخل قاعدة البيانات هذه  عشران الألاف او ملايين المستخدمين سيكون وقت تحميل هذه الصفحة بظئ جدا بسبب عدد الإستعلامات الضخم.

 وهذا ما يسمى بمشكلة N+1 فى Laravel أو ما يطلق عليه Lazy Loading.

> فتخيل مع 1000 مستخدم سيتم طلب 1001 إستعلام لقاعدة البيانات فى هذه الصفحة فقط. وهذا يعتبر إستهلاك لموارد السيرفر. وأيضا دليل على عدم إحترافية الكود الخاص بك.

2. __(Eager Loading)__ - الحصول على البيانات بدون إستخدام ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png"
alt="laravel debugbar eager loading"
caption="laravel debugbar eager loading"
>}}

وكما ترى بإضافة كلمة ***`with`*** مع أسم العلاقة سيتم جلب بيانات كل مستخدم مع الملف الشخصى الخاص به بدون مشكلة N+1.

فتم تقليص عدد الإستعلامات من 4 الى 2 فقط وهذا الأمر سترى تأثيره بشكل واضح إذا كانت قاعدة البيانات هذه كما قلنا متوسطة الحجم او كبيرة وهذا ما يطلق عليه Eager Loading.

## كيفية تحديث علاقة one to one قى Laravel؟
### تحديث البيانات من ناحية المستخدم.

1. إستخدام ***`push function`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بإضافة هذا الرابط.
```PHP
Route::get('/users/update', function () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe Updated';
    $user->profile->lastname = 'Doe Updated';
    $user->push();
    return response()->json($user);
});
```
* نقوم بفتح المتصفح والذهاب الى هذا الرابط الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المستخدم وملف التعريف بنجاح.
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

2. إستخدام ***`update function`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/users/update', function () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe';
    $user->save();
    $user->profile->update([
        'lastname' => 'Doe'
    ]);
    return response()->json($user);
]);
```
* نقوم بفتح المتصفح والذهاب الى هذا الرابط الجديد ***`http://127.0.0.1:8000/users/update`*** لنجد أنه تم تحديث المستخدم وملف التعريف بنجاح.
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

### تحديث البيانات من ناحية ملف التعريف الشخصى.
1. إستخدام ***`push function`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بإضافة هذا الرابط.
```PHP
Route::get('/profiles/update', function () {
    $profile = Profile::with('user')->find(1);
    $profile->firstname = 'John Updated';
    $profile->lastname = 'Doe Updated';
    $profile->user->username = 'John Doe Updated';
    $profile->push();
    return response()->json($profile);
});
```
* نقوم بفتح المتصفح والذهاب الى هذا الرابط الجديد ***`http://127.0.0.1:8000/profiles/update`*** لنجد أنه تم تحديث المستخدم وملف التعريف بنجاح.
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

2. إستخدام ***`update function`***.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بالتعديل على هذا الرابط.
```PHP
Route::get('/profiles/update', function () {
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

* نقوم بفتح المتصفح والذهاب الى هذا الرابط الجديد ***`http://127.0.0.1:8000/profiles/update`*** لنجد أنه تم تحديث المستخدم وملف التعريف بنجاح.
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


## كيفية حذف البيانات من علاقة one to one قى Laravel؟
### حذف البيانات من ناحية المستخدم.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بإضافة هذا الرابط.
```PHP
Route::get('/users/profile/delete', function () {
    $user = User::with('profile')->find(1);
    $user->profile()->delete();
    return response()->json($user);
});
```
* سنقوم بفتح المتصفح والذهاب الى هذا الرابط الجديد ***`http://127.0.0.1:8000/users/profile/delete`*** لنجد أنه تم حذف ملف التعريف الخاص بالمستخدم بنجاح.
```json
{
  "id": 1,
  "username": "Joun Doe",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:07:38.000000Z",
  "profile": null
}
```
> قم بعمل تحديث للصفحة مرتان لتظهر لك أنه تم حذف ملف التعريف الخاص بهذا المستخدم

### حذف البيانات من ناحية ملف التعريف الشخصى.
* نقوم بالتوجه أولا لملف ***`routes/web.php`*** و نقوم بإضافة هذا الرابط.
```PHP
Route::get('/profiles/user/delete', function () {
    $profile = Profile::with('user')->findOrFail(2);
    $profile->delete();
    $profile->user()->delete();
});
```
* سنقوم بفتح المتصفح والذهاب الى هذا الرابط ***`http://127.0.0.1:8000/profiles/user/delete`*** لنجد أنه تم حذف كلاً من المستخدم وملف التعريف بنجاح.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png"
alt="Rcord has deleted"
caption="Rcord has deleted"
>}}

## الخاتمة
هذه المقالة هى بداية لسلسلة كاملة عن ***Laravel Eloquent Relationships*** - العلاقات داخل ***Laravel***.
وقد تناولنا فيها علاقة ***One TO One*** بطريقة كاملة.
 ولم نبخل عليكم بأى معلوم, وإن شاء الله فى الشرح القادم سنتعرف على علاقة ***One To Many***. 
 وفى الختام نصلى على رسول الله سيدنا محمد وعلى اُله وصحبه أجمعين.

- ستجد repo الخاصة بهذه السلسلة على github هنا
{{< github repo="laravelspa/laravel-relations" >}}
