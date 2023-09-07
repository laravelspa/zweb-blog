---
title: "Laravel Eloquent One-To-Many Relationship - Ultimate Guide 2023"
date: 2023-08-12
draft: false
slug: "laravel-eloquent-one-to-many-relationship-ultimate-guide-2023"
description: "A Laravel one-to-many relationship is a database relationship where each row in one table can be linked to more than one row in another table. It can be easily created and manipulated using hasMany() and belongsTo() methods in Laravel."
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'eloquent relationships']
series: ['Laravel Eloquent Relationships']
series_order: 2
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

## How to insert data in a one-to-many relationship in the database?
![How to insert data in a one-to-many relationship in the database?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-insert-data-in-one-to-many-relationship-in-database.png "How to insert data in a one-to-many relationship in the database?")

After we created a One-To-Many relationship between both Users table and Posts table and added ***`hasMany`*** inside ***`User Model`***, also we added the inverse relationship inside ***`Post Model`*** by adding ***`belongsTo`*** to it.

  The time has come to find out how the data is saved in the database while we use this relationship.
What are the methods used in this?


These methods are divided into three basic methods:
1. Without using **function post**.
2. By using **function post**.
3. By using the inverse relationship **function user**.
### 1. Without using **function post**.
Here there are two scenarios:
* First: Add only one post for the user.
* Second: Add more than one post for the user.
#### First: Add only one post for the user.
* We first go to the ***`routes/web.php`*** file and add a new route so that we can test these methods.
```PHP
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
    $user = User::find(1);
    Post::create([
        'user_id' => $user->id,
        'title' => 'Post title 1',
        'body' => 'Post body 1',
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts
    ]);
});
```

* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-many`*** to find that one post has been successfully added to user NoisyId No. 1.
```json
{
  "username": "John Doe",
  "post": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-08-05T03:18:22.000000Z",
      "updated_at": "2023-08-05T03:18:22.000000Z"
    }
  ]
}
```

#### Second: Add more than one post for the user.
* We first go to the ***`routes/web.php`*** file and edit this route.
```PHP
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
   $user = User::find(2);
    Post::insert(
        [
            [
                'user_id' => $user->id,
                'title' => 'Post title 2',
                'body' => 'Post body 2',
            ],
            [
                'user_id' => $user->id,
                'title' => 'Post title 3',
                'body' => 'Post body 3',
            ],
        ]
    );
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts
    ]);
});
```

* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-many`*** to find that 2 posts have been successfully added to the user with ID No. 2.
```json
{
  "username": "Tom Cruz",
  "post": [
    {
      "id": 2,
      "title": "Post title 2",
      "body": "Post body 2",
      "user_id": 2,
      "created_at": null,
      "updated_at": null
    },
    {
      "id": 3,
      "title": "Post title 3",
      "body": "Post body 3",
      "user_id": 2,
      "created_at": null,
      "updated_at": null
    }
  ]
}
```

### 2. By using **function post**.
Here also there are two scenarios:
First: Add only one post for the user
Second: Add more than one post for the user
#### First: Add only one post for the user
* We first go to the ***`routes/web.php`*** file and edit this route.
```PHP
use App\Models\User;
---
Route::get('/one-to-many', function () {
    $user = User::find(3);
    $user->posts()->create([
      'title' => 'Post title 4',
      'body' => 'Post body 4',
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts,
    ]);
});
```

* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-many`*** to find that one post has been successfully added to the user with ID No. 3.
```json
{
  "username": "Adam Smith",
  "post": [
    {
      "id": 4,
      "title": "Post title 4",
      "body": "Post body 4",
      "user_id": 3,
      "created_at": "2023-08-05T03:37:55.000000Z",
      "updated_at": "2023-08-05T03:37:55.000000Z"
    }
  ]
}
```

#### Second: Add more than one post for the user
* We first go to the ***`routes/web.php`*** file and edit this route.
```PHP
use App\Models\User;
---
Route::get('/one-to-many', function () {
   $user = User::find(1);
    $user->posts()->createMany([
        [
            'title' => 'Post title 5',
            'body' => 'Post body 5',
        ],
        [
            'title' => 'Post title 6',
            'body' => 'Post body 6',
        ]
    ]);
    return response()->json([
        'username' => $user->username,
        'post' => $user->posts,
    ]);
});
```

* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-many`*** to find that 2 posts have been successfully added to the user with ID No. 1.

