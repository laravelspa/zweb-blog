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

Often, developers need to interact with databases. If you are using the Laravel framework, you should know about one of its most important features: Eloquent, an object-relational mapper (ORM) that makes this process simple and easy.

__Laravel Eloquent__ is one of the main features in the __Laravel framework__. This is due to its great support for defining, creating, and managing relationships between different tables. In this series of articles, I will show you how to create and use __Eloquent relationships__.

It is important to note that you can start using Eloquent without any prior knowledge of relationships.

{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023.png"
alt="laravel one to one relationship"
caption="laravel one to one relationship"
>}}

As a professional programmer, it is necessary to understand the types of relationships. However, before that, you must ask yourself an important question: what are relationships in the first place?

## What are relationships in databases?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-are-relationships-in-database.png"
alt="What are relationships in databases?"
caption="What are relationships in databases?"
>}}
When working with tables in a database that have relationships between them, we can describe these relationships as links between those tables. This helps you organize and structure data effortlessly, allowing for faster data reading and processing.

## What types of relationships are there in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/what-types-of-relationships-are-there-in-laravel.png"
alt="What types of relationships are there in Laravel?"
caption="What types of relationships are there in Laravel?"
>}}
There are three main types of relationships in databases that are commonly used in practice:

* __one-to-one__:  A one-to-one relationship means that one record in one table can only be related to one record in another table. For example, a customer table might have a one-to-one relationship with an address table. This means that each customer can only have one address, and each address can only be associated with one customer.

* __one-to-many__: A one-to-many relationship means that one record in one table can be related to many records in another table. For example, an order table might have a one-to-many relationship with a product table. This means that one order can have many products, but each product can only be associated with one order.

* __many-to-many__:  A many-to-many relationship means that many records in one table can be related to many records in another table. For example, a student table might have a many-to-many relationship with a course table. This means that many students can take many courses, and many courses can be taken by many students.

Besides these relationships, __Laravel__ offers more relationships, namely:

* __Has Many Through__: This relationship allows you to relate a model to another model through a third model. For example, a user model might have a has many through relationship with a post model, where the third model is a category model. This means that a user can have many posts, where each post belongs to a category.

* __Polymorphic Relations__: This relationship allows you to relate a model to multiple other models of different types. For example, a comment model might have a polymorphic relationship with a model, where the model could be a blog post, a product, or a user. This means that a comment can be associated with any of these three types of models.

* __Many-to-many Polymorphic__: This relationship is a combination of the has many through and polymorphic relationships. It allows you to relate a model to many other models of different types, where the relationship is mediated by a third model. For example, a user model might have a many-to-many polymorphic relationship with a model, where the third model is a role model. This means that a user can have many roles, where each role could be a user, a product, or a blog post.

The number of relationships that we will explain is 6 types. We will build a simple content management system to explain all of those relationships.

> Do I need specific knowledge of Eloquent before reading this?

In response to your question about whether you need specific knowledge of Eloquent before reading this, I would say that it is not strictly necessary, but it would be helpful. The examples in the article do not use any complex Eloquent methods or techniques, but it would be helpful to have a basic understanding of how Eloquent works before reading the article.

If you are new to Laravel, I would recommend reading the Laravel documentation: https://laravel.com/docs/ before reading this article. The documentation provides a good overview of Eloquent and how it works.

Once you have a basic understanding of Eloquent, you should be able to follow the examples in the article without any problems. However, if you get stuck, you can always refer to the documentation for more help.


In the examples below, I have tried to explain everything as clearly as possible, without using too many tricky Eloquent methods and complex techniques. This means that prior knowledge is not strictly necessary. However, it is always best to learn the basics first and then pursue more complex topics such as relationships.

## How to create a One-To-One relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-create-a-One-To-One-relationship-in-laravel.png"
alt="How to create a One-To-One relationship in Laravel?"
caption="How to create a One-To-One relationship in Laravel?"
>}}
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
After we created a ***one-to-one relationship*** between the ***users*** table and the ***profiles*** table, and added the ***`hasOne()`*** method to the ***User*** model, and the ***belongsTo()*** method to the ***Profile*** model, it's time to find out how the data is saved in the database when we use this relationship. And what are the methods used for that?

These methods are divided into three main ways:
1. Without using **method profile**.
2. By using **method profile**.
3. By using the inverse **method user**.

The best method to use depends on the specific needs of your application. 
If you only need to save the profile associated with the user, then the first method is the simplest option. 
If you need to get, update, or delete the profile, then the second method is a better option. 
If you need to get, update, or delete the user, then the third method is a better option.

### 1. Without using **method profile**.
* We first go to the ***`routes/web.php`*** file and add a new route so that we can test these method.
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

* We opened the browser and went to the link ***`http://127.0.0.1:8000/one-to-one`***. To our satisfaction, the user had been created successfully.
```json
{
  "username": "John Doe",
  "firstname": "John",
  "lastname": "Doe"
}
```

### 2. By using **method profile**.
* We first go to the ***`routes/web.php`*** file and edit this route.
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

