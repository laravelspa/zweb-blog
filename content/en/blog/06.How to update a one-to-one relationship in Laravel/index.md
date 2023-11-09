---
title: "How to update a one-to-one relationship in Laravel?"
date: 2023-08-14
draft: false
slug: "how-to-update-a-one-to-one-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 5
---
![How to update one to many relationship in Laravel?](/img/blog/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-update-one-to-many-relationship-in-laravel.png "How to update one to many relationship in Laravel?")
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

- You can find the repo of this series on github here:
---
{{< github repo="laravelspa/laravel-relations" >}}