{{< alert >}}
In addition to these posts, there is another post that was added in a previous step, and therefore there should be 3 posts for this user. This is actually the data obtained from the database in the following response.
{{< /alert >}}

```json
{
  "username": "John Doe",
  "post": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-08-05T03:18:22.000000Z",
      "updated_at": "2023-08-05T03:18:22.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 3,
      "created_at": "2023-08-05T03:42:27.000000Z",
      "updated_at": "2023-08-05T03:42:27.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 3,
      "created_at": "2023-08-05T03:42:27.000000Z",
      "updated_at": "2023-08-05T03:42:27.000000Z"
    }
  ]
}
```

### 3. By using the inverse relationship **function user**.
* We first go to the ***`routes/web.php`*** file and edit this route.
```PHP
use App\Models\User;
use App\Models\Post;
---
Route::get('/one-to-many', function () {
    $user = User::find(2);
    $post = new Post([
        'title' => 'Post title 7',
        'body' => 'Post body 7',
    ]);

    $post->user()->associate($user)->save();
    return response()->json([
        'username' => $post->user->username,
        'title' => $post->title,
        'body' => $post->body,
    ]);
});
```

* We open the browser and go to this link ***`http://127.0.0.1:8000/one-to-many`*** to find that a post has been successfully added to the user with ID No. 2.
```json
{
  "username": "Tom Cruz",
  "title": "Post title 7",
  "body": "Post body 7"
}
```

## How do you retrieve data from one to many relationship in Laravel?
![How do you retrieve data from one to many relationship in Laravel?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-do-you-get-data-into-a-Many-To-One-relationship-in-laravel.png "How do you retrieve data from one to many relationship in Laravel?")
We saw how data is saved in multiple different ways within the database using the One-To-Many relationship in Laravel.
But how to fetch data from the database?
There are several ways in which data can be fetched from a database.
These methods are divided into two basic methods:

### Through the user ***`User Model`***
This method is divided into two sub-methods depending on how the data is organized after it is fetched from the data base

#### Without using ***`API Resources`***.
1. We go first to the ***`routes/web.php`*** file so that we can test these methods and make the following modifications.
```PHP
use App\Models\User;
---
Route::get('/users', function () {
    $users = User::with(['profile', 'posts'])->get();
    return view('users.list', compact('users'));
});
```
Here we can choose the columns for each relationship as follows.
```PHP
use App\Models\User;
---
Route::get('/users', function () {
    $users = User::with(['profile:id,firstname,lastname,user_id', 'posts:title,user_id'])->get();
    return view('users.list', compact('users'));
});
```

