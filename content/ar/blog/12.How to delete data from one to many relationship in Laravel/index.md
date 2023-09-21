---
title: "كيفية حذف البيانات فى علاقة واحد إلى كثير في Laravel؟"
date: 2023-08-20
draft: false
slug: "how-to-delete-data-from-one-to-many-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 11
---
![كيف تحذف البيانات من علاقة واحد إلى كثير في Laravel؟](/img/laravel-eloquent-one-to-many-relationship-ultimate-guide-2023/ar/how-to-delete-data-from-one-to-many-relationship-in-laravel.png "كيف تحذف البيانات من علاقة واحد إلى كثير في Laravel؟")
### حذف البيانات باستخدام نموذج المستخدم.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا الرابط:
```PHP
Route::get('/users/posts/delete', function () {
    $user = User::with('posts')->find(1);
    $user->posts()->whereIn('id', [1, 2])->delete();
    return response()->json($user);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/posts/delete`*** لنجد أنه تم حذف ملف التعريف بنجاح.
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

### حذف البيانات باستخدام نموذج المنشور.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا الرابط:
```PHP
Route::get('/posts/user/delete', function () {
    $post = Post::with('user')->findOrFail(2);
    $post->delete();
});
```
* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/posts/user/delete`***. نرى أنه تم حذف المنشور بنجاح.
{{< figure
src="/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png"
alt="Rcord has deleted"
caption="Rcord has deleted"
>}}

## الخاتمة
هذه المقالة هي تكمبة لسلسلة كاملة عن __Laravel Eloquent Relationships__ علاقات ضمن __Laravel__. لقد غطينا __علاقة واحد إلى كثير__ بطريقة كاملة. لم ندخر لكم أي معلومة ، وإن شاء الله ، سنتعرف في الشرح التالي على __علاقة كثير إلى كثير__.

- يمكنك العثور على repo لهذه السلسلة على github هنا:
{{< github repo="laravelspa/laravel-relations" >}}
