---
title: "Laravel 10 | One-To-One Relationship"
date: 2023-07-30
draft: false
slug: "laravel10-one-to-one-relationship"
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

## Intro
Often the developer needs to interact with databases. And if you are using __Laravel__, you will learn about the most important feature of this framework which is called __Eloquent__, the Object Relationship Diagram (ORM) that makes this process simple and natural.

__Laravel Eloquent__ is one of the main features in the __Laravel__ framework. This is due to its great support for defining, creating and managing relationships between different database tables. In this series I will show you how to create and use __Eloquent__ relationships, so you can get started without any prior knowledge of relationships.

It is essential as a professional that you recognize and understand the six major relationship types that we will go through and review. But before this we must know what are the relations in the first place?

## What are the relationships in the database?
When working with tables in a database between which there are relationships, we can describe relationships as connections between tables. This helps you organize and structure data effortlessly allowing superior readability and faster data processing.

## What types of relationships are there in Laravel?
There are three main types of relationships in a database that appear in practice:
* __one-to-one__: It is a single record associated with only one record. An example of this is that each user has one profile in another table.
* __one-to-many__: An example of this is that each writer owns more than one article in another table.
* __many-to-many__: An example of this is that more than one user can join more than one course.

Besides these relationships, __Laravel__ offers more of them, namely:
* __Has Many Through__
* __Polymorphic Relations__
* __Many-to-many Polymorphic__

The number of relationships that we will explain becomes 6 types. And we will build a simple content management system to explain all the relationships.

Do I need specific knowledge of __Eloquent__ before reading this? In the examples below, I have tried to explain everything as clearly as possible, without using a lot of __Eloquent's__ tricky functions and complex technologies. This means that prior knowledge is not strictly necessary. However, it is always best to learn the basics first, and then move on to more complex topics like relationships.

## One-To-One Relationship
Being the first and simplest basic relation offered by __Laravel__, they join two tables so that one row of the first table is associated with only one row of the other table.

To see this in action we'll start by creating a content management system,

Let's say each user can have one profile. In some cases you could store all profile information in the users table, but that wouldn't be ideal. Here, in our example, I want to create a separate table for them. For example, if we later want to transfer a profile to a different user, this will come in handy.

> By default, the Users table exists, and it does not matter which columns it will contain.

* Let's say we have a Users table with the following columns:
```PHP
Schema::create('users', function (Blueprint $table) {
  $table->id();
  $table->string('username');
  $table->string('email')->unique();
  $table->timestamps();
});
```

* We modify the ***`User.php`*** file.
```PHP
protected $fillable = ['username'];
```

* Let's create a ***`Profile Model`***.
```bash
php artisan make:model Profile -m
```

In a ***`one-to-one`*** relationship we have the freedom to choose one of these two methods:
* Add ***`user_id`*** to ***`profiles`*** table.
* Add a ***`profile_id`*** to the ***`users`*** table.

But as a result of our way of thinking, each user will have their own profile. Mostly this column that connects the two tables is always added to the second table, so we will add it to the profiles table as follows.

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

* We modify the ***`Profile.php`*** file.
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

* Let's go to the ***`User.php`*** file to set the new relationship.
```PHP
public function profile() {
    return $this->hasOne(Profile::class);
}
```

> Let's see how ***`hasOne`*** works.
```PHP
$this->hasOne(Profile::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* There are two ways to create a user and associate them with a relationship with their profile:

> We will first go to the ***`routes/web.php`*** file and add a new route so that we can test these ways.


1. Without using ***`function profile`***.
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
> We will open the browser and go to this link ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```
2. using ***`function profile`***.
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

> We will open the browser and go to this link again ***`http://127.0.0.1:8000/one-to-one`*** to find that the user was created successfully.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
```

* Let's go to the ***`Profile.php`*** file to set the inverse relationship.
```PHP
public function user() {
    return $this->belongsTo(User::class);
}
```

> Learn how ***`belongsTo`*** works.
```PHP
$this->belongsTo(User::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // OwnerKey By Default Id
);
```

> Let's say you want to give the relationship another name, like ***`admin`***, so we have to add a ***`foreignKey`***.
```PHP
public function admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```

> If you do not add a ***`foreignKey`*** when changing the relationship name, you will see this error.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/Attempt-to-read-property-X-on-null.png"
alt="Attempt to read property X on null"
caption="Attempt to read property X on null"
>}}

* Another way to create a profile and inversely associate it with the user who owns it is through the ***`user function`***.
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

> We will open the browser and go to this link again ***`http://127.0.0.1:8000/one-to-one`*** to find that the user was created successfully.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

## How to Optimize Eloquent query in Laravel?
When dealing with a large database with a lot of data inside it comes time to look at the matter from another point of view. It is not just a matter of fetching the data but how long you will need to fetch that data. How many queries will you make per page? To test this, we will download a very popular library called [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). It will help us to know all the inquiries that are made on each page.

* Let's do this command.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> Make sure that ***`APP_DEBUG=true`*** inside the ***`.env`*** file.
* Then we will create a new route to display all the users that were saved in the database while we performed the previous steps.
```PHP
Route::get('/users', function () {
    $users = \App\Models\User::all();
    return view('users.oneToOne', compact('users'));
});
```

* In the ***`views`*** folder, we also add another folder named ***`users`***, and inside it we add the ***`oneToOne.blade`***.php file, and we add this simple table to display the users.
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

* We now go to the browser and go to the following link ***`http://127.0.0.1:8000/users`*** to see what results will appear.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/users-view.png"
alt="Users View"
caption="Users View"
>}}

>  We will now find at the bottom of the page the ***`Laravel Debugbar`*** bar. When you open it, we will find that the number of queries on this page is “4”. And they are only 3 users, imagine with me if there are thousands or millions of users in the database, the time to load this page will be very slow. This is called the N+1 problem in Laravel or it is called Lazy Loading.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/laravel-debugbar-lazy-loading.png"
alt="laravel debugbar lazy loading"
caption="laravel debugbar lazy loading"
>}}

* To solve this problem, we modify the method of fetching users as follows.
```PHP
$users = \App\Models\User::with('profile')->get();
```

> As you can see, by adding the word ***`with`*** with the name of the relationship, this problem will be overcome, so the number of queries has been reduced from 4 to 2, and this matter will clearly see its effect if the database is of medium size or large, of course, and this is what is called Eager Loading.
{{< figure
src="/img/laravel10-one-to-one-relationship-in-arabic/laravel-debugbar-eager-loading.png"
alt="laravel debugbar eager loading"
caption="laravel debugbar eager loading"
>}}

> And we can do the same previous steps if we are going to use the relationship in reverse. An example of this.

* Then we will create a new route to display all the profiles that were saved in the database while we performed the previous steps.
```PHP
Route::get('/profiles', function () {
    $profiles = \App\Models\Profile::with('user')->get();
    return view('profiles.oneToOne', compact('profiles'));
});
```

* In the ***`views`*** folder, we also add another folder named ***`profiles`***, and inside it we add the ***`oneToOne.blade.php`*** file, and we add this simple table to display the users.
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

* We now go to the browser and go to the following link ***`http://127.0.0.1:8000/profiles`*** to see what results will appear.
> You will find that the results that will appear are similar to the previous results.

### Conclusion
This article is the start of a whole series on ***Laravel Eloquent Relationships*** - Relationships within ***Laravel***. In it, we have dealt with the ***One to One*** relationship in a simple way, and we did not spare you any information. God willing, in the next article, we will learn about the ***One To Many*** relationship.
