---
title: "[Arabic] Laravel Eloquent One-To-One Relationship - Ultimate Guide 2023"
date: 2023-08-10
draft: false
slug: "laravel-eloquent-one-to-one-relationship-ultimate-guide-2023-in-arabic"
description: "علاقة Laravel one-to-one هي علاقة قاعدة بيانات حيث يمكن ربط كل صف في جدول واحد بصف واحد بالضبط في جدول آخر. يمكن إنشاؤه ومعالجته بسهولة باستخدام methods hasOne() و belongsTo() في Laravel."
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel Eloquent Relationships']
series_order: 1
---

في كثير من الأحيان ، يحتاج المطورون إلى التفاعل مع قواعد البيانات. إذا كنت تستخدم إطار عمل Laravel ، فيجب أن تعرف عن إحدى ميزاته الأكثر أهمية: Eloquent ، مخطط ربط الكائنات (ORM) الذي يجعل هذه العملية بسيطة وسهلة.

__Laravel Eloquent__ هي واحدة من السمات الرئيسية في إطار عمل __Laravel__. ويرجع ذلك إلى دعمه الكبير لتعريف العلاقات بين الجداول المختلفة وإنشاؤها وإدارتها. في هذه السلسلة من المقالات ، سأوضح لك كيفية إنشاء واستخدام __Laravel Eloquent__.

من المهم ملاحظة أنه يمكنك البدء في استخدام Eloquent دون أي معرفة مسبقة بالعلاقات.

![laravel one to one relationship](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023.png "laravel one to one relationship")

كمبرمج محترف ، من الضروري فهم أنواع العلاقات. ومع ذلك ، يجب أن تسأل نفسك سؤالًا مهمًا: ما هي العلاقات في المقام الأول؟

## ما هي العلاقات في قواعد البيانات؟
![ما هي العلاقات في قواعد البيانات؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/what-are-relationships-in-database.png "ما هي العلاقات في قواعد البيانات؟")

عند العمل مع جداول في قاعدة بيانات لها علاقات فيما بينها ، يمكننا وصف هذه العلاقات على أنها روابط بين تلك الجداول. يساعدك هذا في تنظيم البيانات وهيكتلها دون عناء ، مما يسمح بقراءة البيانات ومعالجتها بشكل أسرع.

## ما هي أنواع العلاقات الموجودة في Laravel؟
![ما هي أنواع العلاقات الموجودة في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/what-types-of-relationships-are-there-in-laravel.png "ما هي أنواع العلاقات الموجودة في Laravel؟")

هناك ثلاثة أنواع رئيسية من العلاقات في قواعد البيانات شائعة الاستخدام في الممارسة:

* __one-to-one (واحد لواحد)__: تعني علاقة رأس برأس أن سجلًا واحدًا في جدول واحد يمكن أن يكون مرتبطًا بسجل واحد فقط في جدول آخر. على سبيل المثال ، قد يكون لجدول العملاء علاقة رأس برأس بجدول عنوان. هذا يعني أنه يمكن لكل عميل الحصول على عنوان واحد فقط ، ويمكن ربط كل عنوان بعميل واحد فقط.

* __one-to-many (واحد لكثير)__: تعني علاقة رأس بأطراف أنه يمكن ربط سجل واحد في جدول واحد بالعديد من السجلات في جدول آخر. على سبيل المثال ، قد يكون لجدول الطلبات علاقة رأس بأطراف بجدول منتج. هذا يعني أنه يمكن أن يحتوي طلب واحد على العديد من المنتجات ، ولكن لا يمكن ربط كل منتج إلا بطلب واحد.

* __many-to-many (كثير إلى كثير)__: تعني علاقة أطراف بأطراف أن العديد من السجلات في جدول واحد يمكن أن تكون مرتبطة بالعديد من السجلات في جدول آخر. على سبيل المثال ، قد يكون لجدول الطالب علاقة أطراف بأطراف بجدول المقرر الدراسي. هذا يعني أنه يمكن للعديد من الطلاب أخذ العديد من الدورات ، ويمكن للعديد من الطلاب أخذ العديد من الدورات.

إلى جانب هذه العلاقات ، يقدم __Laravel__ المزيد من العلاقات ، وهي:

