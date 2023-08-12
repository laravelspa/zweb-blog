---
title: "Laravel Eloquent One-To-One Relationship - Ultimate Guide 2023"
date: 2023-08-10
draft: false
slug: "laravel-eloquent-one-to-one-relationship-ultimate-guide-2023"
description: "Laravel one-to-one relationship is a database relationship where each row in one table can be associated with exactly one row in another table. It can be easily created and managed using the hasOne() and belongsTo() methods in Laravel."
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel Eloquent Relationships']
series_order: 1
---

Often the developer needs to interact with databases. And if you are using the Laravel framework, you should know about the most important feature of Laravel, which is called Eloquent, the object relationship diagram (ORM) that makes this process simple and easy.

__Laravel Eloquent__ is one of the main features in the __Laravel__ framework. This is due to its great support for defining, creating and managing relationships between different tables. In this series of articles I will show you how to create and use __Eloquent relationships__.

Noting that you can start without any prior knowledge of relationships.

{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023.png"
alt="laravel one to one relationship"
caption="laravel one to one relationship"
>}}

It is necessary as a professional programmer to understand the types of relationships, but before that you must ask yourself an important question, what are the relationships in the first place?

## What are relationships in databases?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-are-relationships-in-database.png"
alt="What are relationships in databases?"
caption="What are relationships in databases?"
>}}
When working with tables in a database that have relationships between them, we can describe these relationships as connections between those tables. This helps you organize and structure data effortlessly allowing for faster data reading and processing.

## What types of relationships are there in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-types-of-relationships-are-there-in-laravel.png"
alt="What types of relationships are there in Laravel?"
caption="What types of relationships are there in Laravel?"
>}}
There are three main types of relationships in databases that emerge in practice:
* __one-to-one__: It is a single record associated with only one record. An example of this is that each user has one profile in another table.
* __one-to-many__: It is the association of only one record with more than one other record. An example of this is that each writer has more than one article.
* __many-to-many__: It is the association of more than one record with more than one other record. An example of this is that more than one user can join more than one session.

Besides these relationships, __Laravel__ offers more relationships, namely:
* __Has Many Through__
* __Polymorphic Relations__
* __Many-to-many Polymorphic__

The number of relationships that we will explain becomes 6 types. And we're going to build a simple content management system to explain all of those relationships.

Do I need specific knowledge of Eloquent before reading this?

In the examples below, I have tried to explain everything as clearly as possible, without using too many Eloquent tricky functions and complex techniques. This means that prior knowledge is not strictly necessary.

However, it is always best to learn the basics first and then pursue more complex topics such as relationships.

## How to create a One-To-One relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-create-a-One-To-One-relationship-in-laravel.png"
alt="How to create a One-To-One relationship in Laravel?"
caption="How to create a One-To-One relationship in Laravel?"
>}}
Being the first and simplest basic relationship offered by Laravel, it joins two tables such that one row of the first table is associated with only one row of the other table, or the same table.

To see this in action, we'll start by creating a content management system.

Let's say each user has his own single profile. In some cases, you can store all profile information in the Users table. But this will not be ideal.

In our example, we want to create a separate table for personal profiles. If we later want to transfer a profile from one user to another, this will be available.

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

* We create ***`Profile Model`*** with its table.
```bash
php artisan make:model Profile -m
```

In a ***`One-to-One`*** relationship we have the freedom to choose one of these two methods:
* Add ***`user_id`*** in the ***`profiles`*** table.
* Add ***`profile_id`*** in the ***`users`*** table.

> Mostly this column that joins the two tables is always added to the second table, so we will add it to the profiles table as follows.
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

* Let's go to the ***`User.php`*** file to set the relationship.
```PHP
public function profile() {
    return $this->hasOne(Profile::class);
}
```

> Let's see how ***`hasOne`*** works
```PHP
$this->hasOne(Profile::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // localKey => Primary Key In Parent Table By Default is Id
);
```

* Let's go to the ***`Profile.php`*** file to set the inverse relationship.
```PHP
public function user() {
    return $this->belongsTo(User::class);
}
```

> Let's find out how ***`belongsTo`*** works.
```PHP
$this->belongsTo(User::class,
  'user_id' // foreignKey By Default Parent Model + Promary Key
  'id' // OwnerKey By Default Id
);
```

> Let's say you want to name the relationship something like ***`admin`***, we need to add ***`foreignKey`***.
```PHP
public function admin() {
    return $this->belongsTo(User::class, 
      'user_id' // You must add foreignKey
    );
}
```

> If you do not add ***`foreignKey`*** when changing the relationship name, you will see this error.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/Attempt-to-read-property-X-on-null.png"
alt="Attempt to read property X on null"
caption="Attempt to read property X on null"
>}}

## How to insert data in one to one relationship in database?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-insert-data-in-one-to-one-relationship-in-database.png"
alt="How to insert data in one to one relationship in database?"
caption="How to insert data in one to one relationship in database?"
>}}
After we created a One-To-One relationship between both Users table and Profiles table and added ***`hasOne`*** inside ***`User Model`***, also we added the inverse relationship inside ***` Profile Model`*** by adding ***`belongsTo`*** to it.

