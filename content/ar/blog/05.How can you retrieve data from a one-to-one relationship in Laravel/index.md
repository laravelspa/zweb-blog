---
title: "كيفية استرداد البيانات فى علاقة one to one في Laravel؟"
date: 2023-08-13
draft: false
slug: "how-can-you-retrieve-data-from-a-one-to-one-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 4
---
![كيف يمكنك استرداد البيانات من علاقة one to one في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-do-you-get-data-into-a-One-To-One-relationship-in-laravel.png "كيف يمكنك استرداد البيانات من علاقة one to one في Laravel؟")

لقد رأينا كيف يتم حفظ البيانات بعدة طرق مختلفة في قاعدة البيانات باستخدام علاقة one to one في Laravel. لكن كيف نسترجع البيانات من قاعدة البيانات؟ هناك عدة طرق لاسترداد البيانات من قاعدة البيانات.

يمكن تقسيم هذه الطرق إلى فئتين رئيسيتين:

* الاسترجاع المباشر: تتضمن هذه الطريقة الاستعلام مباشرة عن قاعدة البيانات عن البيانات التي تحتاجها. هذه هي الطريقة الأكثر فاعلية لاسترداد البيانات ، ولكن قد يكون من الصعب استخدامها إذا لم تكن على دراية بـ SQL.

* Eloquent ORM: يوفر Eloquent ORM في Laravel طريقة سهلة الاستخدام لاسترداد البيانات من قاعدة البيانات. يستخدم Eloquent ORM مجموعة من الأساليب لتعيين جداول قاعدة البيانات إلى كائنات في PHP. هذا يجعل من السهل استرداد البيانات من قاعدة البيانات دون الحاجة إلى معرفة SQL.
في هذه السلسلة ، سنركز على استخدام Eloquent ORM لاسترداد البيانات من قاعدة البيانات.
### أثناء استرداد بيانات المستخدمين:
أثناء جمع بيانات المستخدم ، سنحصل على ملفاتهم الشخصية. بعد ذلك ، يتم تقسيم موضوع عرض تلك البيانات إلى شكلين ، ولا ثالث لهما. سيحدد نوع التطبيق الذي تعمل عليه النموذج الذي يجب استخدامه.

* أولاً: تطبيق يستخدم مسارات الويب.
* ثانيًا: تطبيق يعتمد على مسارات API.
#### أولاً: تطبيق يستخدم مسارات الويب.
1. ننتقل أولاً إلى ملف ***`routes/web.php`*** ونجري التغييرات التالية:
```PHP
Route::get('/users', method () {
    $users = User::with(['profile'])->get();
    return view('users.list', compact('users'));
});
```

> إذا تحققنا من الاستجابة لهذا الأمر ، فسنجد أنه تم الحصول على جميع البيانات من قاعدة البيانات.
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

هنا ، يمكنك فقط اختيار الأعمدة التي تحتاجها في النموذج التالي:
```PHP
Route::get('/users', method () {
    $users = User::with(['profile:firstname,lastname,user_id'])->get();
    return view('users.list', compact('users'));
});
```

> إذا تحققنا الآن مرة أخرى ، فستجد أن حجم البيانات قد تم تصغيره.
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

> هنا ، نرى الفرق بين الحالتين في حجم البيانات المعالجة. تزداد المسألة بشكل كبير إذا كانت البيانات أكبر من ذلك بكثير.

> عند اختيار أعمدة معينة من العلاقات ، يجب دائمًا اختيار عمود ***`foreign key`***. بدون اختياره ، لن يتم إرجاع البيانات بشكل صحيح من قاعدة البيانات.