* __Has Many Through (لديه الكثير من خلال)__: تسمح لك هذه العلاقة بربط نموذج بنموذج آخر من خلال نموذج ثالث. على سبيل المثال ، قد يحتوي نموذج المستخدم على العديد من خلال العلاقة مع نموذج المشاركة ، حيث يكون النموذج الثالث هو نموذج الفئة. هذا يعني أنه يمكن للمستخدم الحصول على العديد من المشاركات ، حيث تنتمي كل مشاركة إلى فئة معينة.

* __Polymorphic Relations (العلاقات متعددة الأشكال)__: تسمح لك هذه العلاقة بربط نموذج بعدة نماذج أخرى من أنواع مختلفة. على سبيل المثال ، قد يكون لنموذج التعليق علاقة متعددة الأشكال بنموذج ، حيث يمكن أن يكون النموذج منشور مدونة أو منتجًا أو مستخدمًا. هذا يعني أنه يمكن ربط تعليق بأي من هذه الأنواع الثلاثة من النماذج.

* __Many-to-many Polymorphic (متعدد الأشكال متعدد الأشكال)__: هذه العلاقة عبارة عن مزيج من العلاقات متعددة الأشكال والمتعددة الأشكال. يسمح لك بربط نموذج بالعديد من النماذج الأخرى من أنواع مختلفة ، حيث يتم التوسط في العلاقة بواسطة نموذج ثالث. على سبيل المثال ، قد يكون لنموذج المستخدم علاقة متعددة الأشكال متعددة الأشكال مع نموذج ، حيث يكون النموذج الثالث هو نموذج يحتذى به. هذا يعني أنه يمكن أن يكون للمستخدم العديد من الأدوار ، حيث يمكن أن يكون كل دور مستخدمًا أو منتجًا أو منشور مدونة.

عدد العلاقات التي سنشرحها هو 6 أنواع. سنقوم ببناء نظام بسيط لإدارة المحتوى لشرح كل تلك العلاقات.

> هل أحتاج إلى معرفة محددة بـ Eloquent قبل قراءة هذا؟

ردًا على سؤالك حول ما إذا كنت بحاجة إلى معرفة محددة بـ Eloquent قبل قراءة هذا ، أود أن أقول إنه ليس ضروريًا تمامًا ، ولكنه سيكون مفيدًا. لا تستخدم الأمثلة في المقالة أي طرق أو تقنيات Eloquent معقدة ، ولكن سيكون من المفيد أن يكون لديك فهم أساسي لكيفية عمل Eloquent قبل قراءة المقالة.

إذا كنت جديدًا على Laravel ، فإنني أوصي بقراءة توثيق Laravel: https://laravel.com/docs/ قبل قراءة هذه المقالة. يوفر التوثيق نظرة عامة جيدة على Eloquent وكيف يعمل.

بمجرد أن يكون لديك فهم أساسي لـ Eloquent ، يجب أن تكون قادرًا على اتباع الأمثلة الواردة في المقالة دون أي مشاكل. ومع ذلك ، إذا واجهتك مشكلة ، يمكنك دائمًا الرجوع إلى الوثائق للحصول على مزيد من المساعدة.


في الأمثلة أدناه ، حاولت أن أشرح كل شيء بأكبر قدر ممكن من الوضوح ، دون استخدام الكثير من أساليب Eloquent الصعبة والتقنيات المعقدة. هذا يعني أن المعرفة المسبقة ليست ضرورية تمامًا. ومع ذلك ، فمن الأفضل دائمًا تعلم الأساسيات أولاً ثم متابعة موضوعات أكثر تعقيدًا مثل العلاقات.

## كيف تُنشئ علاقة One-To-One في Laravel؟
![كيف تُنشئ علاقة One-To-One في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-create-a-One-To-One-relationship-in-laravel.png "كيف تُنشئ علاقة One-To-One في Laravel؟")

__علاقات واحد لواحد__ هي أبسط أنواع العلاقات التي يقدمها Laravel. يتم من خلالها ربط جدولين بحيث يرتبط صف واحد في الجدول الأول بصف واحد فقط في الجدول الآخر ، أو نفس الجدول.

لرؤية هذا في العمل ، سنبدأ بإنشاء نظام إدارة المحتوى.

لرؤية هذا عمليًا ، لنبدأ بإنشاء نظام إدارة المحتوى. لنفترض أن لكل مستخدم ملفه الشخصي الفردي. في بعض الحالات ، يمكنك تخزين جميع معلومات الملف الشخصي في جدول **المستخدمون**. ومع ذلك ، هذا ليس مثاليًا.

