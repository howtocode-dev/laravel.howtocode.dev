# ব্লেড টেমপ্লেট

লারাভেল এর সাথে ব্লেড টেমপ্লেট ইঞ্জিন বিল্ড ইন ভাবে এসেছে। আর এটা ব্যাবহার করা খুবই সহজ। ব্লেড টেমপ্লেটে কোড লেখার জন্য ভিউ ফাইলের এক্সটেনশন হতে হবে **.blade.php** ধরুন আপনার ভিউ ফাইলটির নাম **about** তাহলে ব্লেড এর ফরমেটে এটি হবে **about.blade.php** । এখন ব্লেড এর নিয়ম অনুসারে ভিউ পেজ গুলো সাজানোর জন্য একটা মাস্টার পেজ বানাতে হবে যেটাকে এক্সটেন্ড করে অন্য ভিউ গুলো বানাতে হবে। তাহলে আমরা যদি **about** এর জন্য একটি ভিউ বানাই ব্লেড এর নিয়ম অনুযায়ী, প্রথমে **route** এ **about** ডিফাইন করতে হবে তারপর মাস্টার পেজ বানাইতে হবে তারপর **about** পেজটি বানাইতে হবে। নিচে ধারাবাহিক ভাবে কোড গুলো দেখানো হল।

**Route** এর জন্য।

```php
Route::get('/about', function(){
    return view('about');
});
```

মাস্টার পেজের জন্য।

```php
<!-- Stored in resources/views/layouts/master.blade.php -->

<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Laravel Project</title>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css">
    </head>
    <body>
        @yield('content')
        @yield('footer')
    </body>
</html>
```

**About** পেজের জন্য।

```php
<!-- Stored in resources/views/about.blade.php -->

@extends('master')

@section('content')
<div class="container">
    <div class="row">
        <div class="col-md-10">
            <p>Your contents goes here</p>
        </div>
    </div>
</div>
@endsection

@section('footer')
    <script src="{{ URL::asset('assets/js/myscript.js') }}"></script>
@stop
```

এখন ধরুন আপনাকে বিশেষ ক্ষেত্রে কনটেন্ট সেকশন অথবা অন্য কোন সেকশনের জন্য ডিফল্ট কনটেন্ট/ডাটা রাখতে হচ্ছে তাহলে আপনাকে **yield** মেথডটিতে দ্বিতীয় প্যারামিটার পাস করে এটা করতে হবে।

```php
@yield('section', 'Default Content')
```

ব্লেডে ডাটা **echo** করার জন্য ডাবল কারলি ব্রাকেট ব্যাবহার করতে হয়।

```php
Hello, {{ $name }}.

The current UNIX timestamp is {{ time() }}.
```

এইক্ষেত্রে ভেরিয়েবল ডিফাইন করার জন্য ব্লেডের কমেন্টস ব্যাবহার করতে হবে নিচের মত করে।

```php
@php
    $name = 'Sohel Amin';
@endphp

Hello, {{ $name }}
```

এবার ব্লেডে কন্ট্রোল স্ট্রাকচার সহ অন্যান্য ফিচার গুলো ধারাবাহিক ভাবে দেখানো হল।

**If Statements** এর জন্য নিচের পদ্ধ্যতি অনুসরন করুন।

```php
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif

@unless (Auth::check())
    You are not signed in.
@endunless
```

**Loops** এর জন্য নিচের পদ্ধ্যতি অনুসরন করুন।

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

একটা ভিউ ফাইলে অন্য ভিউ ফাইল ইনক্লুড করার জন্য নিচের মত করে লিখতে হবে।

```php
@include('view.name')
```

আর আপনি চাইলে ভিউ ফাইল ইনক্লুড করার সমই ডাটা পাস করতে পারেন এইভাবে।

```php
@include('view.name', ['some' => 'data'])
```

কমেন্টস লিখতে চাইলে।

```php
{{-- This comment will not be in the rendered HTML --}}
```

## ব্লেড টেমপ্লেটিংঃ HTML ও Forms

লারাভেল এর ব্লেড টেমপ্লেট ব্যবহার করে HTML ও Forms তৈরি করতে আমাদের একটি "Third party package" প্রয়োজন হবে। আসুন জেনে নেই কি ভাবে আমরা এই সুবিধা আমাদের প্রোজেক্ট এ আনতে পারি।

প্রোজেক্ট ডিরেক্টরি এর মুল পাথে `composer.json` নামে একটি ফাইল আছে যেটা আমাদের প্রোজেক্ট এর প্রয়োজনীয় প্যাকেজ গুলার তালিকা রাখে। ফাইলটির প্রথম কিছু অংশ অনেকটা এরকমঃ

