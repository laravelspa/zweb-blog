---
title: "Laravel 10 | One-To-One Relationship In Arabic"
date: 2023-07-30
draft: false
slug: "laravel10-one-to-one-relationship-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel 10 Eloquent Relations']
---

{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/laravel10-one-to-one-relationship.png"
alt="laravel10 one to one relationship"
caption="laravel10 one to one relationship"
>}}

## مقدمة
غالبًا ما يحتاج المطور أن يتفاعل مع قواعد البيانات. وإذا كنت تستخدم __Laravel__ فستتعرف على أهم مميزات إطار العمل هذا وهو ما يسمى __Eloquent__ ، مخطط علاقات الكائنات (ORM) الذى يجعل هذه العملية بسيطة وطبيعية.

يعد __Laravel Eloquent__ أحد الميزات الرئيسية في إطار عمل __Laravel__. ويرجع ذلك إلى دعمه الرائع لتحديد وإنشاء وإدارة العلاقات بين جداول البيانات المختلفة. سأوضح لك في هذه السلسلة كيفية إنشاء واستخدام علاقات __Eloquent__ ، بحيث يمكنك البدء دون أي معرفة سابقة بالعلاقات.

من الضروري كمحترف أن تتعرف وتفهم أنواع العلاقات الرئيسية الستة التي سنمر بها ونراجعها.
ولكن قبل هذا يجب أن نعرف ما هى العلاقات من الأساس؟

## ما هي العلاقات فى قاعدة البيانات؟
عند العمل مع الجداول في قاعدة بيانات يوجد بينها علاقات ، يمكننا وصف العلاقات على أنها اتصالات بين الجداول. يساعدك هذا في تنظيم البيانات وهيكلتها دون عناء مما يتيح إمكانية قراءة فائقة ومعالجة للبيانات بشكل أسرع.

## ما هي أنواع العلاقات الموجودة فى Laravel؟
 هناك ثلاثة أنواع رئيسية من العلاقات قى قاعدة البيانات تظهر عند الممارسة:
 * __one-to-one__: وهو سجل واحد مرتبط بسجل واحد فقط. ومثال على ذلك أن كل مستخدم يمتلك بروفايل واحد خاص به فى جدول أخر.
 * __one-to-many__: ومثال على ذلك أن كل كاتب يمتلك أكثر من تدوينه فى جدول أخر.
 * __many-to-many__: ومثال على ذلك أنه يمكن لأكثر من مستخدم الانضمام إلى أكثر من دورة واحدة.

إلى جانب هذه العلاقات ، يقدم __Laravel__ المزيد منها ، وهي:
* __Has Many Through__
* __Polymorphic Relations__
* __Many-to-many Polymorphic__

ليصبح عدد العلاقات التى سنقوم بشرحها هم 6 أنواع.
وسنقوم ببناء نظام إدارة محتوى بسيط لشرح جميع العلاقات.

هل أحتاج إلى معرفة خاصة بـ __Eloquent__ قبل قراءة هذا؟
في الأمثلة أدناه ، حاولت أن أشرح كل شيء بأكبر قدر ممكن من الوضوح ، دون استخدام الكثير من وظائف __Eloquent__ الصعبة والتقنيات المعقدة. هذا يعني أن المعرفة السابقة ليست ضرورية تمامًا. ومع ذلك ، من الأفضل دائمًا تعلم الأساسيات أولاً ، ثم متابعة موضوعات أكثر تعقيدًا مثل العلاقات.

## One-To-One Relationship
نظرًا لكونها أول علاقة أساسية وأبسطها يقدمها __Laravel__ ، فإنهم يربطون جدولين بطريقة بحيث يرتبط صف واحد من الجدول الأول بصف واحد فقط من الجدول الآخر.

لرؤية هذا كتطبيق عملى سنبدأ بإنشاء نظام إدارة المحتوى،