في مثالنا ، نريد إنشاء جدول منفصل للملفات الشخصية. سيسمح لنا ذلك بنقل ملف تعريف من مستخدم إلى آخر بسهولة إذا احتجنا إلى ذلك.

> افتراضيًا ، يوجد جدول **Users** في Laravel. الأعمدة التي يحتوي عليها لا تهم في هذا المثال.

* لنفترض أن لدينا جدول مستخدمين بالأعمدة التالية:
```PHP
Schema::create('users', method (Blueprint $table) {
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

في علاقة *** "واحد لواحد" *** ، لدينا حرية اختيار إحدى هاتين الطريقتين لتأسيس العلاقة:

* أضف ***`user_id`*** في جدول ***`profiles`***.
* أضف ***`profile_id`*** في جدول ***`users`***.

> عادة ، يتم إضافة العمود الذي يربط بين الجدولين إلى الجدول الثاني. لذلك سنقوم بإضافته إلى جدول الملفات الشخصية على النحو التالي:
```PHP {hl_lines=["6"]} 
Schema::create('profiles', method (Blueprint $table) {
  $table->id();
  $table->string('firstname');
  $table->string('lastname');
  $table->string('birthday');
  $table->foreignId('user_id')->constrained();
  $table->timestamps();
});
```

* نقوم بتحرير ملف ***`Profile.php`***.
```PHP
protected $fillable = [
  'user_id',
  'firstname',
  'lastname',
  'birthday'
];
```

* لنقم بتشغيل هذا الأمر لتحديث قاعدة البيانات.
```bash
php artisan migrate
```

* دعنا ننتقل إلى ملف ***`User.php`*** لتحديد العلاقة.
```PHP
public method profile() {
    return $this->hasOne(Profile::class);
}
```

> لنرى كيف تعمل method ***`hasOne()`***.
> تُستخدم هذه الطريقة لحفظ ***id*** للنموذج ذي الصلة في عمود ***foreign key*** للنموذج الأصل.
```PHP
$this->hasOne(Profile::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* دعنا ننتقل إلى ملف ***`Profile.php`*** لتحديد العلاقة العكسية.
```PHP
public method user() {
    return $this->belongsTo(User::class);
}
```

> لنكتشف كيف تعمل ***`belongsTo()`*** method.
> تُستخدم هذه method لحفظ ***id*** للنموذج الأصلي في عمود المفتاح الأساسي للنموذج ذي الصلة.
```PHP
$this->belongsTo(User::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // OwnerKey By Default Id
);
```

> لنفترض أنك تريد تسمية العلاقة بشيء مثل ***`admin`*** ، نحتاج إلى إضافة property ***`foreignKey`*** إلى العلاقة.
```PHP
public method admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```

> يتم استخدام ***`foreignKey`*** property لتحديد اسم العمود في النموذج الفرعي المستخدم للإشارة إلى النموذج الأصلي.

> إذا لم تقم بإضافة ***`foreignKey`*** property إلى العلاقة عند تغيير اسم العلاقة ، فسترى الخطأ التالي:
![Attempt to read property X on null](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/Attempt-to-read-property-X-on-null.png "Attempt to read property X on null")


## كيفية إدراج البيانات في علاقة واحد لواحد في قاعدة البيانات؟
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

## كيف يمكنك استرداد البيانات من علاقة one to one في Laravel؟
![كيف يمكنك استرداد البيانات من علاقة one to one في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-do-you-get-data-into-a-One-To-One-relationship-in-laravel.png "كيف يمكنك استرداد البيانات من علاقة one to one في Laravel؟")

لقد رأينا كيف يتم حفظ البيانات بعدة طرق مختلفة في قاعدة البيانات باستخدام علاقة one to one في Laravel. لكن كيف نسترجع البيانات من قاعدة البيانات؟ هناك عدة طرق لاسترداد البيانات من قاعدة البيانات.

يمكن تقسيم هذه الطرق إلى فئتين رئيسيتين:

* الاسترجاع المباشر: تتضمن هذه الطريقة الاستعلام مباشرة عن قاعدة البيانات عن البيانات التي تحتاجها. هذه هي الطريقة الأكثر فاعلية لاسترداد البيانات ، ولكن قد يكون من الصعب استخدامها إذا لم تكن على دراية بـ SQL.

* Eloquent ORM: يوفر Eloquent ORM في Laravel طريقة سهلة الاستخدام لاسترداد البيانات من قاعدة البيانات. يستخدم Eloquent ORM مجموعة من الأساليب لتعيين جداول قاعدة البيانات إلى كائنات في PHP. هذا يجعل من السهل استرداد البيانات من قاعدة البيانات دون الحاجة إلى معرفة SQL.
في هذه السلسلة ، سنركز على استخدام Eloquent ORM لاسترداد البيانات من قاعدة البيانات.
### أثناء استرداد بيانات المستخدمين:
أثناء جمع بيانات المستخدم ، سنحصل على ملفاتهم الشخصية. بعد ذلك ، يتم تقسيم موضوع عرض تلك البيانات إلى شكلين ، ولا ثالث لهما. سيحدد نوع التطبيق الذي تعمل عليه النموذج الذي يجب استخدامه.

* أولاً: تطبيق يستخدم مسارات الويب.
* ثانيًا: تطبيق يعتمد على مسارات API.
#### أولاً: تطبيق يستخدم مسارات الويب.
1. ننتقل أولاً إلى ملف ***`routes/web.php`*** ونجري التغييرات التالية:
```PHP
Route::get('/users', method () {
    $users = User::with(['profile'])->get();
    return view('users.list', compact('users'));
});
```

> إذا تحققنا من الاستجابة لهذا الأمر ، فسنجد أنه تم الحصول على جميع البيانات من قاعدة البيانات.
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

هنا ، يمكنك فقط اختيار الأعمدة التي تحتاجها في النموذج التالي:
```PHP
Route::get('/users', method () {
    $users = User::with(['profile:firstname,lastname,user_id'])->get();
    return view('users.list', compact('users'));
});
```

> إذا تحققنا الآن مرة أخرى ، فستجد أن حجم البيانات قد تم تصغيره.
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

> هنا ، نرى الفرق بين الحالتين في حجم البيانات المعالجة. تزداد المسألة بشكل كبير إذا كانت البيانات أكبر من ذلك بكثير.

> عند اختيار أعمدة معينة من العلاقات ، يجب دائمًا اختيار عمود ***`foreign key`***. بدون اختياره ، لن يتم إرجاع البيانات بشكل صحيح من قاعدة البيانات.

2. داخل مجلد ***`views`*** ، نضيف مجلدًا آخر باسم ***`users`***. بداخله نضيف الملف ***`list.blade.php`***. في ***`list.blade.php`*** نضيف هذا الجدول البسيط لعرض المستخدمين:
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

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/users`*** لترى النتائج التي ستظهر.
![Laravel One To One Relationship - Users Table](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png "Users Table")

#### ثانيًا: تطبيق يعتمد على مسارات API.

> ما هي API Resources؟

ببساطة ، إنها طبقة وسيطة بين Eloquent واستجابة API ، حيث تقوم بتحويل البيانات التي تم الحصول عليها من قاعدة البيانات إلى JSON مع القدرة على تحديد بيانات معينة دون غيرها أو معالجة تلك البيانات.

1. نقوم بإنشاء API Resocrces للمستخدمين والملفات الشخصية. قم بتنفيذ هذا الأمر في موجه الأوامر.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
```

2. انتقل إلى المسار التالي: *** `App\Http\Resources`*** وعدّل كليهما:

* ملف ***`ProfileResource.php`***.
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class ProfileResource extends JsonResource
{
    public method toArray(Request $request): array
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
    public method toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'username' => $this->username,
            'profile' => ProfileResource::make($this->whenLoaded('profile')),
        ];
    }
}
```

3. انتقل إلى ملف ***`routes/api.php`*** الخاص بك وأضف مسارًا جديدًا.
```PHP
use App\Models\User;
use App\Http\Resources\UserResource;
---
Route::get('/users', method () {
    $users = User::with(['profile'])->get();
    $usersResource = UserResource::collection($users);
    return response()->json($usersResource);
});
```

4. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/api/users`*** لترى النتائج التي ستظهر.
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