```javascript
{
    "name": "laravel/laravel",
    "description": "The Laravel Framework.",
    "keywords": ["framework", "laravel"],
    "license": "MIT",
    "type": "project",
    "require": {
        "php": ">=5.6.4",
        "laravel/framework": "5.3.*"
    },
        .
        .
        .
        .
```

ফাইলে "require" অংশে আমাদের প্রয়োজনীয় প্যাকেজটি যোগ করি, তাহলে এটা দেখতে হবে অনেকটাঃ

```javascript
{
    "name": "laravel/laravel",
    "description": "The Laravel Framework.",
    "keywords": ["framework", "laravel"],
    "license": "MIT",
    "type": "project",
    "require": {
        "php": ">=5.6.4",
        "laravel/framework": "5.3.*",
        "laravelcollective/html": "5.3.*"
    },
        .
        .
        .
        .
```

এবার "Composer" ব্যবহার করে এটাকে আমাদের প্রোজেক্ট এ যোগ করি নিচের command টি Terminal এ লিখে

```bash
composer update
```

এটা আমাদের সব প্যাকেজ গুলোকে update, প্রয়োজনে add করে নিবে।

এবার `config/app.php` ফাইলটি খুলি, যেটা সব "provider" ও এর 'alias' এরে এর মধ্যে রাখে এবং অ্যাপ run করার সময় autoloader এর মাধ্যমে এগুলোকে load করে।

```php
Collective\Html\HtmlServiceProvider::class,
```

লাইনটি provider এরে এর শেষে যোগ করি।

```php
'providers' => [
     // ... previous providers classes

     Collective\Html\HtmlServiceProvider::class,

 ],
```

শেষ কাজ aliases এরে তে দুইটি class এর alias যোগ করা

```php
'aliases' => [
    // ... previous aliases
        'Form' => Collective\Html\FormFacade::class,
        'Html' => Collective\Html\HtmlFacade::class,
],
```

আমরা প্রস্তুত ব্লেড টেমপ্লেট ব্যবহার করে HTML ও Forms তৈরি করত!!!!

উদাহরণ দেখার আগে একটি বিষয় জেনে নেই

```php
{{ $data }}
```

escaped value দেখাবে

```php
{!! $data !!}
```

unescaped value দেখাবে

একটু উদাহরণ দেই

```php
$data = '<strong> How to Code BD </strong>'
{{ $data }}
```

দেখাবেঃ `<strong> How to Code BD </strong>`

```php
$data = '<strong> How to Code BD </strong>'
{!! $data !!}
```

দেখাবেঃ **How to Code BD**

এইবার ফর্ম হেল্পার ব্যাবহার করে ফর্ম **echo** করার জন্য নিচের মত করে লিখতে হবে।

```php
{!! Form::open(['url' => '#']) !!}

{!! Form::close() !!}
```

এবং HTML আউটপুট হবে

```markup
<form accept-charset="UTF-8" action="#" method="POST">
    <input type="hidden" value="hbyt7fA7MW09iT8V54z2V5u8j0mFFJJckSs7XI9G" name="_token">

</form>
```

এখানে 'POST' মেথড দিবে যদি অন্য কোন মেথড আমরা উল্লেখ না করি। সাথে একটি `html <input type="hidden" value="hbyt7fA7MW09iT8V54z2V5u8j0mFFJJckSs7XI9G" name="_token">` যা আমাদের সিকুইরিটি বাড়াবে।

এবার, নানা ধরনের `input element` গুলো আমাদের ফর্মে যোগ করিঃ

```markup
<div id="simple-form">
        <h3>User Profile</h3>
        <hr/>
        {!! Form::open(array('url' => '#')) !!}

        {!! Form::label('user_name', 'Name:') !!}
        {!! Form::text('user_name') !!}
        <br>
        {!! Form::label('email', 'Email:') !!}
        {!! Form::email('email', $value = null) !!}
        <br>
        {!! Form::label('gender', 'Gender:') !!}
        {!! Form::radio('gender', 'Male', true) !!} Male
        {!! Form::radio('gender', 'Female', false) !!} Female
        {!! Form::radio('gender', 'Unisex', false) !!} Unisex
        <br>
        {!! Form::label('role', 'Role:') !!}
        {!! Form::select('role', array(
            'admin' => 'Administrator',
            'author' => 'Author',
            'subscriber' => 'Subscriber',
            'registered' => 'registered'

            )) !!}
        <br>
        {!! Form::label('about', 'About Me') !!}<br>
        {!! Form::textarea('about') !!}
        <br><br>
        {!! Form::submit('Update') !!}
        {!! Form::close() !!}
    </div>
```

