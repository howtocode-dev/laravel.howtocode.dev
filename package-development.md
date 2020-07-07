# প্যাকেজ ডেভেলপমেন্ট

## প্যাকেজঃ

পিএইচপি কিংবা কোন প্রোগ্রামিং ল্যাংগুয়েজে ব্যাবহৃত প্যাকেজ বলতে উক্ত ল্যাংগুয়েজে তৈরি লাইব্রেরীকে বোঝান হয়ে থাকে সেটা বিভিন্ন প্রজেক্টে বারবার ব্যাবহার উপযোগী। পিএইচপিতে ব্যাবহারের ক্ষেত্র অনুযায়ী প্যাকেজ সাধারণত দুই ধরনের হয়ে থাকে একটি হল স্ট্যান্ডঅ্যালন আরেকটি হল কোন নির্দিষ্ট ফ্রেমওয়ার্ক কিংবা স্ট্যাক ভিত্তিক।

## লারাভেল প্যাকেজঃ

লারাভেলের প্যাকেজ পিএইচপির স্ট্যান্ডঅ্যালন প্যাকেজ এর মতই শুধু পার্থক্য হল এটি লারাভেলের ব্যাবহার উপযোগী আর লারাভেল কেন্দ্রিক করেই তৈরি করা হয়।

একটি লারাভেল প্যাকেজ এর মধ্যে নিচের রিসোর্স গুলো থাকতে পারেঃ

1. Service Provider.
2. Configurations.
3. Migrations.
4. Seeds.
5. Views.
6. Routes.
7. Translations.
8. Assets etc.

**বিঃদ্রঃ** একটি লারাভেল প্যাকেজের মধ্যে **Service Provider** থাকতেই হবে।

লারাভেলের প্যাকেজ তৈরি এর আগে পিএইচপিতে কিভাবে প্যাকেজ তৈরি করতে হয় সেই সম্পর্কে ধারণা থাকলে ভাল হয়। আর এই জন্য এই বয়লারপ্লেটটি দেখে নিতে পারেন।

আমরা লারাভেলের প্যাকেজ **“illuminate/workbench”** এই প্যাকেজটি ব্যাবহার করেও প্যাকেজ জেনারেট করতে পারি কিন্তু সমস্যা হল এটি লারাভেল ভার্সন ৪.২ এর পর থেকে বাদ দেয়া হয়েছে। তাছাড়াও এটিতে আর সাপোর্ট দেয়া হয়না, আর ক্লাস অ্যাটোলোডিং এর জন্য PSR-0 ব্যাবহার করা হয়েছে যেটি ডেপ্রিকেটেড হয়েছে কিছুদিনের মধ্যে রিমুভ করা হবে। কাজেই আমরা আমাদের প্যাকেজ তৈরি করতে **“illuminate/workbench”** প্যাকেজটি ব্যাবহার করবনা আর **PSR-4** স্ট্যান্ডার্ড অনুসরণ করব।

## প্যাকেজ তৈরিকরণ প্রক্রিয়াঃ

লারাভেল কিংবা পিএইচপির প্রজেক্টে কম্পোজার প্যাকেজের জন্য **vendor** নামে একটি বাই ডিফল্ড ডিরেক্টরি থাকে। আপনার ভেন্ডর ও প্যাকেজ ডিরেক্টরি তৈরি করার জন্য উক্ত **vendor** ডিরেক্টরিতে ঢুকতে হবে। এবার ধরুন আপনার ভেন্ডর ডিরেক্টরির নাম দিলাম **my-vendor** আর এর ভিতরে প্যাকেজ ডিরেক্টরির জন্য **my-package** নাম দিলাম। মনে রাখবেন ডিরেক্টরি এর নাম সব সময় যেন ছোট হাতের লেখা হয়। এবার প্যাকেজের ইনফরমেশন এবং কনফিগারেশনের জন্য নিচের মত করে **composer.json** ফাইল তৈরি করতে হবে।

