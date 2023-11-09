---
title: "How to insert data in one to one relationship in database?"
date: 2023-08-12
draft: false
slug: "how-to-insert-data-in-one-to-one-relationship-in-database"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 3
---
![How to insert data in one to one relationship in database?](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-insert-data-in-one-to-one-relationship-in-database.png "How to insert data in one to one relationship in database?")

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

- You can find the repo of this series on github here:
---
{{< github repo="laravelspa/laravel-relations" >}}
