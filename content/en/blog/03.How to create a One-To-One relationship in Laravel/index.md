---
title: "How to create a One-To-One relationship in Laravel?"
date: 2023-08-11
draft: false
slug: "how-to-create-a-One-To-One-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 2
---
![How to create a One-To-One relationship in Laravel?](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-create-a-One-To-One-relationship-in-laravel.png "How to create a One-To-One relationship in Laravel?")

__One-to-One relationships__ are the simplest type of relationship offered by Laravel. They join two tables such that one row in the first table is associated with only one row in the other table, or the same table.

To see this in action, we'll start by creating a content management system.

To see this in action, let's start by creating a content management system. Let's say each user has their own single profile. In some cases, you can store all profile information in the **Users** table. However, this is not ideal.

In our example, we want to create a separate table for personal profiles. This will allow us to transfer a profile from one user to another easily if we need to.


> By default, the **Users** table exists in Laravel. The columns that it contains do not matter for this example.

* Let's say we have a users table with the following columns:
```PHP
Schema::create('users', method (Blueprint $table) {
  $table->id();
  $table->string('username');
  $table->string('email')->unique();
  $table->timestamps();
});
```

* We edit the ***`User.php`*** file.
```PHP
protected $fillable = ['username'];
```

* We create ***`Profile Model`*** with its table.
```bash
php artisan make:model Profile -m
```

In a ***`one-to-one`*** relationship, we have the freedom to choose one of these two methods to establish the relationship:

* Add ***`user_id`*** in the ***`profiles`*** table.
* Add ***`profile_id`*** in the ***`users`*** table.

> Usually, the column that joins the two tables is added to the second table. So, we will add it to the profiles table as follows:
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

* We edit the ***`Profile.php`*** file.
```PHP
protected $fillable = [
  'user_id',
  'firstname',
  'lastname',
  'birthday'
];
```

* Let's run this command to update the database.
```bash
php artisan migrate
```

* Let's go to the ***`User.php`*** file to define the relationship.
```PHP
public method profile() {
    return $this->hasOne(Profile::class);
}
```

> Let's see how the ***`hasOne()`*** method works.
> This method is used to save the ***id*** of the related model in the ***foreign key*** column of the parent model.
```PHP
$this->hasOne(Profile::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* Let's go to the ***`Profile.php`*** file to define the inverse relationship.
```PHP
public method user() {
    return $this->belongsTo(User::class);
}
```

> Let's find out how the ***`belongsTo()`*** method works.
> This method is used to save the ***id*** of the parent model in the primary key column of the related model.
```PHP
$this->belongsTo(User::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // OwnerKey By Default Id
);
```

> Let's say you want to name the relationship something like ***`admin`***, we need to add the ***`foreignKey`*** property to the relationship method.
```PHP
public method admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```

> The ***`foreignKey`*** property is used to specify the name of the column in the child model that is used to reference the parent model.

> If you do not add the ***`foreignKey`*** property to the relationship method when changing the relationship name, you will see the following error:
![Attempt to read property X on null](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/Attempt-to-read-property-X-on-null.png "Attempt to read property X on null")

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