The time has come to find out how the data is saved in the database while we use this relationship.
And what are the methods used for that.

These methods are divided into three main ways:
1. Without using **function profile**.
2. By using **function profile**.
3. By using the inverse **function user**.

### 1. Without using **function profile**.
* We go first to the file ***`routes/web.php`*** and add a new link so that we can test these methods.
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
* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```

### 2. By using **function profile**.
* We first go to the file ***`routes/web.php`*** and modify this link.
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

* We open the browser and go to this link again ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
```

### 3. By using the inverse **function user**.
* We first go to the file ***`routes/web.php`*** and modify this link.
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

* We open the browser and go to this link again ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

## How do you get data into a One-To-One relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-do-you-get-data-into-a-One-To-One-relationship-in-laravel.png"
alt="How do you get data into a One-To-One relationship in Laravel?"
caption="How do you get data into a One-To-One relationship in Laravel?"
>}}
We have seen how data is saved in several different ways inside the database using the One-To-One relationship in Laravel.

But how is the data get from the database?
There are several ways in which data can be get from the database.

These methods are divided into two main ways:

### while get users data.
While collecting user data, we will obtain their personal profiles.
After that, the matter is divided in displaying that data between two forms, not a third.
Determines the type of application you are working on, the theme used:
* First: an application based on web routes.
* Secondly: an application that depends on API routes.


#### First: based on ***`Web Routes`***.
1. We first go to the file ***`routes/web.php`*** and make the following modifications.
```PHP
Route::get('/users', function () {
    $users = User::with(['profile'])->get();
    return view('users.list', compact('users'));
});
```
> If we check the Response for this command, we will find that all data has been obtained from the database.
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

Here you can choose only the columns we need in the following form.
```PHP
Route::get('/users', function () {
    $users = User::with(['profile:firstname,lastname,user_id'])->get();
    return view('users.list', compact('users'));
});
```

> If we now check again, you will find that the data size has been reduced.
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

> Here we see the difference between the two cases in the size of the processed data, and the matter increases if the data is much larger than that.
> When choosing specific columns from relationships, you must choose the ***foreignKey*** column, because without choosing it, the data will not be returned correctly from the database.

2. Inside the folder ***`views`*** we add another folder named ***`users`*** and inside it we add the file ***`list.blade.php`*** and we add this simple table to display the users inside it .
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

3. Open your browser and go to the following link ***`http://127.0.0.1:8000/users`*** to see what results will appear.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png"
alt="Laravel One To One Relationship - Users Table"
caption="Users Table"
>}}

#### Second: based on ***`Api Routes`***.

> What is API Resources?

Simply it is an intermediate layer between __Eloquent__ and between the response of the API and the conversion of that data obtained from the database into JSON with the possibility of specifying specific data without others or manipulating that data.

1. We will create an API Resource for User, Profile. Execute this command at the command prompt.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
```
2. Go to the following path ***`App/Http/Resources`*** and modify both:

* File ***`ProfileResource.php`***.
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

* The file ***`UserResource.php`***.
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

3. Go to your ***`routes/api.php`*** file and add a new link.
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

4. Open your browser and go to the following link ***`http://127.0.0.1:8000/api/users`*** to see what results will appear.
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

> We also see here that we have obtained the required data specified within the API Resources files only.

### while get profiles data.
While obtaining profile data, we will obtain each user associated with those profiles.
After that, the matter is divided in displaying that data between two forms, not a third.
Determines the type of application you are working on, the theme used:
* First: an application based on web routes.
* Second: an application based on api routes.


#### First: Based on ***`Web Routes`***.
1. We first go to the file ***`routes/web.php`*** and make the following modifications.
```PHP
Route::get('/profiles', function () {
    $profiles = Profile::with('user')->get();
    return view('profiles.list', compact('profiles'));
});
```

> If we check the Response for this command, we will find that all data has been obtained from the database.
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

 Here we can select only the columns we need as follows.
```PHP
Route::get('/profiles', function () {
    $profiles = Profile::with('user:username,id')->get();
    return view('profiles.list', compact('profiles'));
});
```

> If we now check again, we will find that the data size has been reduced.
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

>When selecting specific columns in relationships, you must choose ***id*** while choosing the ***user*** relationship, because without choosing it, the user will not be returned with the profile.

2. Inside the folder ***`views`*** we add another folder named ***`profiles`*** and inside it we add the file ***`list.blade.php`*** and we add this simple table to display the profiles Inside.
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

3. Open your browser and go to the following link ***`http://127.0.0.1:8000/profiles`*** to see what results will appear.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png"
alt="Laravel One To One Relationship - Profiles Table"
caption="Profiles Table"
>}}


#### Second: Based on ***`Api Routes`***.

1. Go to the following directory ***`App/Http/Resources`*** and edit the file ***`ProfileResource.php`***:
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

2. Go to your ***`routes/api.php`*** file and add a new link.
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

3. Open your browser and go to the following link ***`http://127.0.0.1:8000/api/profiles`*** to see what results will appear.
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