> نرى هنا أيضًا أننا حصلنا على البيانات المطلوبة المحددة في ملفات موارد API فقط.

### أثناء استرداد بيانات الملفات الشخصية:
أثناء الحصول على بيانات الملف الشخصي ، سنحصل على كل مستخدم مرتبط بهذه الملفات الشخصية.
بعد ذلك ، يمكن عرض البيانات في شكلين لا ثالث لهما.
سيحدد نوع التطبيق الذي تعمل عليه والموضوع المستخدم أي شكل من أشكال عرض البيانات هو الأنسب.

* أولاً: تطبيق يستخدم مسارات الويب.
* ثانيًا: تطبيق يعتمد على مسارات API.
#### أولاً: تطبيق يستخدم مسارات الويب.
1. ننتقل أولاً إلى ملف ***`routes/web.php`*** ونجري التغييرات التالية:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user')->get();
    return view('profiles.list', compact('profiles'));
});
```
> إذا تحققنا من الاستجابة لهذا الأمر ، فسنجد أنه تم الحصول على جميع البيانات من قاعدة البيانات.
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

هنا ، يمكنك فقط اختيار الأعمدة التي تحتاجها في النموذج التالي:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user:username,id')->get();
    return view('profiles.list', compact('profiles'));
});
```

> إذا تحققنا الآن مرة أخرى ، فستجد أن حجم البيانات قد تم تصغيره.
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

