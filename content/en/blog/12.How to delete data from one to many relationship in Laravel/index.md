---
title: "How to delete data from one to many relationship in Laravel?"
date: 2023-08-20
draft: false
slug: "how-to-delete-data-from-one-to-many-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 11
---

![How to delete data from one-to-many relationship in Laravel?](/img/blog/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/en/how-to-delete-data-from-one-to-many-relationship-in-laravel.png "How to delete data from one-to-many relationship in Laravel?")
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
![Rcord has deleted](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png "Rcord has deleted")

## Conclusion
This article is a continuation of the entire series on __Laravel Eloquent Relationships__ Relationships within __Laravel__. We have covered __one-to-many relationship__ in a complete manner. We have not spared any information for you, and, God willing, we will learn in the following explanation about __the relationship of many to many__.

- You can find the repo of this series on github here:
---
{{< github repo="laravelspa/laravel-relations" >}}