In order to know the difference between selecting or not selecting columns from the database, let us see the specific response to each case.
* In the first case, with all data retrieved without exception, we find that ***`$users`*** contains that data.
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
    },
    "posts": [
      {
        "id": 1,
        "title": "Post title 1",
        "body": "Post body 1",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      },
      {
        "id": 5,
        "title": "Post title 5",
        "body": "Post body 5",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      },
      {
        "id": 6,
        "title": "Post title 6",
        "body": "Post body 6",
        "user_id": 1,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
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
    },
    "posts": [
      {
        "id": 2,
        "title": "Post title 2",
        "body": "Post body 2",
        "user_id": 2,
        "created_at": null,
        "updated_at": null
      },
      {
        "id": 3,
        "title": "Post title 3",
        "body": "Post body 3",
        "user_id": 2,
        "created_at": null,
        "updated_at": null
      },
      {
        "id": 7,
        "title": "Post title 7",
        "body": "Post body 7",
        "user_id": 2,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
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
    },
    "posts": [
      {
        "id": 4,
        "title": "Post title 4",
        "body": "Post body 4",
        "user_id": 3,
        "created_at": "2023-08-07T06:23:41.000000Z",
        "updated_at": "2023-08-07T06:23:41.000000Z"
      }
    ]
  }
]
```

* In the second case, when specifying the exact required columns from the database, we find that the response is as follows.
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
      "user_id": 1
    },
    "posts": [
      {
        "title": "Post title 1",
        "user_id": 1
      },
      {
        "title": "Post title 5",
        "user_id": 1
      },
      {
        "title": "Post title 6",
        "user_id": 1
      }
    ]
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
      "user_id": 2
    },
    "posts": [
      {
        "title": "Post title 2",
        "user_id": 2
      },
      {
        "title": "Post title 3",
        "user_id": 2
      },
      {
        "title": "Post title 7",
        "user_id": 2
      }
    ]
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
      "user_id": 3
    },
    "posts": [
      {
        "title": "Post title 4",
        "user_id": 3
      }
    ]
  }
]
```

> Here we see the difference between the two cases in the size of the data that was processed, and the matter increases if the size of the data is much larger than that.
> When selecting specific columns in relationships, you must choose ***foreignKey*** because without choosing it, the data will not be returned correctly from the database.

2. We go to the following path ***`resources/views/users`*** and modify this file ***`list.blade.php`*** to display the users and their profiles. Also, what interests us here is displaying the posts for each user.

```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
            <th style="text-align: center">Posts</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($users as $user)
            <tr>
                <td>{{ $user->username }}</td>
                <td>{{ $user->profile->firstname }}</td>
                <td>{{ $user->profile->lastname }}</td>
                <td>
                    <ul>
                        @foreach ($user->posts as $post)
                            <li>{{ $post->title }}</li>
                        @endforeach
                    </ul>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. Open the browser and go to the following link ***`http://127.0.0.1:8000/users`*** to see what results will appear.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}


#### By using ***`API Resources`***.
1. We will create an API Resource for the Post form by executing this command in the command prompt.
```bash
php artisan make:resource PostResource
```
2. You go to the following path ***`App/Http/Resources`*** and we begin to modify each of the following:

* ***PostResource.php***.
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'body' => $this->body,
        ];
    }
}
```

3. We first go to the ***`routes/api.php`*** file to add a new route.
```PHP
use App\Models\User;
use App\Http\Resources\UserResource;
---
Route::get('/users', function () {
    $users = User::with(['profile', 'posts'])->get();
    $usersResource = UserResource::collection($users);
    return response()->json($usersResource);
});
```

4. Open the browser and go to the following link ***`http://127.0.0.1:8000/api/users`*** to see what results will appear.
```json
[
  {
    "id": 1,
    "username": "John Doe",
    "profile": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe"
    },
    "postss": [
      {
        "user_id": 1,
        "title": "Post title 1"
      },
      {
        "user_id": 1,
        "title": "Post title 5"
      },
      {
        "user_id": 1,
        "title": "Post title 6"
      }
    ]
  },
  {
    "id": 2,
    "username": "Tom Cruz",
    "profile": {
      "id": 2,
      "firstname": "Tom",
      "lastname": "Cruz"
    },
    "postss": [
      {
        "user_id": 2,
        "title": "Post title 2"
      },
      {
        "user_id": 2,
        "title": "Post title 3"
      },
      {
        "user_id": 2,
        "title": "Post title 7"
      }
    ]
  },
  {
    "id": 3,
    "username": "Adam Smith",
    "profile": {
      "id": 3,
      "firstname": "Adam",
      "lastname": "Smith"
    },
    "postss": [
      {
        "user_id": 3,
        "title": "Post title 4"
      }
    ]
  }
]
```

