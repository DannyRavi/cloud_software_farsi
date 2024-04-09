
از یک gateway برای تجمیع چندین request منحصر به فرد در یک request استفاده کنید. این الگو زمانی مفید است که یک کلاینت برای انجام یک عملیات باید چندین تماس با سیستم‌های backend مختلف برقرار کند.

## **طرح صورت مسئله:**

برای انجام یک تسک واحد، یک کلاینت ممکن است مجبور باشد چندین فراخونی با سرویسهای مختلف backend برقرار کند. برنامه‌ای که برای انجام یک تسک به سرویس‌های زیادی متکی است، پس باید منابع را برای هر request مصرف کند. هنگامی که هر ویژگی یا سرویس جدیدی به برنامه اضافه می‌شود، درخواست‌های اضافی مورد نیاز است که نیاز به مصرف بیشتر منابع و فراخوانی‌های شبکه را بیشتر می‌کند. این ارتباط بین یک client و یک backend می‌تواند بر کارایی و مقیاس‌پذیری  برنامه تأثیر منفی بگذارد. معماری‌های میکروسرویس این مشکل را رایج‌تر کرده‌اند، زیرا اپلیکیشن‌های ساخته‌شده پیرامون بسیاری از سرویس‌های کوچک‌تر، تعداد فراخوانی متقابل سرویس بیشتری دارند.

در نمودار زیر client برای هر سرویس request ارسال می‌کند (1،2،3). هر سرویس request را پردازش می‌کند و پاسخ را به برنامه برمی‌گرداند (4،5،6). در یک شبکه سلولی با تاخیر زمانی معمولاً بالا، استفاده از requestهای مستقل به این شیوه ناکارآمد است و می‌تواند منجر به قطع اتصال یا requestهایی ناقص شود. در حالی که هر requestهای ممکن است به صورت موازی انجام شود، برنامه باید داده‌ها را برای هر request ارسال و سپس صبر کند و در نهایت پردازش کند و چون همه‌ی در اتصالات و ارتباطات جداگانه و مجزا است احتمال خطا و شکست را افزایش می‌دهد.


![gateway-aggregation-problem](../assets/design_implementation/gateway-aggregation-problem.png)


## **راه حل:**

از یک gateway برای کاهش تماس‌ها و ارتباط‌ها بین client و serviceها استفاده کنید. gateway همیشه requestهای client را دریافت می‌کند، requestها را به سیستم‌های backend مختلف ارسال(dispatche) می‌کند و سپس نتایج را جمع‌آوری می‌کند و آنها را برای client درخواست‌کننده ارسال می‌کند.

این الگو می‌تواند تعداد requestهایی را که برنامه برای سرویس backend  ارائه می‌کند کاهش دهد و عملکرد برنامه را در شبکه‌های با high-latency بهبود بخشد.

در نمودار زیر اپلیکیشن درخواستی را به gateway (1) ارسال می‌کند. request شامل بسته ای از requestهای اضافی است. gateway اینها را تجزیه و آنالیز می‌کند و هر درخواست را با ارسال آن به سرویس مربوطه پردازش می‌کند (2). هر سرویس یک پاسخ به gateway (3) برمی گرداند. gateway پاسخهای هر سرویس را ترکیب می‌کند و پاسخ را به برنامه می‌فرستد (4). برنامه تنها یک request می‌دهد و تنها یک پاسخ از gateway دریافت می‌کند.

![gateway-aggregation](../assets/design_implementation/gateway-aggregation.png)

## مسائل و ملاحظات:

- به هیچ وجه gateway نباید با سرویس backend به عنوان سرویس متصل شده میانی ارتباط داشته باشد.
- همیشه gateway باید در نزدیکی سرویس‌های backend  قرار گیرد تا تاخیر زمانی تا حد امکان کاهش یابد.
- سرویس gateway ممکن است به یک نقطه شکست و خرابی تبدیل شود. اطمینان حاصل کنید که gateway به درستی طراحی شده است تا نیازهای در دسترس بودن برنامه شما را برآورده کند.
- ممکن است gateway یک گلوگاه ایجاد کند. اطمینان حاصل کنید که دروازه عملکرد مناسبی برای تحمل load دارد و می‌تواند برای برآورده کردن رشد و توسعه پیش بینی شده شما جهت توسعه نرم افزار مقیاس‌دهی(scale) شود.
- تست مقیاس‌دهی را در برابر gateway انجام دهید تا مطمئن شوید که خرابیهای متوالی و دنباله دار برای سرویسها ایجاد نمی‌کنید.
- با استفاده از تکنیک هایی مانند  [bulkheads](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead), [circuit breaking](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker), [retry](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry)و timeoutها، نرم افزاری با انعطاف‌پذیری بالا را اجرا کنید.
- اگر یک یا چند فراخوانی از سرویس بیش از حد طول بکشد، ممکن است زمان سپری شده و بازگرداندن مجموعه ای تا حدی قابل قبول باشد. در نظر بگیرید که برنامه شما چگونه با این سناریو برخورد خواهد کرد.
- از ورودی/خروجی ناهمزمان (asynchronous I/O) استفاده کنید تا اطمینان حاصل کنید که تاخیر در backend باعث مشکلات عملکرد در برنامه نمی‌شود.
- پیاده سازی یک ردیاب (tracing) توزیع شده با استفاده از شناسه‌های(IDs) منحصر به فرد برای ردیابی کردن هر فراخوانی جداگانه.
- متریک‌های request و اندازه response را نظارت و مانیتور کنید.
- بازگرداندن داده‌‌های cache  را به عنوان یک استراتژی شناسایی شکست برای مدیریت شکست‌ و خطاهای  درخواست‌های ارتباطی در نظر بگیرید.
- به جای ایجاد تجمیع (aggregation) در gateway، قرار دادن یک سرویس مجتمع در پشت gateway را در نظر بگیرید. Requestهای تجمیع شده  احتمالاً نیازمند منابع متفاوتی نسبت به سایر سرویس‌ها در gateway خواهد بود و ممکن است بر عملکرد مسیریابی و بارگذاری روی gateway تأثیر بگذارد.

## **چه زمانی از این الگو استفاده کنیم؟**

از این الگو زمانی استفاده کنید که:

- یک کلاینت برای انجام یک عملیات باید با چندین سرویس backend ارتباط برقرار کند.
- کلاینت ممکن است از شبکه‌هایی با تاخیر زمانی قابل توجه مانند شبکههای سلولی استفاده کند.

**این الگو ممکن است زمانی مناسب نباشد که:**

- زمانی که می‌خواهید تعداد فراخوانی‌های بین یک کلاینت و یک سرویس را در چندین عملیات کاهش دهید. در آن سناریو، ممکن است بهتر باشد یک عملیات دسته ای/گروهی به سرویس اضافه شود.
- کلاینت یا برنامه در نزدیکی سرویس backend قرار دارد و تأخیر عامل مهمی نیست.

### مثال:

مثال زیر نحوه ایجاد یک سرویس NGINX تجمیع شده در یک gateway ساده با استفاده از زبان برنامه‌نویسی Lua را نشان می‌دهد.

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

- [Backends for Frontends pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends)
- [Gateway Offloading pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading)
- [Gateway Routing pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing)