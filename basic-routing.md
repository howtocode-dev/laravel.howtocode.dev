# বেসিক রাউটিং

সফটওয়্যার আর্কিটেকচারে রাউট হচ্ছে মৌলিক উপাদান\(Basic Component\). রাউট URL\(URL = Uniform Resource Locator\)

থেকে অনুরোধ গ্রহণ করে এবং এপ্লিকেশনকে রিসোর্সের জন্য নির্দেশনা প্রদান করে। লারাভেলের রাউট সমূহ একই সাথে সুবিন্যস্ত করে রাখার জন্য **routes/web.php** তে লিখা হয়।

এর সবচেয়ে বড় সুবিধা হচ্ছে আপনি এক যায়গা থেকেই সমস্ত রাউটকে নিয়ন্ত্রণ করতে পারবেন অর্থাৎ পরবর্তিতে রাউট সম্পর্কিত যেকোনো ধরনের পরিবর্তন এখান থেকেই করতে পারবেন।

উদাহরণ হিসেবে একটি সাধারণ রাউট তুলে ধরা হল।

```php
Route::get('/', function()
{
  return view('hello');
});
```

উপরোক্ত উদাহরণে Route class এর get\(\) মেথডকে কল করা হয়েছে। get\(\) মেথডটি দুইটি আর্গুমেন্ট গ্রহণ করে। প্রথম আর্গুমেন্ট হিসেবে পাথ \(URL Path\) এবং দ্বিতীয় আর্গুমেন্ট হিসেবে ক্লোজার\(Anonymous Function\) যেমনঃ উপরোক্ত উদাহরণে প্রথম আর্গুমেন্ট হিসেবে ফরয়ার্ড স্লাশ / ব্যবহার করা হয়েছে যার দ্বারা রুট ডোমেইনকে নির্দেশ করা হচ্ছে। দ্বিতীয় আর্গুমেন্ট হিসেবে ক্লোজার ব্যবহার করা হয়েছে যার দ্বারা একশন সম্পাদিত হচ্ছে। অর্থাৎ উক্ত রাউটটি **resources/views/hello.php** ফাইলকে রিটার্ন করবে।

কি বিষয়টু একটু ঘোলা মনে হচ্ছে? সমস্যা নাই চলুন আমরা আর একটি উদাহরণের মাধ্যমে স্বচ্ছ ধারণা লাভ করি।

```php
Route::get('books', function()
{
    return 'Hey, I am laravel!';
});
```

এখন যদি আপনি ব্রাউজার এর Address Bar এ **/books \(**[http://localhost/your-project-name/books](http://localhost/your-project-name/books)**\)** লিখে হিট করেন, তাহলে দেখতে পাবেন Hey, I am laravel!

অর্থাৎ get\(\) মেথডটিতে প্রথম আর্গুমেন্ট হিসেবে books যা পাথ নির্দেশ করে এবং দ্বিতীয় আর্গুমেন্ট ক্লোজার যা দ্বারা কাঙ্ক্ষিত একশন সম্পাদিত হচ্ছে।

লারাভেলে রাউট মুলত দুই ভাবে লেখা যায়।

১\) resource ডিফাইন করে রাউটারের উদাহরণঃ

```php
Route::resource('users', 'UserController');
```

২\) controller ডিফাইন করে রাউটারের উদাহরণঃ

```php
Route::controller('users', 'UserController');
```

এ ক্ষেত্রে Controller class টি হবে।

```php
class UserController extends BaseController {
  public function getIndex()
  {
    return 'I am restful index';
  }
}
```

**নোট :** এই দুই পদ্ধতি যথাযথভাবে কাজ করবে যদি আপনার Controller এবং Method সমূহ RESTful হয়। RESTful Controller সম্পর্কে এখান থেকে [বিস্তারিত](https://laravel.com/docs/master/controllers#resource-controllers) পড়ুন

## অনুশীলনঃ

কোন কিছু শেখার সময় হাতে কলমে করতে পারলে শেখাটা ভাল হয় তাই আমরা চেষ্টা করবো কিছু অনুশীলন করতে।

দ্বিতীয় অধ্যায়ে আমরা শিখেছিলাম লারাভেল ইন্সটল করতে, আসুন blog.app নামে একটি লারাভেল অ্যাাপ বানাই ও বেসিক রাউটিং অনুশীলন করি, পরবর্তীতে আমরা আরও নতুন নতুন জিনিস শিখবো ও এটাকে উন্নত করব।

আমাদের নিজস্ব এনভায়রনমেন্ট\(htdocs, www etc.\) এ যাই ও টার্মিনালে নিচের কমান্ড দেই

```bash
composer create-project laravel/laravel blog.app
```

এই কমান্ড টি আমাদের ওয়ার্কিং ফোল্ডারে blog.app নামে একটি directory বানিয়ে তার ভিতরে লারাভেল এর যাবতীয় ফাইল ইন্টারনেট থেকে নামিয়ে নিবে।

আশাকরি ব্রাউজারে আপনি সাইটটি দেখতে পারছেন ।

আসুন নতুন রাউট বানাই ঃ

আমরা যদি **routes/web.php** ফাইলটি খুলি তাহলে এটা দেখবো

```php
Route::get('/', function () {
    return view('welcome');
});
```

আসুন নতুন কিছু রুট বানাই এটাকে পরিবর্তন করে। এই মুহূর্তে আমরা সৌন্দর্যের কথা না ভেবে শুধু কার্যকরীটা দেখি।

```php
Route::get('/', function () {
    return 'This is our home page.';
});

Route::get('/about', function () {
    return 'This is our about page.';
});

Route::get('/contact', function () {
    return 'This is our contact page.';
});

//Lets make some group route
Route::group(['prefix' => 'admin'], function () {
    Route::get('/', function () {
        return 'This is our Admin Dashboard';
    });
    // this link: blog.app/admin/

    Route::get('/user-list', function () {
        return 'This is our Admin Dashboard user list page';
    });
    // this link: blog.app/admin/user-list

    Route::get('/create-blog', function () {
        return 'This is our Admin Dashboard create-blog page';
    });
    //// this link: blog.app/admin/create-blog
});
```

এখানে নতুন হলো রুট গ্রুপ করা। 'prefix' ব্যবহার করে গ্রুপ তৈরি করেছি। আশা করি বুজতে পারছেন। এখন একটু কঠিন মনে হলেও পরবর্তী অধ্যায় করতে করতে সব আয়ত্তে এসে যাবে ।

পরবর্তী চ্যাপ্টারে **ভিউ** নিয়ে আলোচনা করা হবে।