>We also see here that we fetch only the required data.


### Through the post ***`Post Model`***
This method is divided into two sub-methods depending on how the data is organized after it is fetched from the data base

#### Without using ***`API Resources`***.
1. We first go to the ***`routes/web.php`*** file so that we can test these methods and add this new route.
```PHP
use App\Models\Post;
---
Route::get('/posts', function () {
    $posts = Post::with('user', 'user.profile')->get();
    return view('posts.list', compact('posts'));
});
```
Here we can choose the columns for each relationship as follows.
```PHP
use App\Models\Post;
---
Route::get('/posts', function () {
    $posts = Post::with('user:username,id', 'user.profile:firstname,lastname,user_id')->get();
    return view('posts.list', compact('posts'));
});
```

In order to know the difference between selecting or not selecting columns from the database, let us see the specific response to each case.
* In the first case, with all data retrieved without exception, we find that ***`$posts`*** contains that data.
```json
[
  {
    "id": 1,
    "title": "Post title 1",
    "body": "Post body 1",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 2,
    "title": "Post title 2",
    "body": "Post body 2",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 3,
    "title": "Post title 3",
    "body": "Post body 3",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 4,
    "title": "Post title 4",
    "body": "Post body 4",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 3,
      "username": "Adam Smith",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 5,
    "title": "Post title 5",
    "body": "Post body 5",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 6,
    "title": "Post title 6",
    "body": "Post body 6",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 1,
      "username": "John Doe",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  },
  {
    "id": 7,
    "title": "Post title 7",
    "body": "Post body 7",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "id": 2,
      "username": "Tom Cruz",
      "created_at": "2023-08-07T06:23:03.000000Z",
      "updated_at": "2023-08-07T06:23:03.000000Z"
    }
  }
]
```

* In the second case, when specifying the exact required columns from the database, we find that the response is as follows.
```json
[
  {
    "id": 1,
    "title": "Post title 1",
    "body": "Post body 1",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 2,
    "title": "Post title 2",
    "body": "Post body 2",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  },
  {
    "id": 3,
    "title": "Post title 3",
    "body": "Post body 3",
    "user_id": 2,
    "created_at": null,
    "updated_at": null,
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  },
  {
    "id": 4,
    "title": "Post title 4",
    "body": "Post body 4",
    "user_id": 3,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "Adam Smith",
      "id": 3,
      "profile": {
        "firstname": "Adam",
        "lastname": "Smith",
        "user_id": 3
      }
    }
  },
  {
    "id": 5,
    "title": "Post title 5",
    "body": "Post body 5",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 6,
    "title": "Post title 6",
    "body": "Post body 6",
    "user_id": 1,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "John Doe",
      "id": 1,
      "profile": {
        "firstname": "John",
        "lastname": "Doe",
        "user_id": 1
      }
    }
  },
  {
    "id": 7,
    "title": "Post title 7",
    "body": "Post body 7",
    "user_id": 2,
    "created_at": "2023-08-07T06:23:41.000000Z",
    "updated_at": "2023-08-07T06:23:41.000000Z",
    "user": {
      "username": "Tom Cruz",
      "id": 2,
      "profile": {
        "firstname": "Tom",
        "lastname": "Cruz",
        "user_id": 2
      }
    }
  }
]
```

> When choosing specific columns in the relationships, you must choose ***id*** and you must choose the ***user*** relationship, because without you choosing it, the user will not return with the post.

2. We go to the following path ***`resources/views/users`*** and modify this file ***`list.blade.php`*** to display the users and their profiles. Also, what interests us here is displaying the posts Private for each user.

