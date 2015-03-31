#ব্লেড টেমপ্লেটিং

লারাভেল এর সাথে ব্লেড টেমপ্লেট ইঞ্জিন বিল্ড ইন ভাবে এসেছে। আর এটা ব্যাবহার করা খুবই সহজ।
ব্লেড টেমপ্লেটে কোড লেখার জন্য ভিউ ফাইলের এক্সটেনশন হতে হবে **.blade.php** ধরুন আপনার ভিউ ফাইলটির নাম **about** তাহলে ব্লেড এর ফরমেটে এটি হবে **about.blade.php** ।
এখন ব্লেড এর নিয়ম অনুসারে ভিউ পেজ গুলো সাজানোর জন্য একটা মাস্টার পেজ বানাতে হবে যেটাকে এক্সটেন্ড করে অন্য ভিউ গুলো বানাতে হবে।
তাহলে আমরা যদি **about** এর জন্য একটি ভিউ বানাই ব্লেড এর নিয়ম অনুযায়ী, প্রথমে **route** এ **about** ডিফাইন করতে হবে তারপর মাস্টার পেজ বানাইতে হবে তারপর **about** পেজটি বানাইতে হবে। 
নিচে ধারাবাহিক ভাবে কোড গুলো দেখানো হল।

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
{{-- */ $name='Sohel Amin' /* --}}
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


এখন আপনাদেরকে কখন **{{ }}** ডাবল কারলি ব্রাকেট ব্যাবহার করতে হয় আর কখন **{{{ }}}** ট্রিফল ব্রাকেট ব্যাবহার করতে হয় সেই সম্পর্কে বলব।


ডাবল কারলি ব্রাকেট ব্যাবহার করে সুধু ডাটা **echo** করা হয়। অপরদিকে ট্রিফল ব্রাকেট ব্যাবহার করলে **echo** করা ডাটা গুলো **HTML entities** এ রুপ ধারন করবে। তার মানে ডাটা আউটপুটের সময় **htmlentities()** ফাংশনটি কল হবে।

এইবার ফর্ম হেল্পার ব্যাবহার করে ফর্ম **echo** করার জন্য নিচের মত করে লিখতে হবে।

```php
{!! Form::open(['url' => 'action/url']) !!}

{!! Form::close() !!}
```

আজকে এই পর্যন্ত, এর পরের চ্যাপ্টারে **কন্ট্রোলার** সম্পর্কে আলোচনা করা হবে।

