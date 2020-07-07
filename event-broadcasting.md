# ইভেন্ট ব্রডকাস্টিং

ইভেন্ট হল কোন এক মুল এ্যাক্টিভিটি কিংবা ফাংশন কল করার সময় অন্য কোন বিশেষ টাস্ক কিংবা এ্যাক্টিভিটি কার্যকর করা। ধরুন ইউজার রেজিস্ট্রেশনের সময় ইউজারকে স্বাগতম ইমেইল পাঠাতে হবে সেটা ইভেন্ট ফায়ার আপ করে করা যায়। আর এই জন্য উক্ত ইভেন্টের জন্য একটা লিসেনার সেট করতে হবে যেটি ব্যকগ্রাউন্ডে ওই কাজটি করে দিবে।

লারাভেলে স্বাভাবিক ভাবেই ইভেন্ট আর লিসেনার সেট করে এই ধরনের কাজ করা যায়। এমনকি ইভেন্ট কিউইং করারও সিস্টেম আছে।

এবার মুল বিষয় হচ্ছে ইভেন্ট ফায়ার করা এবং তার সাথে সাথে সেটা ব্রডকাস্টিং করা যাতে করে সকল ইউজার কিংবা ক্লায়েন্ট উক্ত ইভেন্ট কে বুজতে পারে। আর সেটা নোটিফিকেশনের মাধ্যমে ইউজারদের কাছে পাঠানো হয়।

পিএইচপি কিংবা লারাভেলে **Pusher** লাইব্রেরী ব্যাবহার করে রিয়ালটাইম ইভেন্ট ব্রডকাস্ট করা যায় কিন্তু **Pusher** পেইড সার্ভিস এছাড়াও এর **HTTP Latency** অনেক বেশি অর্থাৎ রিকুয়েস্ট রেসপন্সে অনেক সময় যার কারণে আমরা **Redis** ব্যবহার করব যেটি **NodeJS, Socket.io** এর সাথে কাজ করে আর অনেক দ্রুত কাজ করে।

## প্রক্রিয়াঃ

প্রথমে আমাদেরকে ইভেন্ট আর লিসেনার তৈরি করে নিতে হবে। ইভেন্ট আর লিসেনার যথাক্রমে **app/Events** ও **app/Listeners** ডিরেক্টরিতে থাকে। যদিও ইভেন্ট ও লিসেনার আলাদা আলাদা করে আর্টিসান কমান্ডের মাধ্যমে জেনারেট করা সম্ভব কিন্তু আমরা এটি খুব সহজেই **EventServiceProvider** এ ডিফাইন করে আর্টিসান কমান্ড দিয়ে জেনারেট করব।

এবার ধরুন আমাদের ক্ষেত্রে আমরা **UserRegisteredEvent** নামে ইভেন্ট রাখব তাহলে নিচের মত করে কোডটি **app/Providers/EventServiceProvider.php** ফাইলে লিখতে হবেঃ

```php
protected $listen = [
    'App\Events\UserRegisteredEvent' => [
        'App\Listeners\UserRegisteredEventListener',
    ],
];
```

এরপর আর্টিসান কমান্ড **php artisan event:generate** রান করতে হবে। যার ফলে ইভেন্ট আর লিসেনার স্বয়ংক্রিয় ভাবে প্রয়োজন মাফিক জেনারেট হবে।

ইভেন্ট আর লিসেনার তৈরি করার পর কিছু পরিবর্তন করার ফলে নিচের মত হবে।

### ইভেন্টঃ

```php
<?php

namespace App\Events;

use App\Events\Event;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;

class UserRegisteredEvent extends Event implements ShouldBroadcast
{

    public $user;

    /**
     * Create a new event instance.
     *
     * @return void
     */
    public function __construct($user)
    {
        $this->user = $user;
    }

    /**
     * Get the channels the event should be broadcast on.
     *
     * @return array
     */
    public function broadcastOn()
    {
        return ['test-channel'];
    }
}
```

উপরের কোড এ খেয়াল করলে দেখবেন ইভেন্ট ক্লাসে আমরা **ShouldBroadcast** ইন্টারফেস ইমপ্লিমেন্ট করেছি যেটা ইভেন্ট ব্রডকাস্টের জন্য করতে হয়। আবার ক্লাসের কন্সট্রাক্টর ফাংকশনের প্যারামিটারে **$user** ভেরিয়েবল পাস করতেছি যেটা পাবলিক ভেরিয়েলে এসাইন থাকবে এবং পরবর্তীতে লিসেনার থেকে একসেস করা যাবে। এবার একবারে নিচের দিকে খেয়াল করলে দেখবেন **broadcastOn\(\)** মেথড/ফাংকশনটি নিচের মত করে ডিফাইন করা হয়েছে।

```php
public function broadcastOn()
{
    return ['test-channel'];
}
```

উক্ত ফাংকশনে রেডিসের একটা চ্যানেল দিতে হবে আর আমাদের ক্ষেত্রে **test-channel** নাম দিয়েছি। এটা যেকোনো নামে হতে পারে তবে যেহেতু এটি ইভেন্ট ব্রডকাস্টের জন্য সাবস্ক্রাইব করা হয় আর ক্লায়েন্ট-ইন্ডে একই চ্যানেল উল্লেখ করে লিসেন করতে হয় কাজেই একটি অর্থপূর্ণ নাম দেয়াই ভাল।

### ইভেন্ট লিসেনারঃ

```php
<?php

namespace App\Listeners;

use App\Events\UserRegisteredEvent;

class UserRegisteredEventListener
{
    /**
     * Create the event listener.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Handle the event.
     *
     * @param  UserRegisteredEvent  $event
     * @return void
     */
    public function handle(UserRegisteredEvent $event)
    {
        var_dump("User: " . $event->user);
    }
}
```

এখানে **handle\(\)** ফাংকশনে আমাদের ইভেন্ট ক্লাসটি ইনজেক্ট করা হয়েছে আর **$event-&gt;user** প্রোপার্টি একসেস করা হচ্ছে। এই প্রোপার্টি ইচ্ছেমত ইমপ্লিমেন্ট করা যায় আপাতত ডাম্প করে দেখানো হয়েছে।

এবার রাউট এ আমরা নিচের মত করে ডিফাইন করব।

```php
Route::get('broadcast', function () {
    event(new App\Events\UserRegisteredEvent('Sohel Amin'));

    return 'Event has been fired!';
});

Route::get('listen', function () {
    return view('events');
});
```

এখানে দুইটি রাউট ডিফাইন করা হয়েছে। **broadcast** রাউটের মাধ্যমে ইভেন্ট ফায়ার করার জন্য ইভেন্ট কল করা হয়েছে আর প্যারামিটারে নাম দেয়া হয়েছে উধাহরন হিসেবে যেটি লিসেনার এ **$event-&gt;user** হিসেবে একসেস করা যাবে। আর **listen** রাউটে একটা ভিউ ফাইল লোড করা হয়েছে যেটিতে নিচের মত কোড আছে।

```php
<!DOCTYPE html>
<html>
    <head>
        <title>Laravel Event Broadcasting</title>
        <link href="//fonts.googleapis.com/css?family=Lato:100" rel="stylesheet" type="text/css">
    </head>
    <body>

        <h2>User's List</h2>
        <ul id="user-list">
        </ul>

        <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.3.6/socket.io.min.js"></script>
        <script>
            var socket = io('http://localhost:3000');

            socket.on("test-channel:App\\Events\\UserRegisteredEvent", function(message){
                console.log(message);

                // Appending user to user's list
                var ul = document.getElementById("user-list");
                var li = document.createElement("li");
                li.appendChild(document.createTextNode(message.user));
                ul.appendChild(li);

            });
        </script>
    </body>
</html>
```

এখানে আমরা **socket.io** ব্যাবহার করেছি কাজেই ভিউ ফাইলে **socket.io** এর জেএস ফাইল ইনক্লুড করেছি এবং চ্যানেল আর ইভেন্ট এর নাম নেমস্পেস সহ ডিফাইন করেছি।