> هنا ، نرى الفرق بين الحالتين في حجم البيانات المعالجة. تزداد المسألة بشكل كبير إذا كانت البيانات أكبر من ذلك بكثير.

> عند تحديد أعمدة معينة في العلاقات ، يجب تضمين حقل ***`id`*** عند اختيار علاقة ***`user`***. خلاف ذلك ، لن يتم إرجاع المستخدم مع بيانات ملف التعريف.

2. داخل مجلد ***`views`*** ، نضيف مجلدًا آخر باسم ***`profiles`***. بداخله نضيف الملف ***`list.blade.php`***. في ***`list.blade.php`*** نضيف هذا الجدول البسيط لعرض الملفات الشخصية للمستخدمين.:
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

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/profiles`*** لترى النتائج التي ستظهر.
![Laravel One To One Relationship - Profiles Table](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png "Profiles Table")

#### ثانيًا: تطبيق يعتمد على مسارات API.

1. انتقل إلى المسار التالي: *** `App\Http\Resources`*** وعدّل هذا الملف ***`ProfileResource.php`***:
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class ProfileResource extends JsonResource
{
    public method toArray(Request $request): array
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

2. انتقل إلى ملف ***`routes/api.php`*** الخاص بك وأضف مسارًا جديدًا.
```PHP
use App\Models\Profile;
use App\Http\Resources\ProfileResource;
---
Route::get('/profiles', method () {
    $profiles = Profile::with(['user'])->get();
    $profilesResource = ProfileResource::collection($profiles);
    return response()->json($profilesResource);
});
```

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/api/profiles`*** لترى النتائج التي ستظهر.
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
![كيفية تحسين استعلامات Eloquent في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-improve-Eloquent-queries-in-laravel.png "كيفية تحسين استعلامات Eloquent في Laravel؟")

عند التعامل مع قاعدة بيانات كبيرة بها الكثير من البيانات ، من المهم مراعاة الأداء. هذا لا يعني فقط الوقت الذي يستغرقه الحصول على البيانات ، ولكن أيضًا عدد الاستعلامات التي يتم تنفيذها لكل صفحة.

لاختبار أداء تطبيقك ، يمكنك استخدام مكتبة تسمى [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). سيعرض لك شريط التصحيح جميع الاستعلامات التي يتم تنفيذها على كل صفحة ، بالإضافة إلى الوقت الذي يستغرقه كل استعلام في التنفيذ. يمكن أن تساعدك هذه المعلومات في تحديد معوقات الأداء وتحسين تطبيقك.

* لنقم بتشغيل هذا الأمر.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> تأكد من أن ***`APP_DEBUG=true`*** داخل ملف ***`.env`***.

* الفرق بين الحالتين كالتالي:
```PHP
$users = User::all();
$users = User::with('profile')->get();
```

* افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/users`*** لترى النتائج التي ستظهر في شريط المكتبة.

1. __(Lazy Loading)__ - استرداد البيانات بدون استخدام ***`with`***.
![laravel debugbar lazy loading](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png "laravel debugbar lazy loading")

في الجزء السفلي من الصفحة ، سنجد الآن شريطًا لمكتبة __Laravel Debugbar__. عند النقر فوقه ، سنجد أنه يعمل مع الكثير من البيانات. الشيء الذي يثير اهتمامنا هنا هو عدد استعلامات SQL في هذه الصفحة ، وكما هو موضح ، فهي __4__ استعلامات.

وقمنا باسترجاع 3 مستخدمين فقط ، تخيلوا معي إذا كان هناك عشرات الآلاف أو الملايين من المستخدمين داخل قاعدة البيانات هذه ، فإن وقت تحميل هذه الصفحة سيكون بطيئًا للغاية بسبب العدد الهائل من الإستعلامات.

هذه تسمى مشكلة N+1 في Laravel. يحدث هذا بسبب lazy loading relationships ، مما يعني أن البيانات المرتبطة لا يتم تحميلها حتى يتم الاحتياج إليها بالفعل. في هذه الحالة ، نتكاسل في تحميل علاقة الملف الشخصى لكل مستخدم. هذا يعني أننا نجري استعلامًا منفصلاً لكل مستخدم للحصول على ملفه الضخصى.

إذا كان هناك 1000 مستخدم ، فسنقدم 1001 استعلامًا: استعلام واحد لجذب المستخدمين ، و 1000 استعلام للحصول على ملفهم الشخصى. يعد هذا مضيعة لموارد الخادم ويمكن أن يجعل تطبيقك بطيئًا.

لحل مشكلة N+1 ، يمكنك تحميل العلاقات eager loading. هذا يعني أنك ستقوم بتحميل البيانات المرتبطة عند استرداد البيانات لأول مرة. في هذه الحالة ، يمكنك استخدام ***`with()`*** method لتحميل علاقة الملف الشخصى. سيؤدي هذا إلى إجراء استعلام واحد فقط للحصول على المستخدمين وملفاتهم الشخصية.

يمكن أن يؤدي التحميل الجاد إلى تحسين أداء تطبيقك عن طريق تقليل عدد الاستعلامات. إنها ممارسة جيدة يجب تحميل العلاقات eager loading كلما أمكن ذلك.

2. __(Eager Loading)__ - استرداد البيانات بإستخدام ***`with`***.
![laravel debugbar eager loading](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png "laravel debugbar eager loading")

كما ترى ، بإضافة طريقة with () مع اسم العلاقة ، سيتم جلب بيانات كل مستخدم بملف التعريف الخاص به بدون مشكلة N + 1.

لذلك تم تقليل عدد الاستعلامات من 4 إلى 2 فقط. من الواضح أن هذا سيكون له تأثير إذا كانت قاعدة البيانات هذه متوسطة الحجم أو كبيرة. هذا ما يسمى eager loading.

## كيفية تحديث علاقة one-to-one في Laravel؟
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

## كيف تحذف البيانات من علاقة واحد إلى واحد في Laravel؟
![كيف تحذف البيانات من علاقة واحد إلى واحد في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-delete-data-from-one-to-one-relationship-in-laravel.png "كيف تحذف البيانات من علاقة واحد إلى واحد في Laravel؟")
### حذف البيانات باستخدام نموذج المستخدم.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/users/profile/delete', method () {
    $user = User::with('profile')->find(1);
    $user->profile()->delete();
    return response()->json($user);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/profile/delete`*** لنجد أنه تم حذف ملف التعريف بنجاح.
```json
{
  "id": 1,
  "username": "Joun Doe",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:07:38.000000Z",
  "profile": null
}
```
> قم بتحديث الصفحة مرتين لإظهار أنه تم حذف ملف تعريف هذا المستخدم.
### حذف البيانات باستخدام نموذج الملف الشخصي.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/profiles/user/delete', method () {
    $profile = Profile::with('user')->findOrFail(2);
    $profile->delete();
    $profile->user()->delete();
});
```
* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/profiles/user/delete`***. نرى أنه تم حذف المستخدم والملف الشخصي بنجاح.
![Rcord has deleted](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png "Rcord has deleted")

## الخاتمة
هذه المقالة هي بداية سلسلة كاملة عن __Laravel Eloquent Relationships__ علاقات ضمن __Laravel__. لقد غطينا __علاقة واحد إلى واحد__ بطريقة كاملة. لم ندخر لكم أي معلومة ، وإن شاء الله ، سنتعرف في الشرح التالي على __علاقة واحد إلى كثير__.

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}