* We open the browser again and go to this link: ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "Tom Cruz",
  "firstname": "Tom",
  "lastname": "Cruz"
}
```

### 3. By using the inverse **method user**.
* We first go to the ***`routes/web.php`*** file and update this route.
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

* We open the browser again and go to this link: ***`http://127.0.0.1:8000/one-to-one`*** to find that the user has been created successfully.
```json
{
  "username": "Adam Smith",
  "firstname": "Adam",
  "lastname": "Smith"
}
```

## How can you retrieve data from a one-to-one relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-can-you-retrieve-data-from-a-one-to-one-relationship-in-larave.png"
alt="How can you retrieve data from a one-to-one relationship in Laravel?"
caption="How can you retrieve data from a one-to-one relationship in Laravel?"
>}}
We have seen how data is saved in several different ways in the database using the one-to-one relationship in Laravel. But how do we retrieve the data from the database? There are several ways to retrieve data from the database.

These methods can be divided into two main categories:

* Direct retrieval: This method involves directly querying the database for the data that you need. This is the most efficient way to retrieve data, but it can be difficult to use if you are not familiar with SQL.

* Eloquent ORM: Laravel's Eloquent ORM provides a more user-friendly way to retrieve data from the database. Eloquent ORM uses a set of methods to map database tables to objects in PHP. This makes it easy to retrieve data from the database without having to know SQL.
In this tutorial, we will focus on using Eloquent ORM to retrieve data from the database.

### While retrieving users data:
While collecting user data, we will obtain their personal profiles. After that, the matter of displaying that data is divided into two forms, not a third. The type of application you are working on and the theme used will determine which form to use.

* First: an application that uses web routes.
* Second: an application that depends on API routes.

#### First: Depends on ***`Web Routes`***.
1. We first go to the ***`routes/web.php`*** file and make the following changes:
```PHP
Route::get('/users', method () {
    $users = User::with(['profile'])->get();
    return view('users.list', compact('users'));
});
```

> If we check the response for this command, we will find that all data has been obtained from the database.
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

Here, you can choose only the columns that you need in the following form:
```PHP
Route::get('/users', method () {
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

> Here, we see the difference between the two cases in the size of the processed data. The matter increases significantly if the data is much larger than that.

> When choosing specific columns from relationships, you must always choose the ***`foreign key`*** column. Without choosing it, the data will not be returned correctly from the database.

2. Inside the ***`views`*** folder, we add another folder named ***`users`***. Inside the users folder, we add the file ***`list.blade.php`***. In ***`list.blade.php`***, we add this simple table to display the users:
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

3. Open your browser and go to the following URL ***`http://127.0.0.1:8000/users`*** to see what results will appear.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png"
alt="Laravel One To One Relationship - Users Table"
caption="Users Table"
>}}

#### Second: Depends on ***`Api Routes`***.

> What are API Resources?

Simply, it is an intermediate layer between Eloquent and the API response, converting data obtained from the database into JSON with the ability to specify specific data without others or manipulate that data.

1. We will create an API Resource for users and profiles. Execute this command at the command prompt.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
```

2. Go to the following path: ***`App\Http\Resources`*** and edit both:

* The file ***`ProfileResource.php`***.
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

* The file ***`UserResource.php`***.
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

3. Go to your ***`routes/api.php`*** file and add a new route.
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

4. Open your browser and go to the following URL ***`http://127.0.0.1:8000/api/users`*** to see what results will appear.
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

### While retrieving profiles data:
While obtaining profile data, we will obtain each user associated with those profiles.
After that, the data can be displayed in two forms, not three.
The type of application you are working on and the theme used will determine which form of data display is most appropriate.

* First: an application that uses web routes.
* Second: an application that depends on API routes.
#### First: Depends on ***`Web Routes`***.
1. We first go to the ***`routes/web.php`*** file and make the following changes:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user')->get();
    return view('profiles.list', compact('profiles'));
});
```
> If we check the response for this command, we will find that all data has been obtained from the database.
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

Here, you can choose only the columns that you need in the following form:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user:username,id')->get();
    return view('profiles.list', compact('profiles'));
});
```

> If we now check again, you will find that the data size has been reduced.
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

> Here, we see the difference between the two cases in the size of the processed data. The matter increases significantly if the data is much larger than that.

> When selecting specific columns in relationships, you must include the ***`id`*** field when choosing the ***`user`*** relationship. Otherwise, the user will not be returned with the profile.

2. Inside the ***`views`*** folder, we add another folder named ***`profiles`***. Inside the profiles folder, we add the file ***`list.blade.php`***. In ***`list.blade.php`***, we add this simple table to display the profiles:
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

3. Open your browser and go to the following URL ***`http://127.0.0.1:8000/profiles`*** to see what results will appear.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png"
alt="Laravel One To One Relationship - Profiles Table"
caption="Profiles Table"
>}}

#### Second: Depends on ***`Api Routes`***.