ব্রাউজার দেখাবেঃ   
 ![blade-simple-form-output](.gitbook/assets/blade-simple-form-output.png)

HTML আউটপুট হবেঃ

```markup
<div id="simple-form">
        <h3>User Profile</h3>
        <hr>
        <form accept-charset="UTF-8" action="#" method="POST"><input type="hidden" value="hbyt7fA7MW09iT8V54z2V5u8j0mFFJJckSs7XI9G" name="_token">
            <label for="user_name">Name:</label>
            <input type="text" id="user_name" name="user_name">
            <br>
            <label for="email">Email:</label>
            <input type="email" id="email" name="email">
            <br>
            <label for="gender">Gender:</label>
            <input type="radio" id="gender" value="Male" name="gender" checked="checked"> Male
            <input type="radio" id="gender" value="Female" name="gender"> Female
            <input type="radio" id="gender" value="Unisex" name="gender"> Unisex
            <br>
            <label for="role">Role:</label>
            <select name="role" id="role">
               <option value="admin">Administrator</option>
               <option value="author">Author</option>
               <option value="subscriber">Subscriber</option>
               <option value="registered">registered</option>
                  </select>
            <br>
            <label for="about">About Me</label><br>
            <textarea id="about" rows="10" cols="50" name="about"></textarea>
            <br><br>
            <input type="submit" value="Update">
        </form>
    </div>
```

কিন্তু আমরা এই ফর্ম এর সাথে CSS দিবো, আমাদের id, class ও নানা ধরনের attribute ও যোগ করতে হবে। পূর্ববর্তী অনুশীলনে আমরা Twitter Bootstrap দিয়েছিলাম, এবার এই অনুশীলনে

* ভিউ তে blade ব্যবহার করে ভিউ আরও ব্যবহার যোগ্য করবো
* Form এর জন্য নতুন Template বানাবো, রাউট সেট করবো
* লারাভেল blade ও Twitter Bootstrap মিলিয়ে একটি Form তৈরি করবো

অনুশীলনঃ গত অধ্যায় এ আমরা ভিউ এবং এই অধ্যায় এ blade template দেখলাম। আসুন একটি master.blade.php ফাইল বানাই `resources/views` এর ভিতর।

```markup
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>@yield('title')</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
            @yield('content')
    </div>

    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
</body>
</html>
```

`@yield('title')` ও `@yield('content')` এর মানে title ও content নামে চাইল্ড টেমপ্লেটে কোনও section থাকলে এই অংশে দেখাবে। এটা আমারা একটি "Parent template" বানালাম।

এবার একটি "Child Template" বানাই উপরের blade template ব্যবহার করে যে Form টি বানিয়েছি সেটা নিয়ে। `resources/views` এর ভিতর pages নামে একটি ডাইরেক্টরি ও তার ভিতর `form.blade.php` নামে ফাইল তৈরি করি। এবং নিচের code গুলা লিখি।

```markup
@extends('master')

@section('title', 'Forms')

@section('content')
    <div class="row">
        <div class="col-md-6 col-md-offset-3">
            <h3>User Profile</h3>
            <hr/>
            {!! Form::open(array('url' => '#')) !!}

            {!! Form::label('user_name', 'Name:') !!}
            {!! Form::text('user_name') !!}
            <br>
            {!! Form::label('email', 'Email:') !!}
            {!! Form::email('email', $value = null) !!}
            <br>
            {!! Form::label('gender', 'Gender:') !!}
            {!! Form::radio('gender', 'Male', true) !!} Male
            {!! Form::radio('gender', 'Female', false) !!} Female
            {!! Form::radio('gender', 'Unisex', false) !!} Unisex
            <br>
            {!! Form::label('role', 'Role:') !!}
            {!! Form::select('role', array(
                'admin' => 'Administrator',
                'author' => 'Author',
                'subscriber' => 'Subscriber',
                'registered' => 'registered'

                )) !!}
            <br>
            {!! Form::label('about', 'About Me') !!}<br>
            {!! Form::textarea('about') !!}
            <br><br>
            {!! Form::submit('Update') !!}
            {!! Form::close() !!}
        </div>
    </div>
@endsection
```

তিনটি জিনিষ খেয়াল করার আছেঃ

