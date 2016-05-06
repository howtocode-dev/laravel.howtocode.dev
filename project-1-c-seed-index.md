# সিড(Seed) ও ইনডেক্স পেইজ  
***
##প্রস্তুতি
যেহেতু এই প্রোজেক্টটি একটি blog এর মতো হবে তাই হোম বা ইনডেক্স পেইজ এ আমাদের পোস্ট গুলোর লিস্ট থাকাই স্বাভাবিক।

### posts টেবিল এর জন্য Post মডেল তৈরিঃ
[মডেল](http://laravel.howtocode.com.bd/model.html) অধ্যায়ে আমার এই বিষয় নিয়ে বেশ আলোচনা করেছিলাম, চাইলে একবার ঘুরে আসতে পারেন।

আসুন নিচের কমান্ডটি দেইঃ
```bash
php artisan make:model Post
```
```project.one/app/Post.php``` ফাইলটি আমরা পাবো, সাধারণ নিয়ম অনুসারে Post ক্লাসটি, গত অধ্যায়ে migrate করা posts টেবিলটাকেই গ্রহণ করবে।
আসুন টেবিলটি দেখে নেইঃ
![pro-1-blog-posts-table](images/pro-1-blog-posts-table.png)

এবার Post.php ফাইলটি খুলি
```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    //
}

```
এবং নিচের মতো পরিবর্তন করি
```php
class Post extends Model
{
    protected $guarded = ['status'];
    protected $fillable = ['title', 'content'];
}
```
অংশটি বোঝার জন্য [এইখানে দেখুন](http://laravel.howtocode.com.bd/model.html#massassignment-vulnerability) ।

### posts টেবিল এ কিছু রেকর্ড ইন্সার্ট করি ডেভেলপমেন্ট এর সুবিধার জন্য
আমরা আগেই [Tinker](http://laravel.howtocode.com.bd/model.html#tinker-%E0%A6%A8%E0%A6%BF%E0%A7%9F%E0%A7%87-%E0%A6%95%E0%A6%BF%E0%A6%9B%E0%A7%81-%E0%A6%AE%E0%A6%9C%E0%A6%BE) ব্যবহার করে কিভাবে টেবিলে ডাটা ইন্সার্ট করা যায় তা দেখেছিলাম। আজ দেখবো Seeding ।

## সিডিং(Seeding)
লারাভেল ডাটাবেজে টেস্ট ডাটা ইন্সার্ট করার জন্য সাধারণ একটি method যোগ করেছে, যার নাম সিডিং(Seeding)। সব সিড ক্লাস ```database/seeds ``` ডাইরেক্টরিতে থাকে। সিড ক্লাস এর নাম আপনি যেকোনো কিছুই দিতে পারেন, কিন্তু কিছু রীতি মেনে নাম দেয়াই ভালো, যেমন আমাদের posts টেবিলের সিডার ক্লাস এর নাম হতে পারে ``` PostsTableSeeder ```।

আসুন তাহলে আমাদের সিডার বানিয়ে ফেলি নিচের কমান্ড দিয়েঃ
```bash
php artisan make:seeder PostsTableSeeder
```
আমরা নিশ্চয় ```project.one/database/seeds/PostsTableSeeder.php``` ফাইলটি পাবো।
```php
<?php

use Illuminate\Database\Seeder;

class PostsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        //
    }
}
```
আমাদের ফাইলটাকে পরিবর্তন করার আগে আসুন ```project.one/database/seeds/DatabaseSeeder.php``` ফাইল এ একটু পরিবর্তন আনি নিচের মতো।

```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $this->call(PostsTableSeeder::class);
    }
}
```
এবার ```project.one/database/seeds/PostsTableSeeder.php``` ফাইলটাকে পরিবর্তন করি

```php
public function run()
    {
        DB::table('posts')->insert([
            'title' => 'Seeding Title',
            'content' => 'Seeding content'
        ]);
    }
```
এবার নিচের কমান্ডটি দিন ও ডাটাবেজে টেবিলটির পরিবর্তন লক্ষ্য করুন।
```bash
php artisan db:seed
```
<div class='caution-div'>
<span class='caution'>সতর্কতাঃ</span> আপনি যদি নিচের মতো এরর দেখেন
<pre>
<code class="lang-bash">[ReflectionException]                  
Class PostsTableSeeder does not exist</code>
</pre>

তাহলে নিচের কমান্ডটি দিন।
<pre>
<code class="lang-bash">composer dump-autoload
</code></pre>
আবার db::seed কমান্ডটি দিন ও ডাটাবেজে টেবিলটির পরিবর্তন লক্ষ্য করুন।
</div>

**কিন্তু Tinker বা উপরের মতো করে টেস্ট ডাটা ডাটাবেজে ঢুকানো বড়ই কষ্টকর তাই লারাভেল টেস্ট ডাটার কারখানা বানিয়ে দিয়েছে, আসুন তার সাথে পরিচিত হই।**

### মডেল ফ্যাক্টরি(model factory) এর ব্যবহারঃ
```project.one/database/factories/ModelFactory.php``` ফাইলটিতে নিচের মতো করে একটি ফ্যাক্টরি ডিফাইন করি।
```php
$factory->define(App\Post::class, function (Faker\Generator $faker) {
    return [
        'title' => $faker->sentence(mt_rand(3, 10)),
        'content' => join("\n\n", $faker->paragraphs(mt_rand(3, 6))),
        'created_at' => $faker->dateTimeBetween('-1 month', '+3 days'),
    ];
});
```

```project.one/database/seeds/DatabaseSeeder.php``` ফাইলটি আগের মতোই থাকুক।
```php
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $this->call(PostsTableSeeder::class);
    }
}
```
এবার ```project.one/database/seeds/PostsTableSeeder.php``` ফাইলটাকে পরিবর্তন করি
```php
<?php

use Illuminate\Database\Seeder;

class PostsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        App\Post::truncate(); // delete all previous rows

        factory(App\Post::class, 10)->create(); // Create 10 rows
    }

}
```
এবার নিচের কমান্ডটি দিন ও ডাটাবেজে টেবিলটির পরিবর্তন লক্ষ্য করুন।
```bash
php artisan db:seed
```
**আপনি এভাবে মডেল ফ্যাক্টরি ও ফেকার এর সাহায্যে সহজেই ইউজার, পাসওয়ার্ড, ঠিকানা, মানুষের নাম এবং আরও অনেক কিছুই তৈরি করতে পারেন**



## কি ভাবে সোর্স কোড পাওয়া যাবে?

গিটহাব এর [project.one](https://github.com/robertbiswas/project.one) রিপজিটোরিটি ফোর্ক করুন এবং আপনার সিস্টেমে ক্লোন করুন।

এই অধ্যায়ের সোর্স কোড পেতে
```bash
git checkout d549749
```

সর্বশেষ কমিট পর্যন্ত পেতে আবার নিচের কমান্ডটি দিন
```bash
git checkout master
```