1. Go to the following path: ***`App\Http\Resources`*** and edit the file ***`ProfileResource.php`***:
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

2. Go to your ***`routes/api.php`*** file and add a new route.
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

3. Open your browser and go to the following URL ***`http://127.0.0.1:8000/api/profiles`*** to see what results will appear.
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

When dealing with a large database with a lot of data, it is important to consider performance. This means not only how long it takes to get the data, but also how many queries are executed per page.

To test the performance of your application, you can use a library called [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). Debugbar will show you all the queries that are executed on each page, as well as how long each query takes to execute. This information can help you to identify performance bottlenecks and optimize your application.

* Let's run this command.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> Make sure that ***`APP_DEBUG=true`*** is inside the ***`.env`*** file.

* The difference between the two cases is as follows:
```PHP
$users = User::all();
$users = User::with('profile')->get();
```

* We open the browser and go to the following URL: ***`http://127.0.0.1:8000/users`*** to see what results will appear in the library bar.

1. __(Lazy Loading)__ - Retrieve data without using ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png"
alt="laravel debugbar lazy loading"
caption="laravel debugbar lazy loading"
>}}

At the bottom of the page, we will now find a bar for the __Laravel Debugbar library__. When you click on it, we will find that it works with a lot of data. The thing that interests us here is the number of SQL queries on this page, and as shown, they are __4__ queries.

And we only retrieved 3 users, imagine with me if there are tens of thousands or millions of users inside this database, the time to load this page will be very slow because of the huge number of queries.

This is called the N+1 problem in Laravel. It is caused by lazy loading relationships, which means that the associated data is not loaded until it is actually needed. In this case, we are lazy loading the posts relationship for each user. This means that we are making a separate query for each user to get their profiles.

If there are 1000 users, we will make 1001 queries: 1 query to get the users, and 1000 queries to get their profiles. This is a waste of server resources and can make your application slow.

To solve the N+1 problem, you can eager load the relationships. This means that you will load the associated data when you first retrieve the data. In this case, you would use the with() method to eager load the profiles relationship. This would only make one query to get the users and their profiles.

Eager loading can improve the performance of your application by reducing the number of queries. It is a good practice to eager load relationships whenever possible.

2. __(Eager Loading)__ - Retrieve data using ***`with`***.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png"
alt="laravel debugbar eager loading"
caption="laravel debugbar eager loading"
>}}

As you can see, by adding the with() method with the name of the relationship, each user's data will be fetched with their own profile without the N+1 problem.

So the number of queries was reduced from 4 to 2 only. This will clearly have an effect if this database is of medium size or large. This is what is called eager loading.

## How to update a one-to-one relationship in Laravel?
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-update-one-to-one-relationship-in-laravel.png"
alt="How to update one to one relationship in laravel?"
caption="How to update one to one relationship in laravel?"
>}}
### Update data using User Model.

1. Using ***`push method`***.
* We first go to the ***`routes/web.php`*** file and add this route:
```PHP
Route::get('/users/update', method () {
    $user = User::with('profile')->find(1);
    $user->username = 'John Doe Updated';
    $user->profile->lastname = 'Doe Updated';
    $user->push();
    return response()->json($user);
});
```

* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/users/update`*** to find that the user and profile have been updated successfully.
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

2. By Using ***`update method`***.
* We first go to the file ***`routes/web.php`*** and edit this route.
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

* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/users/update`*** to find that the user and profile have been updated successfully.
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

### Update data using Profile Model.
1. By Using ***`push method`***.
* We first go to the ***`routes/web.php`*** file and add this route:
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

* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/profiles/update`*** to find that the user and profile have been updated successfully.
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

2. By Using ***`update method`***.
* We first go to the ***`routes/web.php`*** file and edit this route:
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

* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/profiles/update`*** to find that the user and profile have been updated successfully.
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
### Delete data using User Model.
* We first go to the ***`routes/web.php`*** file and add this route:
```PHP
Route::get('/users/profile/delete', method () {
    $user = User::with('profile')->find(1);
    $user->profile()->delete();
    return response()->json($user);
});
```

* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/users/profile/delete`*** to find that the profile have been deleted successfully.
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

### Delete data Using Profile Model.
* We first go to the ***`routes/web.php`*** file and add this route:
```PHP
Route::get('/profiles/user/delete', method () {
    $profile = Profile::with('user')->findOrFail(2);
    $profile->delete();
    $profile->user()->delete();
});
```
* We open the browser and navigate to the new URL ***`http://127.0.0.1:8000/profiles/user/delete`***. We see that both the user and the profile have been deleted successfully.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png"
alt="Rcord has deleted"
caption="Rcord has deleted"
>}}

## Conclusion
This article is the start of a whole series on __Laravel Eloquent Relationships__ - Relationships within __Laravel__. We have covered the __One TO One relationship__ in a complete way. We did not spare you any information, and God willing, in the following explanation, we will learn about the __One To Many relationship__.

- You can find the repo for this series on github here
{{< github repo="laravelspa/laravel-relations" >}}