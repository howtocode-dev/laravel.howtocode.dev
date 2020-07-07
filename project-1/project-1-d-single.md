# লিঙ্ক ও সিঙ্গেল পেইজ

গত পর্বে আমরা আমাদের ইনডেক্স পেইজ বানিয়েছিলাম, আজ পোস্ট এ ক্লিক করে পুরা পোস্টটি দেখতে নতুন রাউট, কন্ট্রোলার ও ভিউ বানাবো।

আসুন প্রথমে আমাদের রুট বানিয়ে ফেলি routes.php ফাইলে।

```php
Route::get('post/{id?}', 'PostsController@show');
```

এবার project.one/app/Http/Controllers/PostsController.php এর ভিতর show মেথডটি বানাই।

```php
public function show($id){
        $post = Post::find($id);
        if($post == null) return redirect(action('PostsController@index'));
        return view('posts.single', compact('post'));
    }
```

এবার project.one/resources/views/posts ডাইরেক্টরি তে single.blade.php নামে নিচের মতো ভিউ বানাই।

```markup
@extends('master')
@section('title', '|| Post')
@section('content')
    <div class="col-md-12">

        <article>
            <h1>{!! $post->title !!}</h1>
            <div>
                <p class="post-info"><span>Created at: {{$post->created_at->format('d-M-Y')}}</span></p>
            </div>
            <div class="single-post-content">
                {!! $post->content !!}
            </div>
        </article>
    </div>
@endsection
```

ইনডেক্স পেইজ এর লিঙ্ক এ ক্লিক করলে যেন এই ভিউ তে আসে সেজন্য পোস্ট এর লিঙ্কটা project.one/resources/views/posts/index.blade.php ফাইলে পরিবর্তন করে দিয়ে আসি, পরিবর্তনের পর ফাইলটি হবে এরকমঃ

```markup
@extends('master')
@section('title', 'Posts')
@section('content')
    <div class="col-md-12">
        @if (count($posts))
            @foreach($posts as $post)
                <article>
                    <a href="{!! action('PostsController@show', $post->id) !!}"><h1>{!! $post->title !!}</h1></a>
                    <div>
                        <p class="post-info"><span>Created at: {{$post->created_at->format('d-M-Y')}}</span></p>
                    </div>
                    <div class="post-excerpt">
                        {!! str_limit($post->content, 150) !!}
                    </div>
                </article>
                <hr>
            @endforeach
        @else
            <h1>Hi, you've landed in our First Prject!</h1>
            <h2>Sorry, We don't have any post right now.</h2>
        @endif
    </div>
@endsection
```

## কি ভাবে সোর্স কোড পাওয়া যাবে?

গিটহাব এর [project.one](https://github.com/robertbiswas/project.one) রিপজিটোরিটি ফোর্ক করুন এবং আপনার সিস্টেমে ক্লোন করুন।

এই অধ্যায়ের সোর্স কোড পেতে

```bash
git checkout dcf3ab9
```

সর্বশেষ কমিট পর্যন্ত পেতে আবার নিচের কমান্ডটি দিন

```bash
git checkout master
```

**কথাও বুঝতে সমস্যা হোলে নিচে কমেন্ট করুন, প্লিজ।**

