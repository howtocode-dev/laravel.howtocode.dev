# কন্ট্রোলার

কন্ট্রোলার হচ্ছে মডেল ও ভিউ এর মধ্যবর্তী একটা অংশ, এটি **HTTP Request** গুলো সার্ভ করে থাকে। যদিও আমরা এই কাজটি লারাভেলে কন্ট্রোলার ছাড়া রাউট ডিফাইন করে ভিউ ও মডেলের মধ্যে যোগাযোগ করতে পারি। আলাদাভাবে কন্ট্রোলার ব্যাবহার করা হচ্ছে একটি আদর্শ পদ্ধতি। **routes/web.php** ফাইলের মধ্যে সকল প্রকার লজিক গুলো ডিক্লেয়ার না করে একটা পৃথক ফাইলের মধ্যে রাখা যায়, যেটি একটি সিঙ্গেল ক্লাসের মধ্যে সীমাবদ্ধ থাকবে। আর যখন ওই কন্ট্রোলার ক্লাসের কোন ফাংশন অথবা কোন মেথডকে **HTTP Request** এর মাধ্যমে কল করার দরকার হবে তখন সেটা রাউটের মধ্যে ডিফাইন করে দিতে হবে। লারাভেলে কন্ট্রোলার ফাইল গুলা **app/Http/Controllers** এই ডিরেক্টরিতে রাখতে হয়।

## বেসিক কন্ট্রোলারঃ

এখন ইউজার এর জন্য একটা বেসিক কন্ট্রোলার তৈরি সম্পর্কে আমি আপনাদেরকে ধারনা দিব। ধরুন কন্ট্রোলারটিতে ২টি মেথড রাখব নিচে সেটা দেয়া হল।

```php
<?php namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class UserController extends Controller {

    public function index() {
        return "You've arrived at User Controller.";
    }

    public function showProfile($id) {
        $data = [
            'name' => 'Sohel Amin',
            'email' => 'sohelamin@example.com'
        ];
        return view('user.profile', $data);
    }
}
```

