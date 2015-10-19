# ইন্সটলেশনঃ 

লারাভেল ফ্রেমওয়ার্ককে ইন্সটল করতে হলে **LAMP/LEMP** এনভায়রনমেন্ট আগে থেকেই প্রস্তুত থাকতে হবে। 
অর্থাৎ **PHP, MySql, Apache/nginx** ইন্সটল করা থাকতে হবে আর এগুলা আমরা উইন্ডোস মেশিন হলে **wamp/xampp** ব্যাবহার করে করতে পারি অপরদিকে ইউনিক্স মেশিন হলে **LAMP/LEMP** স্ট্যাক সেটআপ করে করতে পারি।

আরেকটি অপরিহার্য বিষয় হল যে লারাভেল ইন্সটল আর এর ডিপেন্ডেন্সি ম্যানেজ করার জন্য **Composer** অবশ্যই ইন্সটল করা থাকতে হবে।
আপনার মেশিনে যদি **Composer** ইন্সটল না থাকে তাহলে নিচের পদ্ধতি অনুসরণ করতে পারেন।

উইন্ডোস মেশিন হলে [এই লিংক](https://getcomposer.org/Composer-Setup.exe) থেকে কম্পোজার ইন্সটলারটি নামিয়ে নিয়ে খুব সহজেই ইন্সটল করে নিতে পারেন।

আবার লিনাক্স কিংবা ইউনিক্স মেশিন হলে টার্মিনালে নিচের কমান্ডটি লিখতে হবেঃ  

```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

আমরা যেহেতু লারাভেলের নতুন ভার্সন ব্যবহার করব সেহেতু পিএইসপির নতুন ভার্সনের সাথে সাথে কিছু এক্সটেনশন থাকতে হবে নিচে সেগুলা নিচে উল্লেখ করা হলঃ 

* PHP >= 5.5.9
* OpenSSL PHP Extension
* PDO PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension

কম্পোজার সহ সব কিছু ইন্সটল আর কনফিগার করা হয়ে গেলে নিচের মত করে লারাভেল ইন্সটল করুন।

টার্মিনাল কিংবা কমান্ড প্রমোট ওপেন করুন। এবার টার্মিনাল হতে আপনার ওয়ার্কিং ডিরেক্টরিতে নেভিগেট করুন। এরপর নিচের মত করে কমান্ড লিখুনঃ 

```
composer create-project laravel/laravel your-project-name --prefer-dist
```

এখানে **your-project-name** এর জায়গায় আপনার প্রজেক্টের নাম দিতে হবে।
ধরুন আমাদের ক্ষেত্রে howtocode নাম দিলাম তাহলে আমাদেরকে প্রথম থেকে নিচের মত করে কমান্ড লিখতে হবেঃ 
```
cd /var/www/html
composer create-project laravel/laravel howtocode –prefer-dist
cd howtocode
```

**বিঃদ্রঃ** **Composer** কমান্ডটি উইন্ডোস মেশিনে রান করার সমই **github** এর এক্সেস চাইতে পারে সেই ক্ষেত্রে আপনার এক্সেসটি ব্যাবহার করবেন। 

এবার ইন্সটল করা হয়ে গেলে আমরা লারাভেলের আর্টিসান কমান্ড দিয়ে লারাভেল রান করতে পারি।
```
php artisan serve
```
আর **http://localhost:8000** লিংক দিয়ে প্রজেক্টি দেখতে পারব। 
আর্টিসান কমান্ডটি না ব্যাবহার করতে চাইলে **http://localhost/howtocode/public/** লিংক দিয়ে অ্যাক্সেস করতে পারব। 
 
অন্যদিকে আমরা লারাভেল ইন্সটলার ব্যাবহার করেও খুব সহজেই লারাভেল ইন্সটল করে নিতে পারি নিচের মত করেঃ 

### ইন্সটলারটিকে গ্লোবালী ইন্সটল করার জন্যঃ 

```
composer global require "laravel/installer=~1.1"
```

এবার কম্পোজারের **~/.composer/vendor/bin** ডিরেক্টরিকে **PATH** এ যুক্ত করতে হবে এর জন্য ইউনিক্স মেশিনের কমান্ড নিচে দেয়া হলঃ 
```
export PATH="~/.composer/vendor/bin:$PATH"
```

### এবার ইন্সটলার দিয়ে লারাভেল প্রজেক্ট তৈরি করার জন্যঃ

```
laravel new howtocode
```

পরিশেষে **Pretty URL** এর জন্য নিচের কনফিগার ব্যাবহার করবেন।

### Apache/htaccess এর জন্যঃ 

```
Options +FollowSymLinks
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

### nginx এর জন্যঃ 

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

পরবর্তী চ্যাপ্টারে **বেসিক রাউটিং** নিয়ে আলোচনা করা হবে। 
