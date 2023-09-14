---
title: "How to create a One-To-Many relationship in Laravel?"
date: 2023-08-16
draft: false
slug: "how-to-create-a-One-To-Many-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationship']
series_order: 7
---

After we learned about the types of relationships within __Laravel__ in the previous part.
We discussed the first type of these relationships, which is the One-To-One relationship.

Today we continue the series we started learning about __Laravel Eloquent Relationships__.

We are talking about the second type, which is called __One-To-Many__ or __hasMany__.

![laravel one to many relationship](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023.png "laravel one to many relationship")


## How to create a One-To-Many relationship in Laravel?
![How to create a One-To-Many relationship in Laravel?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-create-a-One-To-Many-relationship-in-laravel.png "How to create a One-To-Many relationship in Laravel?")

 The One-To-Many relationship is one of the most important types of relationships inside __Laravel Eloquent__.
  We also learned in the previous lesson that it is the connection of a row from the first table to more than one row from the second table.

And as a continuation of the practical application (content management system), which we started in the previous lesson.
We create a __One-To-One__ relationship between the user and the personal profile.

Today we are going to create __One-To-Many__ relationship between user and post.
Each user can own one or more publications.

* We create ***`Post Model`*** with its own table.
```bash
php artisan make:model Post -m
```

* We go to this path ***`database/migrations`*** and modify the publications table by adding some columns as follows:
```PHP
Schema::create('posts', function (Blueprint $table) {
  $table->id();
  $table->string('title');
  $table->text('body');
  $table->foreignId('user_id')->constrained();
  $table->timestamps();
});
```

* We modify the ***`Post.php`*** file.
```PHP
protected $fillable = [
  'user_id',
  'title',
  'body',
];
```

* We execute this command to update the database and add the Posts table.
```bash
php artisan migrate
```

* We go to the ***`User.php`*** file and set the ***`hasMany`*** relationship.
```PHP
public function posts() {
    return $this->hasMany(Post::class);
}
```


> Let's learn how ***`hasMany`*** works
```PHP
$this->hasMany(Post::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* We go to the file ***`Post.php`*** and set the inverse relationship ***`belongsTo`***.
```PHP
public function user() {
    return $this->belongsTo(User::class);
}
```

> We have explained ***`belongsTo`*** in this part of the previous article and we are explaining the __One-To-One__ relationship.

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