এখানে মুল রাউটের জন্য **index\(\)** আর ইউজার প্রোফাইল দেখার জন্য **showProfile\(\)** নামে ২টি মেথড ব্যাবহার করা হয়েছে। কন্ট্রোলার ডিক্লেয়ার করার সময় মনে রাখতে হবে যেন সঠিক ভাবে কন্ট্রোলার এর নেমস্পেস ডিফাইন করা হয়। এইক্ষেত্রে এইখানে **App\Http\Controllers** নেমস্পেস ব্যাবহার করা হয়েছে। কন্ট্রোলার এর নেমস্পেস ডিফাইন করা খুবই সহজ কারণ কন্ট্রোলার এর ডিরেক্টরি লোকেশন আর নেমস্পেস একই। এরপর কন্ট্রোলার এর নাম দিতে হয়। ইউজার কন্ট্রোলার এর জন্য দেয়া হয়েছে **UserController** মনে রাখবেন কন্ট্রোলার এর নাম আর কন্ট্রোলার এর ফাইল এর নাম একই হতে হবে। যেমনঃ **UserController.php** এখন ব্রাউজারে কন্ট্রোলারটিকে একসেস করতে চাইলে নিচের মত করে রাউট ডিফাইন করতে হবে। Route::get\('user', 'UserController@index'\); উপরের লাইনের মাধ্যমে **index** মেথডটিকে কল করা হয়েছে আর এর জন্য ব্রাউজারে দিতে হবে [http://localhost:8000/user/](http://localhost:8000/user/) এরকম এড্রেস।

```php
Route::get('user/{id}', 'UserController@showProfile');
```

উপরের লাইনের মাধ্যমে **showProfile** মেথডটিকে কল করা হয়েছে আর এর জন্য ব্রাউজারে দিতে হবে [http://localhost:8000/user/1](http://localhost:8000/user/1) এরকম এড্রেস। এখানে **1** এর জায়গায় আপনার কাঙ্ক্ষিত **id** ব্যবহার করতে হবে। আর **id** অনুযায়ী ডাটাবেস থেকে ডাটা আনতে হবে মডেল এর সাহায্যে। এই উদাহরণে আমি আপনাদেরকে শুধু একটা অ্যারে এর মাধ্যমে দেখিয়েছি। আপনারা পরে মডেল ব্যবহার করে এটা করবেন।

### কন্ট্রোলার আর নেমস্পেসঃ

অ্যাপ্লিকেশন ডেভেলপ করার সময় কিছু কন্ট্রোলারকে পৃথক কোন ফোল্ডার কিংবা ডিরেক্টরিতে রাখতে হয়। আর এইক্ষেত্রে আমরা কাস্টম নেমস্পেস ব্যাবহার করে তা করতে পারি। ধরুন লারাভেলের কন্ট্রোলার ডিরেক্টরিতে **Photos** নামে একটি সাব-ডিরেক্টরি তৈরি করব আর ওই সাব-ডিরেক্টরিতে **AdminController** রাখব তাহলে কন্ট্রোলার ফাইলটি নিচের মত হবে।

```php
<?php namespace App\Http\Controllers\Photos;

use App\Http\Controllers\Controller;

class AdminController extends Controller {

    public function index() {
        return "You've arrived at Admin Controller.";
    }

}
```

আর রাউট ফাইলে নিচের মত করে ডিফাইন করতে হবে।

```php
Route::get('photos/admin', 'Photos\AdminController@index');
```

আর এখানে **AdminController** এর সামনে শুধু **Photos** নেমস্পেস উল্লেখ করা হয়েছে।

## ইমপ্লিসিট কন্ট্রোলারসঃ

লারাভেলে একটা সিঙ্গেল কন্ট্রোলার এর সকল মেথডকে একটা সিঙ্গেল রাউটের মধ্যে ডিফাইন করা যায়। ধরুন আমরা ইউজার কন্ট্রোলার এর মেথডগুলা নিচের মত করে রাখব।

```php
<?php namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class UserController extends Controller {

    public function getIndex() {
        //
    }

    public function postProfile() {
        //
    }

    public function anyLogin() {
        //
    }

}
```

তাহলে এর জন্য রাউটে নিচের মত করে ডিফাইন করতে হবে।

```php
Route::controller('users', 'UserController');
```

এখানে মেথডের সামনে প্রিফিক্স হিসেবে **get**, **post** আর **any** ব্যাবহার করা হয়েছে। আর এই প্রিফিক্স ব্যাবহার করার ফলে স্বয়ংক্রিয় ভাবে রাউট সেই মেথডগুলোকে **HTTP Verb** হিসেবে ডিফাইন করে নিবে। এখানে **any** এর মাধ্যমে **HTTP** এর সকল **Verb** রেজিস্টার হবে।

আপনারা রেজিস্টারকৃত রাউটগুলো খুব সহজেই কমান্ড প্রমোট অথবা টার্মিনালে দেখতে পারেন এই কমান্ডটির মাধ্যমে।

```text
php artisan route:list
```

## রেস্টফুল রিসোর্স কন্ট্রোলারঃ

আপনারা যদি রেস্টফুল কন্ট্রোলার তৈরি করতে চান তাহলে রিসোর্স কন্ট্রোলার এর মাধ্যমে এটা করতে পারেন যেটি আপনাদেরকে অনেক সময় বাঁচিয়ে দিবে।

**Artisan** এর মাধ্যমে আপনারা খুব সহজেই রেস্টফুল কন্ট্রোলার তৈরি করতে পারেন। ধরুন এখন আমরা **Photo** কন্ট্রোলার তৈরি করব তাহলে আমাদেরকে নিচের মত করে টার্মিনালে লিখতে হবে।

```text
php artisan make:controller PhotoController
```

এবার রাউটে নিচের মত লিখতে হবে।

```text
Route::resource('photo', 'PhotoController');
```

যার ফলে কন্ট্রোলার এ আগেই ডিফাইনকৃত কিছু মেথড পাওয়া যাবে। এখন উক্ত কন্ট্রোলারে আপনাদের প্রয়োজনীয় লজিক গুলো ডিফাইন করতে হবে।

রিসোর্স কন্ট্রোলারটি নিচের মত করে অ্যাকশন হ্যান্ডেল করবে।

| **Verb** | **Path** | **Action** | **Route Name** |
| :--- | :---: | ---: | ---: |
| GET | /photo | index | photo.index |
| GET | /photo/create | create | photo.create |
| POST | /photo | store | photo.store |
| GET | /photo/{photo} | show | photo.show |
| GET | /photo/{photo}/edit | edit | photo.edit |
| PUT/PATCH | /photo/{photo} | update | photo.update |
| DELETE | /photo/{photo} | destroy | photo.destroy |

এখন আপনি আপনার প্রয়োজনে রিসোর্স রাউট এর অ্যাকশন কাস্টমাইজ করে নিতে পারেন। ধরুন আপনি **index** আর **show** অ্যাকশন/মেথড রাখতে চাচ্ছেন তাহলে রাউটে নিচের মত করে লিখতে হবে।

```php
Route::resource('photo', 'PhotoController',
                ['only' => ['index', 'show']]);
```

এবার আপনি বিশেষ কিছু অ্যাকশন বাদ দিবেন তাহলে নিচের মত করে তা করতে পারেন।

```php
Route::resource('photo', 'PhotoController',
                ['except' => ['create', 'store', 'update', 'destroy']]);
```

রিসোর্স কন্ট্রোলারে বাই ডিফল্ট কিছু নাম থাকে মেথড কিংবা অ্যাকশন এ আপনি চাইলে সহজেই তা পরিবর্তন করতে পারেন রাউটের মাধ্যমে। এইক্ষেত্রে কন্ট্রোলার ক্লাসের মধ্যে কোন পরিবর্তন করতে হবে না।

```php
Route::resource('photo', 'PhotoController',
                ['names' => ['create' => 'photo.build']]);
```

এবার আপনি যদি নেস্টেট রিসোর্স কন্ট্রোলার ব্যবহার করতে চান অর্থাৎ **Url** এরকম হবে **photos/{photos}/comments/{comments}**

তাহলে আপনি সেটা রাউটে নিচের মত করে লিখতে পারেন।

```php
Route::resource('photos.comments', 'PhotoCommentController');
```

এইক্ষেত্রে আগের ডিফাইন করা রাউটটিকেও রাখতে হবে।

এবার ধরুন আপনাকে অতিরিক্ত কিছু মেথড আপনার কন্ট্রোলারে রাখতে হচ্ছে এবং আপনি তা রাউটেও ডিফাইন করতে চাচ্ছেন তাহলে আপনাকে নিচের মত করে লিখতে হবে।

```php
Route::get('photos/popular', 'PhotoController@method');
Route::resource('photos', 'PhotoController');
```

এইক্ষেত্রে অবশ্যই মনে রাখবেন রাউটে রিসোর্স কন্ট্রোলার ডিফাইন করার আগেই ওই মেথডটি রাউটে ডিফাইন করতে হবে।

## কন্ট্রোলার মিডলওয়্যারঃ

মিডলওয়ারকে কন্ট্রোলারের রাউটে ডিফাইন করা যায়। ধরুন আমরা ইউজার অ্যাথেন্টিকেশনের জন্য **auth** মিডলওয়্যারটি ব্যাবহার করব তাহলে রাউটে নিচের মত করে লিখতে হবে।

```php
Route::get('profile', [
    'middleware' => 'auth',
    'uses' => 'UserController@showProfile'
]);
```

এবার কন্ট্রোলারের কন্সট্রাক্টরে _**$this-&gt;middleware\('auth'\);**_ এই লাইনটি যোগ করতে হবে। নিচে কন্ট্রোলার এর কোডটি কিছু উদাহরণ সহ দেয়া হল।

```php
<?php namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class UserController extends Controller {

    /**
     * Instantiate a new UserController instance.
     */
    public function __construct()
    {
      // auth middleware will affect on all method in this controller
        $this->middleware('auth');
      // log middleware will only affect for fooAction & barAction
        $this->middleware('log', ['only' => ['fooAction', 'barAction']]);
      // subscribed middleware will affect on all method exclude fooAction & barAction
        $this->middleware('subscribed', ['except' => ['fooAction', 'barAction']]);
    }

}
```

## অনুশীলনঃ

আজ জানলাম controller এবং বুঝলাম routes.php থেকে সরাসরি view তে না গিয়ে controller ব্যবহার করা উচিৎ। আসুন আমাদের আগের অনুশীলন গুলি পরিবর্তন করে routes.php থেকে সরাসরি view তে না গিয়ে controller ব্যবহার করি।

লারাভেল এর দারুণ এক ফিচার Artisan CLI. ভবিষ্যৎ অনুশীলনে আমরা এটার প্রচুর ব্যবহার দেখবো। আর কথা না বাড়িয়ে নিচের command টি run করাই।

```bash
php artisan make:controller PagesController
```

Controller created successfully. এই বার্তা আমাদের Terminal এ আসলেই বুজব লারাভেল আমাদের হয়ে একটি ফাইল তৈরি করেছে app/Http/Controllers পাথে, আমাদের দেওয়া নাম আনুসারে PagesController.php এবং আমাদের প্রয়োজনীয় কিছু code লিখেছে নিচের মতো।

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;

class PagesController extends Controller
{
    //
}
```

এবার এই class PagesController ভিতর আমাদের প্রয়োজন মতো কিছু method লিখে ফেলি।

```php
class PagesController extends Controller
{
    public function home(){
        return view('home');
    }

    public function about(){
        return view('about');
    }

    public function forms(){
        return view('pages.forms');
    }
}
```

এবার routes.php কেও update করে ফেলিঃ

```php
Route::group(['middleware' => ['web']], function () {

    Route::get('/', 'PagesController@home');

    Route::get('/about', 'PagesController@about');

    Route::get('/form', 'PagesController@forms');

});
```

এবার ব্রাউজারে একটু চেক করে নিন। যদি `NotFoundHttpException in RouteCollection.php ....` এই এররটি দেয় তাহলে দেখুন আপনার view গুলা আছে কিনা এবং আপনি সঠিক ভাবে address bar এ address টি লিখেছেন কিনা। আগের অনুশীলন গুলি করা থাকলে সমস্যা হবার কথা নয়।

আজকে এই পর্যন্তই। এর পরের চ্যাপ্টারে **মাইগ্রেশন** নিয়ে আলোচনা করা হবে।

