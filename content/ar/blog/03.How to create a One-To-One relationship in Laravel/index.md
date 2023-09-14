---
title: "كيف تُنشئ علاقة One-To-One في Laravel؟"
date: 2023-08-11
draft: false
slug: "how-to-create-a-One-To-One-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 2
---
## كيف تُنشئ علاقة One-To-One في Laravel؟
![كيف تُنشئ علاقة One-To-One في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-create-a-One-To-One-relationship-in-laravel.png "كيف تُنشئ علاقة One-To-One في Laravel؟")

__علاقات واحد لواحد__ هي أبسط أنواع العلاقات التي يقدمها Laravel. يتم من خلالها ربط جدولين بحيث يرتبط صف واحد في الجدول الأول بصف واحد فقط في الجدول الآخر ، أو نفس الجدول.

![laravel one to one relationship](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023.png "laravel one to one relationship")

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

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}
