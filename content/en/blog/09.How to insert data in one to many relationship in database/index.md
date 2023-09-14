---
title: "How to insert data in one to many relationship in database?"
date: 2023-08-17
draft: false
slug: "how-to-insert-data-in-one-to-many-relationship-in-database"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 8
---

![How to insert data in a one-to-many relationship in the database?](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-insert-data-in-one-to-many-relationship-in-database.png "How to insert data in a one-to-many relationship in the database?")

After we created a One-To-Many relationship between both Users table and Posts table and added ***`hasMany`*** inside ***`User Model`***, also we added the inverse relationship inside ***`Post Model`*** by adding ***`belongsTo`*** to it.

  The time has come to find out how the data is saved in the database while we use this relationship.
What are the methods used in this?


These methods are divided into three basic methods:
1. Without using **function post**.
2. By using **function post**.
3. By using the inverse relationship **function user**.

### 1.Without using **function post**.
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


> In addition to these posts, there is another post that was added in a previous step, and therefore there should be 3 posts for this user. This is actually the data obtained from the database in the following response.

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

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