```PHP
<table>
    <thead>
        <tr>
            <th style="text-align: center">Username</th>
            <th style="text-align: center">Firstname</th>
            <th style="text-align: center">Lastname</th>
            <th style="text-align: center">Posts</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($users as $user)
            <tr>
                <td>{{ $user->username }}</td>
                <td>{{ $user->profile->firstname }}</td>
                <td>{{ $user->profile->lastname }}</td>
                <td>
                    <ul>
                        @foreach ($user->posts as $post)
                            <li>{{ $post->title }}</li>
                        @endforeach
                    </ul>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

3. Open the browser and go to the following link ***`http://127.0.0.1:8000/users`*** to see what results will appear.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}



#### By using ***`API Resources`***.

1. You go to the following path ***`App/Http/Resources`*** and we start editing the file ***`PostResource.php`***:
```PHP
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
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

2. We first go to the ***`routes/web.php`*** file to add a new route.
```PHP
use App\Models\Post;
use App\Http\Resources\PostResource;
---
Route::get('/posts', function () {
    $posts = Post::with(['user'])->get();
    $postsResource = PostResource::collection($posts);
    return response()->json($postsResource);
});
```

3. Open the browser and go to the following link ***`http://127.0.0.1:8000/api/posts`*** to see what results will appear.
```json
[
  {
    "id": 3,
    "title": "Post title 3",
    "body": "Post body 3",
    "user": {
      "id": 2,
      "username": "Tom Cruz"
    }
  },
  {
    "id": 4,
    "title": "Post title 4",
    "body": "Post body 4",
    "user": {
      "id": 3,
      "username": "Adam Smith"
    }
  },
  {
    "id": 5,
    "title": "Post title 5",
    "body": "Post body 5",
    "user": {
      "id": 1,
      "username": "John Doe Updated"
    }
  },
  {
    "id": 6,
    "title": "Post title 6",
    "body": "Post body 6",
    "user": {
      "id": 1,
      "username": "John Doe Updated"
    }
  },
  {
    "id": 7,
    "title": "Post title 7",
    "body": "Post body 7",
    "user": {
      "id": 2,
      "username": "Tom Cruz"
    }
  }
]
```

>We also see here that we fetch only the required data.


## How to update one to many relationship in Laravel?
![How to update one to many relationship in Laravel?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-update-one-to-many-relationship-in-laravel.png "How to update one to many relationship in Laravel?")
### Update data using the user form.

1. Using ***`push method`***.
* First go to ***`routes/web.php`*** file and modify this route:
```PHP
Route::get('/users/update', method () {
    $user = User::with('posts')->find(1);
    $post = $user->posts()->whereId(1)->first();
    $post->title = 'Post title 1 updated';
    $post->push();
    return response()->json($user);
});
```

* We open the browser and go to the new URL ***`http://127.0.0.1:8000/users/update`*** to find that the post has been updated successfully.
```json
{
  "id": 1,
  "username": "John Doe",
  "created_at": "2023-09-06T17:24:02.000000Z",
  "updated_at": "2023-09-06T17:25:58.000000Z",
  "posts": [
    {
      "id": 1,
      "title": "Post title 1 updated",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-09-06T17:28:58.000000Z",
      "updated_at": "2023-09-06T18:37:30.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    }
  ]
}
```

2. Using ***`update method`***.
* First go to the file ***`routes/web.php`*** and modify this route.
```PHP
Route::get('/users/update', method () {
    $user = User::with('posts')->find(1);
    $post = $user->posts()->whereId(1)->first();
    $post->title = 'Post title 1';
    $post->update();
    return response()->json($user);
]);
```

* We open the browser and go to the new URL ***`http://127.0.0.1:8000/users/update`*** to find that the post has been updated successfully.
```json
{
  "id": 1,
  "username": "John Doe",
  "created_at": "2023-09-06T17:24:02.000000Z",
  "updated_at": "2023-09-06T17:25:58.000000Z",
  "posts": [
    {
      "id": 1,
      "title": "Post title 1",
      "body": "Post body 1",
      "user_id": 1,
      "created_at": "2023-09-06T17:28:58.000000Z",
      "updated_at": "2023-09-06T18:41:35.000000Z"
    },
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    }
  ]
}
```