এবার ব্রডকাস্টিং এর কনফিগারেশন ফাইলে ডিফল্ট ড্রাইভার হিসেবে রেডিস ডিফাইন করব নিচের মত করে **config/broadcasting.php** এই ফাইলে।

```php
'default' => env('BROADCAST_DRIVER', 'redis'),
```

এর সাথে সাথে রেডিস এর পিএইচপি লাইব্রেরী কম্পোজারে অ্যাড করে নিতে হবে। আর এই জন্য নিচের কমান্ড লিখতে হবে।

```text
composer require predis/predis
```

এখন আমাদের এই কাজে **NodeJS, Redis** আর **Socket.io** লাগবে কাজেই এগুলা ইন্সটল করে নিতে হবে।

**NodeJS** ইন্সটল করা [এই লিংক](https://nodejs.org/en/download/) দেখে নিতে পারেন।

**Redis Server** ইন্সটল করা [এই লিংক](http://redis.io/download) থেকে দেখে নিতে পারেন।

এবার **socket.io** জন্য একটা স্ক্রিপ্ট নিচের মত করে লিখতে হবে যেটি **NodeJS** এ করা আর **ExpressJS** এও লেখা সম্ভব। আর এটি অ্যাপ্লিকেশনের রুট ডিরেক্টরিতে রাখতে হবে।

```javascript
var app = require('http').createServer(handler);
var io = require('socket.io')(app);

var Redis = require('ioredis');
var redis = new Redis();

app.listen(3000, function() {
    console.log('Server is running!');
});

function handler(req, res) {
    res.writeHead(200);
    res.end('');
}

io.on('connection', function(socket) {
    //
});

redis.psubscribe('*', function(err, count) {
    //
});

redis.on('pmessage', function(subscribed, channel, message) {
    message = JSON.parse(message);
    io.emit(channel + ':' + message.event, message.data);
});
```

স্ক্রিপ্টে খেয়াল করলে দেখবেন নোডের কিছু মডিউল কিংবা প্যাকেজ যেমনঃ **socket.io, ioredis** ব্যাবহার করা হয়েছে কাজেই আমাদেরকে **npm** প্যাকেজ ম্যানেজারটি ইন্সটল আর কনফিগার করার জন্য প্রজেক্ট ডিরেক্টরি থেকে টার্মিনালে নিচের কমান্ড রান করাতে হবে।

```text
npm install
npm install ioredis
npm install socket.io
```

এবার মোটামুটি সকল কাজ শেষ ইভেন্ট ব্রডকাস্ট করার জন্য প্রজেক্টটি রান করাতে হবে এইক্ষেত্রে নিচের ধাপ অনুসরণ করতে হবে।

### প্রজেক্ট/অ্যাপ্লিকেশন রান করার জন্য প্রজেক্ট ডিরেক্টরি থেকে টার্মিনালেঃ

```text
php artisan serve
```

### রেডিস সার্ভার রান করার জন্য অন্য টার্মিনালেঃ

```text
redis-server
```

### নোডের socket.js ফাইলটি রান করার জন্য প্রজেক্ট ডিরেক্টরি থেকে টার্মিনালেঃ

```text
node socket.js
```

পরিশেষে ইভেন্ট ব্রডকাস্ট করার জন্য [http://localhost:8000/broadcast/](http://localhost:8000/broadcast/) এই লিংকে হিট করতে হবে আর ইভেন্ট লিসেন কিংবা রিয়াল্টাইম নোটিফিকেশন দেখতে [http://localhost:8000/listen/](http://localhost:8000/listen/) এই লিংকে হিট করতে হবে আর এটি একাধিক ব্রাউজার থেকে দেখতে পারেন।

সোর্স কোডটি [এই লিংক](https://github.com/sohelamin/laravel-event-broadcast) থেকে দেখে নিতে পারেন। [https://github.com/sohelamin/laravel-event-broadcast](https://github.com/sohelamin/laravel-event-broadcast)