لنفترض أن كل مستخدم يمكن أن يكون له ملف تعريف واحد. في بعض الحالات ، يمكنك تخزين جميع معلومات الملف الشخصي في جدول المستخدمين ، لكن هذا لن يكون مثاليًا. هنا ، في مثالنا ، أريد إنشاء جدول منفصل لهم. على سبيل المثال ، إذا أردنا لاحقًا نقل ملف تعريف إلى مستخدم مختلف ، فسيكون هذا مفيدًا.

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

* لنقم بإنشاء ***`Profile Model`***.
```bash
php artisan make:model Profile -m
```

فى علاقة ***`One-to-One`*** لدينا الحرية فى اختيار احد هاتين الطريقتين:
* إضافة ***`user_id`*** فى جدول ***`profiles`***.
* إضافة ***`profile_id`*** فى جدول ***`users`***.

ولكن نتيجة طريقة تفكيرنا ، سيكون لكل مستخدم ملفه الشخصي الخاص به. وفي الغالب يتم دائمًا إضافة هذا العمود الذي يربط بين الجدولين إلى الجدول الثاني ، لذلك سنضيفه إلى جدول الملفات الشخصية على النحو التالي.
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

* لنتوجه الى ملف ***`User.php`*** لنقم بتعيين العلاقة الجديدة.
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

* هناك طريقتين لإنشاء مستخدم وربطه بعلاقة مع ملف التعريف الخاص به:

> سنتوجه أولا لملف ***`routes/web.php`*** نضيف رابط جديد حتى نستطيع أن نختبر هذه الطرق.
1. بدون إستخدام **function profile**.
```PHP
Route::get('/one-to-one', function () {
    $user = \App\Models\User::create(['username' => 'John Doe']);
    \App\Models\Profile::create([
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
> سنقوم بفتح المتصفح ونذهب الى هذا الرابط ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```


2. بإستخدام ***`function profile`***.
```PHP
Route::get('/one-to-one', function () {
    $user = \App\Models\User::create(['username' => 'Tom Cruz']);
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

> سنقوم بفتح المتصفح ونذهب الى هذا الرابط مرة أخرى ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
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

> لنقل أنك تريد تسمية العلاقة بإسم أخر مثل ***`admin`*** فيجب علينا إضافة ***`foreignKey`***s.
```PHP
public function admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```
> وإذا لم تقم بإضافة ***`foreignKey`*** عند تغيير أسم العلاقة سيظهر لك هذا الخطأ.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/Attempt-to-read-property-X-on-null.png"
alt="Attempt to read property X on null"
caption="Attempt to read property X on null"
>}}


* هناك طريقة أخرى لإنشاء ملف تعريف وربطه بعلاقة عكسية مع المستخدم الذى يمتلكه من خلال **`function user`**.
```PHP
Route::get('/one-to-one', function () {
    $user = \App\Models\User::create(['username' => 'Adam Smith']);
    $profile = new \App\Models\Profile([
        'firstname' => 'Adam',
        'lastname' => 'Smith',
        'birthday' => '01-01-1999'
    ]);

    $profile->user()->associate($user);
    $profile->save();
    return response()->json([
        'username' => $profile->user->username,
        'firstname' => $profile->firstname,
        'lastname' => $profile->lastname,
    ]);
});
```

> سنقوم بفتح المتصفح ونذهب الى هذا الرابط مرة أخرى ***`http://127.0.0.1:8000/one-to-one`*** لنجد أنه تم إنشاء المستخدم بنجاح.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

## كيفية تحسين استعلام Eloquent في Laravel؟
عند التعامل مع قاعدة بيانات كبيرة بداخلها الكثير من البيانات يأتى الوقت لتنظر للأمر بنظرة أخرى.
فالأمر ليس مجرد جلب للبيانات ولكن كم من الوقت ستحتاج لجلب تلك البيانات.
وكم عدد الإستعلامات التى ستقوم فى كل صفحة. ولإختبار هذا الأمر سنقوم بتنزيل مكتبة مشههور جدا اسمها 
[Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). ستساعدنا فى معرفة جميع الإستعلامات التى تتم فى كل صفحة.