2. داخل مجلد ***`views`*** ، نضيف مجلدًا آخر باسم ***`users`***. بداخله نضيف الملف ***`list.blade.php`***. في ***`list.blade.php`*** نضيف هذا الجدول البسيط لعرض المستخدمين:
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

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/users`*** لترى النتائج التي ستظهر.
![Laravel One To One Relationship - Users Table](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/users-table.png "Users Table")

#### ثانيًا: تطبيق يعتمد على مسارات API.

> ما هي API Resources؟

ببساطة ، إنها طبقة وسيطة بين Eloquent واستجابة API ، حيث تقوم بتحويل البيانات التي تم الحصول عليها من قاعدة البيانات إلى JSON مع القدرة على تحديد بيانات معينة دون غيرها أو معالجة تلك البيانات.

1. نقوم بإنشاء API Resocrces للمستخدمين والملفات الشخصية. قم بتنفيذ هذا الأمر في موجه الأوامر.
```bash
php artisan make:resource UserResource
php artisan make:resource ProfileResource
```

2. انتقل إلى المسار التالي: *** `App\Http\Resources`*** وعدّل كليهما:

* ملف ***`ProfileResource.php`***.
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

* ملف ***`UserResource.php`***.
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

3. انتقل إلى ملف ***`routes/api.php`*** الخاص بك وأضف مسارًا جديدًا.
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

4. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/api/users`*** لترى النتائج التي ستظهر.
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

> نرى هنا أيضًا أننا حصلنا على البيانات المطلوبة المحددة في ملفات موارد API فقط.

### أثناء استرداد بيانات الملفات الشخصية:
أثناء الحصول على بيانات الملف الشخصي ، سنحصل على كل مستخدم مرتبط بهذه الملفات الشخصية.
بعد ذلك ، يمكن عرض البيانات في شكلين لا ثالث لهما.
سيحدد نوع التطبيق الذي تعمل عليه والموضوع المستخدم أي شكل من أشكال عرض البيانات هو الأنسب.

* أولاً: تطبيق يستخدم مسارات الويب.
* ثانيًا: تطبيق يعتمد على مسارات API.
#### أولاً: تطبيق يستخدم مسارات الويب.
1. ننتقل أولاً إلى ملف ***`routes/web.php`*** ونجري التغييرات التالية:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user')->get();
    return view('profiles.list', compact('profiles'));
});
```
> إذا تحققنا من الاستجابة لهذا الأمر ، فسنجد أنه تم الحصول على جميع البيانات من قاعدة البيانات.
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

هنا ، يمكنك فقط اختيار الأعمدة التي تحتاجها في النموذج التالي:
```PHP
Route::get('/profiles', method () {
    $profiles = Profile::with('user:username,id')->get();
    return view('profiles.list', compact('profiles'));
});
```

> إذا تحققنا الآن مرة أخرى ، فستجد أن حجم البيانات قد تم تصغيره.
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

> هنا ، نرى الفرق بين الحالتين في حجم البيانات المعالجة. تزداد المسألة بشكل كبير إذا كانت البيانات أكبر من ذلك بكثير.

> عند تحديد أعمدة معينة في العلاقات ، يجب تضمين حقل ***`id`*** عند اختيار علاقة ***`user`***. خلاف ذلك ، لن يتم إرجاع المستخدم مع بيانات ملف التعريف.

2. داخل مجلد ***`views`*** ، نضيف مجلدًا آخر باسم ***`profiles`***. بداخله نضيف الملف ***`list.blade.php`***. في ***`list.blade.php`*** نضيف هذا الجدول البسيط لعرض الملفات الشخصية للمستخدمين.:
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

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/profiles`*** لترى النتائج التي ستظهر.
![Laravel One To One Relationship - Profiles Table](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/profiles-table.png "Profiles Table")

#### ثانيًا: تطبيق يعتمد على مسارات API.

1. انتقل إلى المسار التالي: *** `App\Http\Resources`*** وعدّل هذا الملف ***`ProfileResource.php`***:
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

2. انتقل إلى ملف ***`routes/api.php`*** الخاص بك وأضف مسارًا جديدًا.
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

3. افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/api/profiles`*** لترى النتائج التي ستظهر.
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

## كيفية تحسين استعلامات Eloquent في Laravel؟
![كيفية تحسين استعلامات Eloquent في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-improve-Eloquent-queries-in-laravel.png "كيفية تحسين استعلامات Eloquent في Laravel؟")

عند التعامل مع قاعدة بيانات كبيرة بها الكثير من البيانات ، من المهم مراعاة الأداء. هذا لا يعني فقط الوقت الذي يستغرقه الحصول على البيانات ، ولكن أيضًا عدد الاستعلامات التي يتم تنفيذها لكل صفحة.

لاختبار أداء تطبيقك ، يمكنك استخدام مكتبة تسمى [Laravel Debugbar](https://github.com/barryvdh/laravel-debugbar). سيعرض لك شريط التصحيح جميع الاستعلامات التي يتم تنفيذها على كل صفحة ، بالإضافة إلى الوقت الذي يستغرقه كل استعلام في التنفيذ. يمكن أن تساعدك هذه المعلومات في تحديد معوقات الأداء وتحسين تطبيقك.

* لنقم بتشغيل هذا الأمر.
```bash
composer require barryvdh/laravel-debugbar --dev
```

> تأكد من أن ***`APP_DEBUG=true`*** داخل ملف ***`.env`***.

* الفرق بين الحالتين كالتالي:
```PHP
$users = User::all();
$users = User::with('profile')->get();
```

* افتح المتصفح وانتقل إلى عنوان URL التالي ***`http://127.0.0.1:8000/users`*** لترى النتائج التي ستظهر في شريط المكتبة.

1. __(Lazy Loading)__ - استرداد البيانات بدون استخدام ***`with`***.
![laravel debugbar lazy loading](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-lazy-loading.png "laravel debugbar lazy loading")

في الجزء السفلي من الصفحة ، سنجد الآن شريطًا لمكتبة __Laravel Debugbar__. عند النقر فوقه ، سنجد أنه يعمل مع الكثير من البيانات. الشيء الذي يثير اهتمامنا هنا هو عدد استعلامات SQL في هذه الصفحة ، وكما هو موضح ، فهي __4__ استعلامات.

وقمنا باسترجاع 3 مستخدمين فقط ، تخيلوا معي إذا كان هناك عشرات الآلاف أو الملايين من المستخدمين داخل قاعدة البيانات هذه ، فإن وقت تحميل هذه الصفحة سيكون بطيئًا للغاية بسبب العدد الهائل من الإستعلامات.

هذه تسمى مشكلة N+1 في Laravel. يحدث هذا بسبب lazy loading relationships ، مما يعني أن البيانات المرتبطة لا يتم تحميلها حتى يتم الاحتياج إليها بالفعل. في هذه الحالة ، نتكاسل في تحميل علاقة الملف الشخصى لكل مستخدم. هذا يعني أننا نجري استعلامًا منفصلاً لكل مستخدم للحصول على ملفه الضخصى.

إذا كان هناك 1000 مستخدم ، فسنقدم 1001 استعلامًا: استعلام واحد لجذب المستخدمين ، و 1000 استعلام للحصول على ملفهم الشخصى. يعد هذا مضيعة لموارد الخادم ويمكن أن يجعل تطبيقك بطيئًا.

لحل مشكلة N+1 ، يمكنك تحميل العلاقات eager loading. هذا يعني أنك ستقوم بتحميل البيانات المرتبطة عند استرداد البيانات لأول مرة. في هذه الحالة ، يمكنك استخدام ***`with()`*** method لتحميل علاقة الملف الشخصى. سيؤدي هذا إلى إجراء استعلام واحد فقط للحصول على المستخدمين وملفاتهم الشخصية.

يمكن أن يؤدي التحميل الجاد إلى تحسين أداء تطبيقك عن طريق تقليل عدد الاستعلامات. إنها ممارسة جيدة يجب تحميل العلاقات eager loading كلما أمكن ذلك.

2. __(Eager Loading)__ - استرداد البيانات بإستخدام ***`with`***.
![laravel debugbar eager loading](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/laravel-debugbar-eager-loading.png "laravel debugbar eager loading")

كما ترى ، بإضافة طريقة with () مع اسم العلاقة ، سيتم جلب بيانات كل مستخدم بملف التعريف الخاص به بدون مشكلة N + 1.

لذلك تم تقليل عدد الاستعلامات من 4 إلى 2 فقط. من الواضح أن هذا سيكون له تأثير إذا كانت قاعدة البيانات هذه متوسطة الحجم أو كبيرة. هذا ما يسمى eager loading.

- يمكنك العثور على repo لهذه السلسلة على github هنا:
---
{{< github repo="laravelspa/laravel-relations" >}}