## How to improve Eloquent queries in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-improve-Eloquent-queries-in-laravel.png"
alt="How to improve Eloquent queries in Laravel?"
caption="How to improve Eloquent queries in Laravel?"
>}}
When dealing with a large database with a lot of data inside, here is a way to look at the matter from a different perspective.

It is not just about getting the data but how long it will take to get that data.
How many queries will be executed per page.

To test this matter, we will download a very famous library called
[Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). It will help us to know all the queries that are executed on each page.

*Let's run this command.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> Make sure that ***`APP_DEBUG=true`*** is inside the ***`.env`*** file.

* The difference between the two cases:
```PHP
$users = User::all();
$users = User::with('profile')->get();
```

* We open the browser and go to the following link ***`http://127.0.0.1:8000/users`*** to see what results will appear in the library bar.

1. __(Lazy Loading)__ - Get data without using ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png"
alt="laravel debugbar lazy loading"
caption="laravel debugbar lazy loading"
>}}

At the bottom of the page, we will now find a bar for the ***`Laravel Debugbar`*** library. When you open it, we will find that it works with a lot of data.
The thing that interests us here is the number of queries on this page, and as shown, they are __4__ queries.

And we only bring 3 users, imagine with me if there are tens of thousands or millions of users inside this database, the time to load this page will be very slow because of the huge number of queries.

This is called the N+1 problem in Laravel or it is called Lazy Loading.

> Imagine, with 1000 users, 1001 database queries will be requested on this page only. This is a consumption of server resources. And also proof that your code is not professional.

2. __(Eager Loading)__ - Get data without using ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png"
alt="laravel debugbar eager loading"
caption="laravel debugbar eager loading"
>}}

As you can see, by adding the word ***`with`*** with the name of the relationship, each user's data will be fetched with their own profile without a problem, N+1.

So the number of queries was reduced from 4 to 2 only, and this matter will clearly see its effect if this database, as we said, is of medium size or large, and this is what is called Eager Loading.

## How to update one to one relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-update-one-to-one-relationship-in-laravel.png"
alt="How to update one to one relationship in laravel?"
caption="How to update one to one relationship in laravel?"
>}}
### Update data from the user's discretion.

1. Using ***`push function`***.
* We go first to the file ***`routes/web.php`*** and add this link.
```PHP
Route::get('/users/update', function () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe Updated';
    $user->profile->lastname = 'Doe Updated';
    $user->push();
    return response()->json($user);
});
```
* We open the browser and go to this new link ***`http://127.0.0.1:8000/users/update`*** to find that the user and profile have been updated successfully.
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

2. By Using ***`update function`***.
* We first go to the file ***`routes/web.php`*** and modify this link.
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
* We open the browser and go to this new link ***`http://127.0.0.1:8000/users/update`*** to find that the user and profile have been updated successfully.
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

### Update data by using profile.
1. By Using ***`push function`***.
* We go first to the file ***`routes/web.php`*** and add this link.
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
* We open the browser and go to this new link ***`http://127.0.0.1:8000/profiles/update`*** to find that the user and profile have been updated successfully.
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

2. By Using ***`update function`***.
* We first go to the file ***`routes/web.php`*** and modify this link.
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

* We open the browser and go to this new link ***`http://127.0.0.1:8000/profiles/update`*** to find that the user and profile have been updated successfully.
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

## How to delete data from one to one relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-delete-data-from-one-to-one-relationship-in-laravel.png"
alt="How to delete data from one to one relationship in Laravel?"
caption="How to delete data from one to one relationship in Laravel?"
>}}
### Delete data from the user's.
* We go first to the file ***`routes/web.php`*** and add this link.
```PHP
Route::get('/users/profile/delete', function () {
    $user = User::with('profile')->find(1);
    $user->profile()->delete();
    return response()->json($user);
});
```
* We will open the browser and go to this new link ***`http://127.0.0.1:8000/users/profile/delete`*** to find that the user's profile has been deleted successfully.
```json
{
  "id": 1,
  "username": "Joun Doe",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:07:38.000000Z",
  "profile": null
}
```
> Refresh the page twice to show that this user's profile has been deleted.

### Delete data from profile.
* We go first to the file ***`routes/web.php`*** and add this link.
```PHP
Route::get('/profiles/user/delete', function () {
    $profile = Profile::with('user')->findOrFail(2);
    $profile->delete();
    $profile->user()->delete();
});
```
* We will open the browser and go to this link ***`http://127.0.0.1:8000/profiles/user/delete`*** to find that both the user and the profile have been deleted successfully.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png"
alt="Rcord has deleted"
caption="Rcord has deleted"
>}}

## Conclusion
This article is the start of a whole series on ***Laravel Eloquent Relationships*** - Relationships within ***Laravel***.
We have covered the ***One TO One*** relationship in a complete way.
  We did not spare you any information, and God willing, in the next explanation, we will learn about the ***One To Many*** relationship.

- You can find the repo for this series on github here
{{< github repo="laravelspa/laravel-relations" >}}