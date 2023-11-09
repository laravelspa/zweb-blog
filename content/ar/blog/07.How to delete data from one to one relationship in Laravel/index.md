---
title: "كيفية حذف البيانات فى علاقة واحد إلى واحد في Laravel؟"
date: 2023-08-15
draft: false
slug: "how-to-delete-data-from-one-to-one-relationship-in-laravel-in-arabic"
cascade:
  showReadingTime: true
categories: ['Laravel', 'Laravel Relationships']
tags: ['laravel10', 'eloquent', 'relationships']
series: ['Laravel Eloquent Relationships']
series_order: 6
---
![كيف تحذف البيانات من علاقة واحد إلى واحد في Laravel؟](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/ar/how-to-delete-data-from-one-to-one-relationship-in-laravel.png "كيف تحذف البيانات من علاقة واحد إلى واحد في Laravel؟")
### حذف البيانات باستخدام نموذج المستخدم.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/users/profile/delete', method () {
    $user = User::with('profile')->find(1);
    $user->profile()->delete();
    return response()->json($user);
});
```

* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/users/profile/delete`*** لنجد أنه تم حذف ملف التعريف بنجاح.
```json
{
  "id": 1,
  "username": "Joun Doe",
  "created_at": "2023-08-07T06:23:03.000000Z",
  "updated_at": "2023-08-10T05:07:38.000000Z",
  "profile": null
}
```
> قم بتحديث الصفحة مرتين لإظهار أنه تم حذف ملف تعريف هذا المستخدم.
### حذف البيانات باستخدام نموذج الملف الشخصي.
* نذهب أولاً إلى ملف ***`routes/web.php`*** وأضف هذا المسار:
```PHP
Route::get('/profiles/user/delete', method () {
    $profile = Profile::with('user')->findOrFail(2);
    $profile->delete();
    $profile->user()->delete();
});
```
* نفتح المتصفح وننتقل إلى عنوان URL الجديد ***`http://127.0.0.1:8000/profiles/user/delete`***. نرى أنه تم حذف المستخدم والملف الشخصي بنجاح.
![Rcord has deleted](/img/laravel-eloquent-one-to-one-relationship-ultimate-guide-2023/404.png "Rcord has deleted")

## الخاتمة
هذه المقالة هي بداية سلسلة كاملة عن __Laravel Eloquent Relationships__ علاقات ضمن __Laravel__. لقد غطينا __علاقة واحد إلى واحد__ بطريقة كاملة. لم ندخر لكم أي معلومة ، وإن شاء الله ، سنتعرف في الشرح التالي على __علاقة واحد إلى كثير__.

- يمكنك العثور على repo لهذه السلسلة على github هنا:
---
{{< github repo="laravelspa/laravel-relations" >}}
