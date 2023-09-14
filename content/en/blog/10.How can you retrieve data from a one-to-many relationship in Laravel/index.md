---
title: "How can you retrieve data from a one-to-many relationship in Laravel?"
date: 2023-08-18
draft: false
slug: "how-can-you-retrieve-data-from-a-one-to-many-relationship-in-laravel"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 9
---
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
![Users view with posts](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/users-view-with-posts.png "Users view with posts")



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
![Users view with posts](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/users-view-with-posts.png "Users view with posts")

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

> We also see here that we fetch only the required data.

- You can find the repo of this series on github here:
{{< github repo="laravelspa/laravel-relations" >}}