`@extends('master')` মানে এই ফাইলটি views এর ভিতর যে master.blade.php আছে সেটি ব্যবহার করবে। `@section('title', 'Forms')` এটি section, প্রথম প্যারামিটারটি section এর নাম ও দ্বিতীয় প্যারামিটারটি section এর value. Valueটি যদি এমন একটি স্ট্রিং হয় তো এভাবেই লেখা যায়। কিন্তু দেখেন content নামের sectionটি শুধু প্রথম প্যারামিটারটি ব্যবহার করে নামটি দিয়েছে পরে এর বিশাল value দিয়েছে এবং অবশেষে @endsection লিখে sectionটি শেষ করেছে।

ফাইল তো হল, এদের জন্য রাউট তৈরি করি। routes/web.php ফাইলটি খুলে নিচের মত রাউট বানাই। আগের অনুশীলন গুলো করে থাকলে এটা আপনার জন্য খুবি সহজ।

```php
Route::get('/form', function () {
        return view('pages.forms');
    });
```

এখানে pages.forms লেখার কারণ pages ডাইরেক্টরি এর মধ্যে forms.blade.php ফাইলটি আছে। আর লারাভেল তো জানেই ভিউগুলা কোথায় থাকে।

আসুন এবার Twitter Bootstrap এর সাথে আমাদের HTML Blade মিক্সাপ করি।

আপনার forms.blade.php ফাইলটি আপডেট করুনঃ

```markup
<div class="col-md-6 col-md-offset-3">
            <h3>User Profile</h3>
            <hr/>
            {!! Form::open(array('url' => '#', 'class'=>'form-horizontal')) !!}
            <div class="form-group">
                {!! Form::label('user_name', 'Name:') !!}
                {!! Form::text('user_name', '', array('class' =>'form-control', 'placeholder'=>'User Name')) !!}
            </div>
            <div class="form-group">
                {!! Form::label('email', 'Email:') !!}
                {!! Form::email('email', $value = null, array('class' =>'form-control', 'placeholder'=>'Email Address')) !!}
            </div>
            <div class="form-group">
                {!! Form::label('gender', 'Gender:') !!}
                {!! Form::radio('gender', 'Male', true) !!} Male
                {!! Form::radio('gender', 'Female', false) !!} Female
                {!! Form::radio('gender', 'Unisex', false) !!} Unisex
            </div>
            <div class="form-group">
                {!! Form::label('role', 'Role:') !!}
                {!! Form::select('role', array(
                    'admin' => 'Administrator',
                    'author' => 'Author',
                    'subscriber' => 'Subscriber',
                    'registered' => 'registered'
                    ),null, ['class' => 'form-control']) !!}
            </div>
            <div class="form-group">
                {!! Form::label('about', 'About Me') !!}<br>
                {!! Form::textarea('about','',array('class'=>'form-control', 'rows' => '4')) !!}
            </div>
            {!! Form::submit('Update Profile', array('class' => 'btn btn-primary')) !!}
            {!! Form::close() !!}
        </div>
```

ব্রাউজার দেখাবেঃ   
 ![mix-blade-bootstrape-form](.gitbook/assets/mix-blade-bootstrape-form.png)

HTML আউটপুট হবেঃ

```markup
<form class="form-horizontal" accept-charset="UTF-8" action="#" method="POST"><input type="hidden" value="ZQ6nS9ifenMpro11inQ0azWGrnz5kRoi7lQ3UEdz" name="_token">
    <div class="form-group">
        <label for="user_name">Name:</label>
        <input type="text" id="user_name" value="" name="user_name" placeholder="User Name" class="form-control">
    </div>
    <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" placeholder="Email Address" class="form-control">
    </div>
    <div class="form-group">
        <label for="gender">Gender:</label>
        <input type="radio" id="gender" value="Male" name="gender" checked="checked"> Male
        <input type="radio" id="gender" value="Female" name="gender"> Female
        <input type="radio" id="gender" value="Unisex" name="gender"> Unisex
    </div>
    <div class="form-group">
        <label for="role">Role:</label>
        <select name="role" id="role" class="form-control"><option value="admin">Administrator</option><option value="author">Author</option><option value="subscriber">Subscriber</option><option value="registered">registered</option></select>
    </div>
    <div class="form-group">
        <label for="about">About Me</label><br>
        <textarea id="about" cols="50" name="about" rows="4" class="form-control"></textarea>
    </div>
    <input type="submit" value="Update Profile" class="btn btn-primary">
</form>
```

কি লারাভেল Blade templating মজার না?

আজকে এই পর্যন্ত, এর পরের চ্যাপ্টারে **কন্ট্রোলার** সম্পর্কে আলোচনা করা হবে।