### Update data using the post form.
1. Using ***`push method`***.
* First go to ***`routes/web.php`*** file and add this route:
```PHP
Route::get('/posts/update', method () {
    $post = Post::with('user')->find(1);
    $post->title = 'Post title 1 updated';
    $post->user->username = 'John Doe Updated';
    $post->push();
    return response()->json($post);
});
```

* We open the browser and go to the new URL ***`http://127.0.0.1:8000/posts/update`*** to find that the post has been updated successfully.
```json
{
  "id": 1,
  "title": "Post title 1 updated",
  "body": "Post body 1",
  "user_id": 1,
  "created_at": "2023-09-06T17:28:58.000000Z",
  "updated_at": "2023-09-06T18:50:30.000000Z",
  "user": {
    "id": 1,
    "username": "John Doe Updated",
    "created_at": "2023-09-06T17:24:02.000000Z",
    "updated_at": "2023-09-06T18:49:54.000000Z"
  }
}
```

2. Using ***`update method`***.
* First go to the file ***`routes/web.php`*** and modify this route.
```PHP
Route::get('/posts/update', method () {
    $post = Post::with('user')->find(1);
    $post->user->username = 'John Doe';
    $post->update([
        'title' => 'Post title 1'
    ]);
    return response()->json($post);
]);
```

* We open the browser and go to the new URL ***`http://127.0.0.1:8000/posts/update`*** to find that the post has been updated successfully.
```json
{
  "id": 1,
  "title": "Post title 1",
  "body": "Post body 1",
  "user_id": 1,
  "created_at": "2023-09-06T17:28:58.000000Z",
  "updated_at": "2023-09-06T18:55:45.000000Z",
  "user": {
    "id": 1,
    "username": "John Doe",
    "created_at": "2023-09-06T17:24:02.000000Z",
    "updated_at": "2023-09-06T18:49:54.000000Z"
  }
}
```

## How to delete data from one-to-many relationship in Laravel?
![How to delete data from one-to-many relationship in Laravel?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-delete-data-from-one-to-many-relationship-in-laravel.png "How to delete data from one-to-many relationship in Laravel?")
### Delete data using the user form.
* First go to ***`routes/web.php`*** file and add this route:
```PHP
Route::get('/users/posts/delete', function () {
    $user = User::with('posts')->find(1);
    $user->posts()->whereIn('id', [1, 2])->delete();
    return response()->json($user);
});
```

* We open the browser and go to the new URL ***`http://127.0.0.1:8000/users/posts/delete`*** to find that the post has been deleted successfully.
```json
{
  "id": 1,
  "username": "John Doe Updated",
  "created_at": "2023-09-06T17:24:02.000000Z",
  "updated_at": "2023-09-06T18:49:54.000000Z",
  "posts": [
    {
      "id": 5,
      "title": "Post title 5",
      "body": "Post body 5",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    },
    {
      "id": 6,
      "title": "Post title 6",
      "body": "Post body 6",
      "user_id": 1,
      "created_at": "2023-09-06T17:29:49.000000Z",
      "updated_at": "2023-09-06T17:29:49.000000Z"
    }
  ]
}
```
### Delete data using the publication form.
* First go to ***`routes/web.php`*** file and add this path:
```PHP
Route::get('/posts/user/delete', function () {
    $post = Post::with('user')->findOrFail(2);
    $post->delete();
});
```
* We open the browser and go to the new URL ***`http://127.0.0.1:8000/posts/user/delete`***. We see that the post has been successfully deleted.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png"
alt="Rcord has deleted"
caption="Rcord has deleted"
>}}

## Conclusion
This article is a continuation of the entire series on __Laravel Eloquent Relationships__ Relationships within __Laravel__. We have covered __one-to-many relationship__ in a complete manner. We have not spared any information for you, and, God willing, we will learn in the following explanation about __the relationship of many to many__.

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
