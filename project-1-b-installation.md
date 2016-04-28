#লারাভেল ইন্সটল
***
আসুন নিচের কমান্ডটির মাধ্যমে আমাদের প্রোজেক্ট project.one ইন্সটল করি।
```bash
composer create-project laravel/laravel project.one
```
ইন্সটল হবার পর ভার্সন চেক করলাম, আপনি নিশ্চয় আরও আপডেটএড ভার্সন পাবেন।
![project-1-version-check](images/project-1-version-check.png)

ভার্চুয়াল হস্ট কনফিগার করার জন্য আমার host(OS X path: /private/etc/hosts) ফাইলে নিচের অংশ
```bash
#Project 1 - howtocode.com.bd
127.0.0.1   project.one
```

ও httpd-vhosts.conf(OS X path: /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf) ফাইলে নিচের অংশ
```bash
<VirtualHost *:80>
    DocumentRoot "/Applications/MAMP/htdocs/project.one/public"
    ServerName project.one
</VirtualHost>
```  
আপনি নিশ্চয় বুজতে পারছেন এই প্রোজেক্টটি করার সময় আমি MAMP ব্যবহার করছি।

এবার এড্রেস বার এ ```http://project.one``` দিয়ে দেখি

![project-1-initialize](images/project-1-initialize.png)

এই প্রোজেক্টতে আমরা ব্লেড টেম্পলেট দিয়ে Form তৈরি করবো তাই আসুন ```laravelcollective/html``` যোগ করে নেই আমাদের প্রোজেক্ট এ। প্রয়োজনে <a href='http://laravel.howtocode.com.bd/blade-template.html#%E0%A6%AC%E0%A7%8D%E0%A6%B2%E0%A7%87%E0%A6%A1-%E0%A6%9F%E0%A7%87%E0%A6%AE%E0%A6%AA%E0%A7%8D%E0%A6%B2%E0%A7%87%E0%A6%9F%E0%A6%BF%E0%A6%82%E0%A6%83-html-%E0%A6%93-forms' target='_blank'> ব্লেড টেমপ্লেটিংঃ HTML ও Forms </a> অংশটি আরেকবার দেখে নিন।

তাহলে আজ আমাদের প্রোজেক্ট শুরু হল।