* لنقم بتنفيذ هذا الأمر.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> وتأكد أن ***`APP_DEBUG=true`*** داخل ملف ***`.env`***.

* ثم سنقوم بإنشاء رابط جديد لعرض جميع المستخدمين الذين تم حفظهم فى قاعدة البينات أثناء قيامنا بالخطوات السابقة.
```PHP
Route::get('/users', function () {
    $users = \App\Models\User::all();
    return view('users.oneToOne', compact('users'));
});
```

* ونضيف أيضا بداخل فولدر ***`views`*** فولدر أخر أسمه ***`users`*** وبداخله نضيف ملف ***`oneToOne.blade.php`*** ونقوم بإضافة هذا الجدول البسيط لعرض المستخدمين.
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

* نتجه الأن للمتصفح ونذهب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/users-view.png"
alt="Users View"
caption="Users View"
>}}

> سنجد الأن أسفل الصفحة شريط ***`Laravel Debugbar`*** عند فتحه سنجد أن عدد الإستعلامات الموجوده فى هذه الصفحة "4" إستعلامات. وهم فقط 3 مستخدمين, تخيل معى لو داخل قاعدة البيانات الاف او ملايين المستخدمين سيكون وقت تحميل هذه الصفحة بظئ جدا. وهذا ما يسمى بمشكلة N+1 فى Laravel أو ما يطلق عليه Lazy Loading.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/laravel-debugbar-lazy-loading.png"
alt="laravel debugbar lazy loading"
caption="laravel debugbar lazy loading"
>}}

* ولحل هذه المشكلة نقوم بتعديل طريقة جلب المستخدمين بالشكل التالى.
```PHP
$users = \App\Models\User::with('profile')->get();
```
> وكما ترى بإضافة كلمة ***`with`*** مع أسم العلاقة سيتم التغلب على هذه المشكلة فتم تقليص عدد الإستعلامات من 4 الى 2 وهذا الأمر سترى تأثيره بشكل واضح إذا كانت قاعدة البيانات متوسطة الحجم او كبيرة بالطبع وهذا ما يطلق عليه Eager Loading.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/laravel-debugbar-eager-loading.png"
alt="laravel debugbar eager loading"
caption="laravel debugbar eager loading"
>}}

> ويمكننا القيام بنفس تلك الخطوات السابقة إذا كنا سنستخدم العلاقة بشكل عكسى.
مثال على هذا.

* ثم سنقوم بإنشاء رابط جديد لعرض جميع ملفات التعريف الذين تم حفظهم فى قاعدة البينات أثناء قيامنا بالخطوات السابقة.
```PHP
Route::get('/profiles', function () {
    $profiles = \App\Models\Profile::with('user')->get();
    return view('profiles.oneToOne', compact('profiles'));
});
```

* ونضيف أيضا بداخل فولدر ***`views`*** فولدر أخر أسمه ***`profiles`*** وبداخله نضيف ملف ***`oneToOne.blade.php`*** ونقوم بإضافة هذا الجدول البسيط لعرض المستخدمين.
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

* نتجه الأن للمتصفح ونذهب الى الرابط التالى ***`http://127.0.0.1:8000/profiles`*** لنرى ما هى النتائج التى ستظهر.
> ستجد أن النتائج التى سوف تظهر تشبه نفس النتائج السابقة.

### الخاتمة
هذه المقالة هى بداية لسلسلة كاملة عن ***Laravel Eloquent Relationships*** - العلاقات داخل ***Laravel***.
وقد تناولنا فيها علاقة ***One TO One*** بطريقة بسيطة ولم نبخل عليكم بأى معلومة, وإن شاء الله فى المقال القادم سنتعرف على علاقة ***One To Many***. وفى الختام نصلى على رسول الله سيدنا محمد وعلى اُله وصحبه أجمعين.