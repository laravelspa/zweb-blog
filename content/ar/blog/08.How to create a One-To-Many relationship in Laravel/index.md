---
title: "كيف تنشئ علاقة One-To-Many فى Laravel؟"
date: 2023-08-16
draft: false
slug: "how-to-create-a-One-To-Many-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationship']
series_order: 7
---

بعد أن تعرفنا على أنواع العلاقات داخل __Laravel__ فى الحزء السابق.
وقمنا بتناول أول نوع من هذه العلاقات وهى علاقة One-To-One.

اليوم نستكمل السلسلة  التى بدأناها فى تعلم __Laravel Eloquent Relationships__.

و نتكلم عن النوع الثانى والذى يطلق عليه __One-To-Many__ او __hasMany__.

![laravel one to many relationship](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023.png "laravel one to many relationship")


## كيف تنشئ علاقة One-To-Many فى Laravel؟
![كيف تُنشئ علاقة One-To-Many في Laravel؟](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/ar/how-to-create-a-One-To-Many-relationship-in-laravel.png "كيف تُنشئ علاقة One-To-Many في Laravel؟")

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

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}