```javascript
{
    "name": "my-vendor/my-package",
    "license": "MIT",
    "description": "Description about your package",
    "keywords": [],
    "authors": [
        {
            "name": "Your Name",
            "email": "email@example.com"
        }
    ],
    "require": {
        "php": ">=5.5.9",
        "illuminate/support": "5.1.*"
    },
    "autoload": {
        "psr-4": {
            "MyVendor\\MyPackage\\": "src/"
        }
    }
}
```

এখানে **"name": "my-vendor/my-package"** এই সেকশনে আপনার ভেন্ডর এবং প্যাকেজ ডিরেক্টরির নাম দিতে হবে।

নিচের সেকশনে আপনার প্যাকেজের নেমস্পেস, সাব-নেমস্পেস দিতে হবে। আর **src** ডিরেক্টরির মধ্যে আমরা সব ফাইল রাখব এইজন্য **"src/"** দেয়া হয়েছে।

এবার একটি সার্ভিস প্রোভাইডার তৈরি করা শিখব। নিচে **"MyPackageServiceProvider.php"** নামে একটি স্যাম্পল প্রোভাইডার দেখানো হলঃ

```php
<?php

namespace MyVendor\MyPackage;

use Illuminate\Support\ServiceProvider;

class MyPackageServiceProvider extends ServiceProvider
{

    /**
     * Indicates if loading of the provider is deferred.
     *
     * @var bool
     */
    protected $defer = false;

    /**
     * Perform post-registration booting of services.
     *
     * @return void
     */
    public function boot()
    {

        // Loading routes
        if (!$this->app->routesAreCached()) {
            require __DIR__ . '/routes.php';
        }

        // Publishing configs
        $this->publishes([
            __DIR__ . '/config/my-package.php' => config_path('my-package.php'),
        ]);

        // Publishing views
        $this->publishes([
            __DIR__ . '/views' => base_path('resources/views'),
        ]);

        // Loading translations
        $this->loadTranslationsFrom(__DIR__ . '/translations', 'my-package');

        // Publishing public assets
        $this->publishes([
            __DIR__ . '/assets' => public_path('my-vendor/my-package'),
        ], 'public');

        // Publishing migrations
        $this->publishes([
            __DIR__ . '/migrations' => database_path('/migrations'),
        ], 'migrations');

        // Publishing seeds
        $this->publishes([
            __DIR__ . '/seeds' => database_path('/seeds'),
        ], 'migrations');

    }

    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function register()
    {
        $this->app->bind('MyPackageClass', 'MyVendor\MyPackage\MyPackageClass');
    }

}
```

উপরের সার্ভিস প্রোভাইডার ক্লাসে নেমস্পেস **namespace MyVendor\MyPackage;** দেয়া হয়েছে। আপনার প্যাকেজ তৈরি করার সময় আপনার মত করে এটি দিবেন।

এখানে **Illuminate\Support\ServiceProvider** ব্যাবহার করা হয়েছে আর এইটাকে এক্সটেন্ড করে প্যাকেজের সার্ভিস প্রোভাইডার ডিক্লেয়ার করা হয়েছে। এই লাইনে protected $defer = false; ডেফার মোড ফলস করা হয়েছে। ডেফার সার্ভিস প্রোভাইডার বলতে সার্ভিস প্রভাইডারটি সার্ভিস কন্টেইনারে রেজিস্টার হবে কিন্তু প্রয়োজন না হওয়া পর্যন্ত অ্যাপ্লিকেশনে লোড হবে না।

এবার দেখতে পাচ্ছেন **boot\(\)** মেথডটিতে কিছু রাউট, কনফিগারেশন, ডাটাবেস মাইগ্রেশন, সিডিং, ভিউস ফাইল আরও কিছু অ্যাসেটস লোড কিংবা পাবলিশের জন্য ডিক্লেয়ার করা হয়েছে। এসব রিসোর্স আপনার প্যাকেজে প্রয়োজন হলে স্যাম্পল কোডের মত করে ডিক্লেয়ার করবেন আর তার সাথে ডিরেক্টরি তৈরি করে ওই ডিরেক্টরির মধ্যে ফাইল গুলো রাখবেন। তবে প্যাকেজের মধ্যে এই রিসোর্স গুলো থাকা অপরিহার্য না। এখানে পাবলিশ বলতে রিসোর্স গুলো প্যাকেজ থেকে অ্যাপ্লিকেশন এর ভিতরে কাংখিত ডিরেক্টরিতে হস্তান্তর করা বুঝান হয়েছে।

