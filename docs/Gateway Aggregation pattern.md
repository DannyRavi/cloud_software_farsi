
از یک gateway برای تجمیع چندین requests منحصر به فرد در یک request استفاده کنید. این الگو زمانی مفید است که یک کلاینت برای انجام یک عملیات باید چندین تماس با سیستم‌های backend مختلف برقرار کند.

**طرح صورت مسئله:**

برای انجام یک task واحد، یک کلاینت ممکن است مجبور باشد چندین فراخونی با سرویس های مختلف backend برقرار کند. برنامه‌ای که برای انجام یک task به serviceهای زیادی متکی است، باید resourceهای را برای هر request مصرف کند. هنگامی که هر ویژگی یا سرویس جدیدی به برنامه اضافه می شود، request های اضافی مورد نیاز است که نیاز به منابع و فراخوانی‌های شبکه را بیشتر می کند. این ارتباط بین یک client و یک backend می تواند بر performance و scale برنامه تأثیر منفی بگذارد. معماری‌های میکروسرویس این مشکل را رایج‌تر کرده‌اند، زیرا برنامه‌های کاربردی ساخته‌شده پیرامون بسیاری از سرویس‌های کوچک‌تر، تعداد فراخوانی متقابل سرویس بیشتری دارند.

در نمودار زیر client برای هر سرویس request ارسال می کند (1،2،3). هر سرویس request را پردازش می کند و پاسخ را به برنامه برمی‌گرداند (4،5،6). در یک شبکه سلولی با latency معمولاً بالا، استفاده از requestهای مستقل به این شیوه ناکارآمد است و می‌تواند منجر به قطع اتصال یا requestهایهای ناقص شود. در حالی که هر requestهای ممکن است به صورت موازی انجام شود، برنامه باید داده ها را برای هر request ارسال و سپس صبر کند و در نهایت پردازش کند و چون همه‌ی در اتصالات و ارتباطات جداگانه و مجزا است احتمال خطا و failure را افزایش می دهد.

![[Pasted image 20240209180420.png]]

**راه حل:**

از یک gateway برای کاهش chattiness و ارتباط بین client و serviceها استفاده کنید. gateway همیشه requestهای client را دریافت می‌کند، requestها را به سیستم‌های backend مختلف ارسال(dispatche) می‌کند و سپس نتایج را جمع‌آوری می‌کند و آنها را برای client درخواست‌کننده ارسال می‌کند.

این الگو می‌تواند تعداد requestهایی را که برنامه برای backend service ارائه می‌کند کاهش دهد و عملکرد برنامه را در شبکه‌های با high-latency بهبود بخشد.

در نمودار زیر اپلیکیشن درخواستی را به gateway (1) ارسال می کند. request شامل بسته ای از request های اضافی است. gateway اینها را تجزیه و آنالیز می کند و هر درخواست را با ارسال آن به سرویس مربوطه پردازش می کند (2). هر سرویس یک پاسخ به gateway (3) برمی گرداند. gateway پاسخ های هر سرویس را ترکیب می کند و پاسخ را به برنامه می فرستد (4). برنامه تنها یک request می دهد و تنها یک پاسخ از gateway دریافت می کند.

![[Pasted image 20240209180433.png]]

مسائل و ملاحظات:

- به هیچ وجه gateway نباید با سرویس backend به صورت service coupling across ارتباط داشته باشد .
- همیشه gateway باید در نزدیکی backend services قرار گیرد تا latency تا حد امکان کاهش یابد.
- سرویس gateway ممکن است به یک نقطه bottleneck تبدیل شود. اطمینان حاصل کنید که gateway به درستی طراحی شده است تا نیازهای در دسترس بودن برنامه شما را برآورده کند.
- ممکن است gateway یک bottleneck ایجاد کند. اطمینان حاصل کنید که دروازه عملکرد مناسبی برای تحمل load دارد و می تواند برای برآورده کردن رشد و توسعه پیش بینی شده شما جهت توسعه نرم افزار scaled شود.
- تست scaled را در برابر gateway انجام دهید تا مطمئن شوید که خرابی های متوالی و دنباله دار برای سرویس ها ایجاد نمی کنید.
- با استفاده از تکنیک هایی مانند [bulkheads](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fbulkhead&si=fo7vnuqdocyx&st=post&k=hY2Sw4HYapv4vKI%2BEsBGoDsSHP9whnMH0XIT79e9OO8%3D), [circuit breaking](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fcircuit-breaker&si=fo7vnuqdocyx&st=post&k=illBQcz%2B0lwlr2cki6CvgScDVOOaNe4df3y7okB3uWI%3D), [retry](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fretry&si=fo7vnuqdocyx&st=post&k=tkuGIe0Yb61oxrEKa%2FCUib9SHMLqdFsikEy6rRFHRJE%3D) و timeouts، نرم افزاری با انعطاف پذیری را اجرا کنید.
- اگر یک یا چند فراخوانی از سرویس بیش از حد طول بکشد، ممکن است زمان سپری شده و بازگرداندن مجموعه ای تا حدی قابل قبول باشد. در نظر بگیرید که برنامه شما چگونه با این سناریو برخورد خواهد کرد.
- از ورودی/خروجی ناهمزمان (asynchronous I/O) استفاده کنید تا اطمینان حاصل کنید که تاخیر در backend باعث مشکلات عملکرد در برنامه نمی شود.
- پیاده سازی tracing توزیع شده با استفاده از شناسه های(IDs) منحصر به فرد برای track کردن هر فراخوانی جداگانه.
- متریک‌های request و اندازه response را Monitor کنید.
- بازگرداندن داده های کش(cached) را به عنوان یک استراتژی شناسایی شکست برای مدیریت failure شدن درخواست‌های ارتباطی در نظر بگیرید.
- به جای ایجاد تجمیع(aggregation) در gateway، قرار دادن یک سرویس تجمیع(aggregation) در پشت gateway را در نظر بگیرید. Request aggregation احتمالاً نیازمند resourceهای متفاوتی نسبت به سایر serviceهای در gateway خواهد بود و ممکن است بر عملکرد مسیریابی و بارگذاری دروازه تأثیر بگذارد.

**چه زمانی از این الگو استفاده کنیم؟**

از این الگو زمانی استفاده کنید که:

- یک کلاینت برای انجام یک عملیات باید با چندین سرویس backend ارتباط برقرار کند.
- کلاینت ممکن است از شبکه هایی با latency قابل توجه مانند شبکه های سلولی استفاده کند.

**این الگو ممکن است زمانی مناسب نباشد که:**

- زمانی که می خواهید تعداد فراخوانی های بین یک کلاینت و یک سرویس را در چندین عملیات کاهش دهید. در آن سناریو، ممکن است بهتر باشد یک عملیات دسته ای/گروهی به سرویس اضافه شود.
- کلاینت یا برنامه در نزدیکی سرویس backend قرار دارد و تأخیر عامل مهمی نیست.

### مثال:

مثال زیر نحوه ایجاد یک سرویس NGINX تجمیع(aggregation) در gateway ساده با استفاده از زبان برنامه نویسی Lua را نشان می دهد.

```csharp
worker_processes  4;

events {
  worker_connections 1024;
}

http {
  server {
    listen 80;

    location = /batch {
      content_by_lua '
        ngx.req.read_body()

        -- read json body content
        local cjson = require "cjson"
        local batch = cjson.decode(ngx.req.get_body_data())["batch"]

        -- create capture_multi table
        local requests = {}
        for i, item in ipairs(batch) do
          table.insert(requests, {item.relative_url, { method = ngx.HTTP_GET}})
        end

        -- execute batch requests in parallel
        local results = {}
        local resps = { ngx.location.capture_multi(requests) }
        for i, res in ipairs(resps) do
          table.insert(results, {status = res.status, body = cjson.decode(res.body), header = res.header})
        end

        ngx.say(cjson.encode({results = results}))
      ';
    }

    location = /service1 {
      default_type application/json;
      echo '{"attr1":"val1"}';
    }

    location = /service2 {
      default_type application/json;
      echo '{"attr2":"val2"}';
    }
  }
}
```

### منابع مرتبط:

[Backends for Frontends pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fbackends-for-frontends&si=fo7vnuqdocyx&st=post&k=amRHLKRSlUazyozh4Fvrw9PLiuT21czNeppL%2FipLmpA%3D)  
[Gateway Offloading pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-offloading&si=fo7vnuqdocyx&st=post&k=fN2kT7E4v46olEvjRoXrfez7aOkLpqTmpAcl4vwblkA%3D)  
[Gateway Routing pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-routing&si=fo7vnuqdocyx&st=post&k=6UauUTJ8abyJPTv3CkproOxIAmc%2FYyqxgouuHwqkubc%3D)