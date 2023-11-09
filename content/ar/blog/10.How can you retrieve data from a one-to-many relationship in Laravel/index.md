---
title: "كيفية استرداد البيانات فى علاقة one to many في Laravel؟"
date: 2023-08-18
draft: false
slug: "how-can-you-retrieve-data-from-a-one-to-many-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 9
---
![كيف يمكنك استرداد البيانات من علاقة one to many في Laravel؟](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/ar/how-do-you-get-data-into-a-Many-To-One-relationship-in-laravel.png "كيف يمكنك استرداد البيانات من علاقة one to many في Laravel؟")
رأينا كيف يتم حفظ البيانات بطرق مختلفة ومتعددة داخل قاعدة البيانات بإستخدام علاقة One-To-Many فى Laravel.
ولكن كيف يتم جلب البيانات من قاعدة البيانات؟
هناك عدة طرق يمكن من خلالها جلب البيانات من قاعدة البيانات.
وتنقسم هذه الطرق الى طريقتين أساسيتين:

### من خلال المستخدم ***`User Model`***
تنقسم هذه الطريقة الى طريقتين فرعيتين تعتمد على كيفية تنظيم البيانات بعد جلبها من قاغدة البيانات

#### بدون إستخدام ***`API Resources`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  حتى نستطيع أن نختبر هذه الطرق ونقوم بالتعديلات التالية.
```PHP
use App\Models\User;
---
Route::get('/users', function () {
    $users = User::with(['profile', 'posts'])->get();
    return view('users.list', compact('users'));
});
```
 هنا يمكننا اختيار الاعمده الخاصه بكل علاقة بالشكل التالي.
```PHP
use App\Models\User;
---
Route::get('/users', function () {
    $users = User::with(['profile:id,firstname,lastname,user_id', 'posts:title,user_id'])->get();
    return view('users.list', compact('users'));
});
```

ولكى نعرف الفرق بين تحديدنا او عدم تحديدنا للأعمدة من قاعدة البيانات تعالوا نرى الأستجابة الخاصة لكل حالة
* الخالة الأولى مع جلب البيانات جميعها بدون إستثناء نجد أن ***`$users`*** يحتوى على البيانات تلك.
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

* أما فى الحالة الثانية عند تحديد الأعمدة المطلوبة بالضبط من قاعدة البيانات نجد أن الإستجابة كالتالى.
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

> وهنا نرى الفرق بين الحالتين فى حجم البيانات التى تمت معالجتها ويزداد الأمر إذا كانت البيانات حجمها أكبر من ذلك بكثير.
> يجب عند إختيار إعمدة محددة فى العلاقات أن تختار ***foreignKey*** لأن بدون إختيارك له لن تعود البيانات بشكل صحيح من قاعدة البيانات.

2. نقوم بالذهاب الى المسار التالى ***`resources/views/users`*** ونقوم بتعديل هذا الملف ***`list.blade.php`*** لعرض المستخدمين وملفات التعريف الخاصة بهم وأيضا ما يهمنا هنا هو عرض المنشورات الخاصة لكل مستخدم.

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

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}


#### عن طريق إستخدام ***`API Resources`***.
1. سنقوم بإنشاء API Resource لنموذج Post عن طريق تنفيذ هذا الأمر فى موجه الأوامر.
```bash
php artisan make:resource PostResource
```
2. تذهب الى المسار التالى ***`App/Http/Resources`*** ونبدأ بالتعديل على كلا من:

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

3. نقوم بالتوجه أولا لملف ***`routes/api.php`***  لنضيف رابط جديد.
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

4. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/api/users`*** لنرى ما هى النتائج التى ستظهر.
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

> ونرى هنا أيضا أننا نجلب البيانات المطلوبة فقط.


### من خلال المنشور ***`Post Model`***
تنقسم هذه الطريقة الى طريقتين فرعيتين تعتمد على كيفية تنظيم البيانات بعد جلبها من قاغدة البيانات

#### بدون إستخدام ***`API Resources`***.
1. نقوم بالتوجه أولا لملف ***`routes/web.php`***  حتى نستطيع أن نختبر هذه الطرق ونقوم بإضافة هذا الرابط الجديد.
```PHP
use App\Models\Post;
---
Route::get('/posts', function () {
    $posts = Post::with('user', 'user.profile')->get();
    return view('posts.list', compact('posts'));
});
```
 هنا يمكننا اختيار الاعمده الخاصه بكل علاقة بالشكل التالي.
```PHP
use App\Models\Post;
---
Route::get('/posts', function () {
    $posts = Post::with('user:username,id', 'user.profile:firstname,lastname,user_id')->get();
    return view('posts.list', compact('posts'));
});
```

ولكى نعرف الفرق بين تحديدنا او عدم تحديدنا للأعمدة من قاعدة البيانات تعالوا نرى الأستجابة الخاصة لكل حالة
* الخالة الأولى مع جلب البيانات جميعها بدون إستثناء نجد أن ***`$posts`*** يحتوى على البيانات تلك.
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

* أما فى الحالة الثانية عند تحديد الأعمدة المطلوبة بالضبط من قاعدة البيانات نجد أن الإستجابة كالتالى.
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

> يجب عند إختيار إعمدة محددة فى العلاقات أن تختار ***id*** وأنت تختار علاقة ***user*** لأن بدون إختيارك له لن يعود المستخدم مع المنشور. 

2. نقوم بالذهاب الى المسار التالى ***`resources/views/users`*** ونقوم بتعديل هذا الملف ***`list.blade.php`*** لعرض المستخدمين وملفات التعريف الخاصة بهم وأيضا ما يهمنا هنا هو عرض المنشورات الخاصة لكل مستخدم.

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

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/users`*** لنرى ما هى النتائج التى ستظهر.
{{< figure
src="/img/laravel10-one-to-many-relationship/users-view-with-posts.png"
alt="Users view with posts"
caption="Users view with posts"
>}}



#### عن طريق إستخدام ***`API Resources`***.

1. تذهب الى المسار التالى ***`App/Http/Resources`*** ونبدأ بالتعديل على ملف ***`PostResource.php`***:
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

2. نقوم بالتوجه أولا لملف ***`routes/web.php`***  لنضيف رابط جديد.
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

3. أفتح المتصفح وقم بالذهاب الى الرابط التالى ***`http://127.0.0.1:8000/api/posts`*** لنرى ما هى النتائج التى ستظهر.
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

> ونرى هنا أيضا أننا نجلب البيانات المطلوبة فقط.


- يمكنك العثور على repo لهذه السلسلة على github هنا:
---
{{< github repo="laravelspa/laravel-relations" >}}