**register\(\)** মেথডটির মধ্যে সাধারণত প্যাকেজের ক্লাস ডিপেন্ডেন্সি গুলো লারাভেলের সার্ভিস কন্টেইনারে বাইন্ড করার জন্য ডিক্লেয়ার করা হয়। নিচের লাইনে দেখলে বুঝতে পারবেন আমি **MyPackageClass** নামে একটি ক্লাস বাইন্ড করেছি।

```php
$this->app->bind('MyPackageClass', 'MyVendor\MyPackage\MyPackageClass');
```

**MyPackageClass** এর জন্য নিচের কোড লিখেছি।

```php
<?php

namespace MyVendor\MyPackage;

class MyPackageClass
{
    public static function sayHi()
    {
        return 'Hello World!';
    }
}
```

প্যাকেজটি ব্যাবহারের নিয়মঃ স্যাম্পল প্যাকেজটি আপনার অ্যাপ্লিকেশনে ব্যাবহার করতে চাইলে আমার [এই বয়লারপ্লেটটি](https://github.com/sohelamin/laravel-package-boilerplate) দেখতে কিংবা ব্যাবহার করতে পারেন। যদিও ব্যাবহারের নিয়ম বয়লারপ্লেটটিতে দেয়া আছে তারপরও নিচের মত করে কনফিগার করতে পারেন। আপনার লারাভেল প্রজেক্টের মেইন composer.json ফাইলে নিচের কোড যোগ করতে হবেঃ

```javascript
"psr-4": {
    "App\\": "app/",
    "MyVendor\\MyPackage\\": "vendor/my-vendor/my-package/src"
}
```

ক্লাস অ্যাটোলোডিং এর জন্য কম্পোজার ফাইলে প্যাকেজটিকে চিনিয়ে দেয়া হয়েছে তবে মনে রাখবেন যখন আপনি কম্পোজার প্যাকেজ এর জন্য "packagist.org" আপনার প্যাকেজটি সাবমিট কিংবা পাবলিশ করবেন তখন উপরের লাইন লেখার কোন দরকার নাই। এবার সার্ভিস প্রোভাইডারটি "config/app.php" ফাইলে নিচের মত করে যোগ করতে হবে।

```php
'providers' => [
    ...

    MyVendor\MyPackage\MyPackageServiceProvider::class,
],
```

এবার নিচের কমান্ড রান করেনঃ

```text
composer dump-autoload
composer update
```

আর প্যাকেজের রিসোর্স গুলো পাবলিশ করার জন্যঃ

```text
php artisan vendor:publish
```

এবার প্যাকেজের **MyPackageClass** টি অ্যাপ্লিকেশনের মধ্যে ব্যাবহারের জন্য নিচের মত করে লিখতে হবে।

```php
$myPackage = $this->app['MyPackageClass'];
echo $myPackage::sayHi();
```

অথবা নিচের মত করেও সরাসরি ব্যাবহার করা যাবে।

```php
echo \MyVendor\MyPackage\MyPackageClass::sayHi();
```

লারাভেলের প্যাকেজ ডেভেলপমেন্ট সম্পর্কে বিস্তারিত জানতে [এই লিঙ্ক](http://laravel.com/docs/5.1/packages) অনুসরণ করবেন। লারাভেলের সার্ভিস প্রোভাইডার সম্পর্কে বিস্তারিত জানতে [এই লিঙ্ক](http://laravel.com/docs/5.1/providers) অনুসরণ করবেন। সার্ভিস প্রোভাইডার সম্পর্কে বিস্তারিত জানতে [এই লিঙ্ক](http://laravel.com/docs/5.1/container) অনুসরণ করবেন।

এছাড়াও আপনারা আমার ডেভেলপ করা কিছু প্যাকেজ দেখে আইডিয়া নিতে পারেন। [https://github.com/appzcoder/](https://github.com/appzcoder/)

