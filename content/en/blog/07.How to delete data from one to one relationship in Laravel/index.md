---
title: "How to delete data from one to one relationship in Laravel?"
date: 2023-08-15
draft: false
slug: "how-to-delete-data-from-one-to-one-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 6
---
![How to delete data from one to one relationship in Laravel?](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-delete-data-from-one-to-one-relationship-in-laravel.png "How to delete data from one to one relationship in Laravel?")
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
![Rcord has deleted](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png "Rcord has deleted")

## Conclusion
This article is the start of a whole series on __Laravel Eloquent Relationships__ - Relationships within __Laravel__. We have covered the __One TO One relationship__ in a complete way. We did not spare you any information, and God willing, in the following explanation, we will learn about the __One To Many relationship__.

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
