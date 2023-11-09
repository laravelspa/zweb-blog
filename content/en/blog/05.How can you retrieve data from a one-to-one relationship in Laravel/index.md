---
title: "How can you retrieve data from a one-to-one relationship in Laravel?"
date: 2023-08-13
draft: false
slug: "how-can-you-retrieve-data-from-a-one-to-one-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 4
---
![How can you retrieve data from a one-to-one relationship in Laravel?](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-can-you-retrieve-data-from-a-one-to-one-relationship-in-laravel.png "How can you retrieve data from a one-to-one relationship in Laravel?")

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
![Laravel One To One Relationship - Users Table](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png "Users Table")


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
![Laravel One To One Relationship - Profiles Table](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png "Profiles Table")


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
![How to improve Eloquent queries in Laravel?](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/en/how-to-improve-Eloquent-queries-in-laravel.png "How to improve Eloquent queries in Laravel?")


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
![laravel debugbar lazy loading](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png "laravel debugbar lazy loading")

At the bottom of the page, we will now find a bar for the __Laravel Debugbar library__. When you click on it, we will find that it works with a lot of data. The thing that interests us here is the number of SQL queries on this page, and as shown, they are __4__ queries.

And we only retrieved 3 users, imagine with me if there are tens of thousands or millions of users inside this database, the time to load this page will be very slow because of the huge number of queries.

This is called the N+1 problem in Laravel. It is caused by lazy loading relationships, which means that the associated data is not loaded until it is actually needed. In this case, we are lazy loading the posts relationship for each user. This means that we are making a separate query for each user to get their profiles.

If there are 1000 users, we will make 1001 queries: 1 query to get the users, and 1000 queries to get their profiles. This is a waste of server resources and can make your application slow.

To solve the N+1 problem, you can eager load the relationships. This means that you will load the associated data when you first retrieve the data. In this case, you would use the with() method to eager load the profiles relationship. This would only make one query to get the users and their profiles.

Eager loading can improve the performance of your application by reducing the number of queries. It is a good practice to eager load relationships whenever possible.

2. __(Eager Loading)__ - Retrieve data using ***`with`***.
![laravel debugbar eager loading](/img/blog/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png "laravel debugbar eager loading")

As you can see, by adding the with() method with the name of the relationship, each user's data will be fetched with their own profile without the N+1 problem.

So the number of queries was reduced from 4 to 2 only. This will clearly have an effect if this database is of medium size or large. This is what is called eager loading.

- You can find the repo of this series on github here:
---
{{< github repo="laravelspa/laravel-relations" >}}
