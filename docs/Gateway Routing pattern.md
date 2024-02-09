در این الگو باید requestها را با استفاده از یک endpoint به چندین سرویس یا چندین instance از سرویس هدایت کنید. این الگو زمانی مفید است که می خواهید:

- چندین سرویس را در یک endpoint واحد نشان دهید و بر اساس request به سمت سرویس مورد نظر برویم.
- چندین instance از یک سرویس را در یک endpoint برای load balancing یا availability در قرار دهیم.
- نسخه‌های مختلف یک سرویس را در یک endpoint و ترافیک مسیر در نسخه‌های مختلف نشان دهیم.

**طرح صورت مسئله:**

هنگامی که یک client نیاز به استفاده چندین سرویس دارد(چندین instance از یک سرویس یا ترکیبی از سرویس‌های مختلف) در این حالت client باید از وضعیت ارتباطی سرویس ها آگاه باشد. به عنوان مثال سناریوهای زیر را در نظر بگیرید.

- یک - **Multiple disparate services (** چندین سرویس متفاوت**) :** یک برنامه تجارت الکترونیک (e-commerce application) ممکن است خدماتی مانند جستجو، بررسی، سبد خرید، تسویه حساب و تاریخچه سفارش ارائه دهد. هر سرویس دارای یک API متفاوت است که کلاینت باید با آن تعامل داشته باشد و کلاینت باید در مورد نحوه ارتباطی هر endpoint برای اتصال به سرویس ها بداند. اگر یک API تغییر کند، کلاینت نیز باید update شود. اگر یک سرویس را به دو یا چند سرویس جداگانه تغییر دهید، کد باید هم در سرویس و هم در کلاینت تغییر کند.
- دو - **Multiple instances of the same service** (چندین نمونه از یک سرویس): سیستم می تواند نیاز به اجرای چندین instance از یک سرویس در جایی مشابه یا متفاوت داشته باشد. اجرای چندین instance را می توان برای اهداف load balancing یا برای برآوردن نیازهای availability انجام داد. هر بار که یک instance برای مطابقت با تعداد درخواست‌های بالا یا پایین روی application تغییر می‌کند در این حالت client باید update شود.
- سه - **Multiple versions of the same service** (چندین ورژن از یک سرویس) : به عنوان بخشی از استراتژی deployment، ورژن های جدید یک سرویس را می توان در کنار ورژن های موجود deploy کرد که این به عنوان استقرار **سبز آبی** شناخته می شود. در این سناریوها، client باید هر بار که تغییراتی در درصد ترافیک هدایت شده به نسخه جدید و endpoint موجود ایجاد می‌شود، به‌روزرسانی شود.

**راه حل:**

یک gateway در جلوی مجموعه ای از برنامه ها، سرویس ها، deploymentها قرار دهید. از مسیریابی لایه ۷ اپلیکیشن برای مسیریابی request به instance های مناسب استفاده کنید.

با استفاده از این الگو، برنامه client فقط باید در مورد یک endpoint واحد بداند و با یک endpoint واحد ارتباط برقرار کند. موارد زیر نشان می‌دهد که چگونه الگوی gateway routing به سه سناریویی که در بخش **طرح صورت مسئله** شده‌اند، می‌پردازد.

### روش Multiple disparate services

![[Pasted image 20240209175721.png]]

الگوی gateway routing در این سناریو که یک client از چندین سرویس را استفاده می کند مناسب است. اگر یک سرویس یکپارچه، تجزیه یا جایگزین شود، client لزوماً نیازی به updating ندارد و می‌تواند به requestهایی روی gateway ادامه دهد و فقط مسیریابی تغییر می‌کند.

یک gateway همچنین این امکان را می دهد که backend service را از کلاینت ها abstract کنید و به شما این امکان را می دهد که فراخوانی‌های client را ساده نگه دارید و در عین حال تغییرات را در سرویس های backend در پشت gateway کنترل کنید. فراخوانی‌های client را می‌توان به هر سرویس یا سرویسی که برای مدیریت رفتار مورد انتظار client نیاز دارد هدایت کرد و این امکان را می‌دهد که سرویس را در پشت gateway بدون تغییر در سمت client به راحتی؛ اضافه، تقسیم و سازماندهی مجدد کنید.

![[Pasted image 20240209175731.png]]

کلید واژه Elasticity کلمه بسیار مهمی در رایانش ابری است. سرویس‌ها را می توان برای پاسخگویی به تقاضای فزاینده زیاد (spun up) کرد و یا زمانی که تقاضا کم است برای صرفه جویی در هزینه ها کاهش (spun down) داد. پیچیدگی ثبت و لغو ثبت service instance در gateway خلاصه شده است. client از افزایش یا کاهش تعداد serviceها بی اطلاع است.

همیشه Service instances ها می توانند در یک جا یا چند جا روی سرورهای مختلف مستقر شوند. الگوی [Geode pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgeodes&si=swkawy5gliac&st=post&k=hcd%2BXwy60IhXmo3GHTq6ofXemvnHKflnww2ocFiKD%2FY%3D) توضیح می‌دهد که چگونه deployment چند ناحیه/node/ سرور و ... می‌تواند latency را بهبود بخشد و availability بودن یک سرویس را افزایش دهد.

### روش Multiple versions of the same service

![[Pasted image 20240209175742.png]]

این الگو را می توان برای deployment استفاده کرد و این امکان را می دهد که نحوه به روز رسانی به کاربران را مدیریت کنید. هنگامی که یک نسخه جدید از سرویس شما مستقر می شود، می توان آن را به موازات نسخه موجود مستقر کرد. مسیریابی به شما امکان می‌دهد تا کنترل کنید چه نسخه‌ای از سرویس به مشتریان ارائه می‌شود، و به شما این امکان را می‌دهد که از استراتژی‌های انتشار مختلف استفاده کنید، چه به‌روزرسانی‌های incremental، parallel یا complete rollouts. هر مشکلی که پس از استقرار سرویس جدید مشاهده شود، می‌تواند به سرعت با ایجاد یک تغییر پیکربندی در gateway، بدون تأثیر بر کلاینت‌ها، بازگردانده شود.

### مسائل و ملاحظات:

- سرویس gateway می تواند یک نقطه failure را ایجاد کند. مطمئن شوید که به درستی طراحی شده است تا نیازهایavailability بودن برنامه را برآورده کند. قابلیت انعطاف پذیری(resiliency) و تحمل خطا را در اجرا در نظر بگیرید.
- سرویس gateway می تواند یک گلوگاه(bottleneck) ایجاد کند. اطمینان حاصل کنید که دروازه عملکرد مناسبی برای handle کردن load دارد و به راحتی می تواند مطابق با انتظارات scale مقیاس شود.
- تست load را در برابر دروازه انجام دهید تا مطمئن شوید که خرابی های زنجیره‌ای و دنباله دار برای سرویس ها ایجاد نمی کند.
- مسیریابی Gateway در شبکه در لایه سطح 7 است که می تواند بر اساس IP، port، header یا URL باشد.
- به طور کلی Gateway service می تواند global یا regional ای باشد. Azure Front Door یک global gateway است، در حالی که Azure Application Gateway به صورت regional است. اگر راه حل شما به deployment service به صورت multi-region نیاز دارد، از یک global gateway استفاده کنید. اگر workload به شکل regional دارید که به کنترل دقیق نحوه balance ترافیک نیاز دارد از Application Gateway استفاده کنید. به عنوان مثال؛ وقتی می خواهید ترافیک بین ماشین های مجازی را balance کنید.
- سرویس gateway یک public endpoint برای سرویس‌هایی است که در جلو آن قرار دارد. محدود کردن دسترسی public network به backend services را در طراحی در نظر بگیرید آن هم فقط به صورت ایجاد دسترسی به سرویس‌ها فقط از طریق gateway یا از طریق یک private virtual network.

### **چه زمانی از این الگو استفاده کنیم؟**

- یک client نیاز به استفاده از چندین سرویس دارد که در پشت یک gateway قابل دسترسی است.
- وقتی که می خواهیم برنامه های client را با استفاده از یک endpoint ساده کنید.
- وقتی که requestها را از externally addressable endpoints به internal virtual endpoints هدایت یا مسیردهی کنید، مانند قرار دادن پورت‌های یک VM برای cluster کردن آدرس‌های IP مجازی.
- یک client باید سرویس‌هایی را که در چندین ناحیه مختلف (فیزیکی/مجازی) اجرا می شود برای رسیدن به latency یا availability مناسب، استفاده کند.
- یک client باید تعداد مختلفی از service instanceها را مورد استفاده قرار دهد.
- وقتی که می خواهیم یک deployment strategy را پیاده سازی کنیم که در آن clientها به چندین ورژن از سرویس به طور همزمان دسترسی داشته باشند.

### مثال:

با استفاده از Nginx به عنوان router؛ مثال زیر یک فایل پیکربندی ساده برای سروری است که درخواست‌های برنامه‌های موجود در virtual directoriesهای مختلف را به ماشین‌های مختلف در backend هدایت می‌کند.

```lua
server {
    listen 80;
    server_name domain.com;

    location /app1 {
        proxy_pass http://10.0.3.10:80;
    }

    location /app2 {
        proxy_pass http://10.0.3.20:80;
    }

    location /app3 {
        proxy_pass http://10.0.3.30:80;
    }
}
```


  
برای پیاده سازی الگوی gateway routing می توان از خدمات Azure زیر استفاده کرد:

- An [Application Gateway instance](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Fapplication-gateway%2Ftutorial-multiple-sites-cli&si=swkawy5gliac&st=post&k=Vzm9XEWxOnDimo8ukl8blu0IDDlOaFsOB%2BDSzQ6Zk4Q%3D), which provides regional layer-7 routing.
- An [Azure Front Door instance](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Ffrontdoor&si=swkawy5gliac&st=post&k=e7X3Exq7fwImncPG3LFOoJPF1cms7ivLO0aO%2BPsPdT4%3D), which provides global layer-7 routing.

### منابع مرتبط:

- [Backends for Frontends pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fbackends-for-frontends&si=swkawy5gliac&st=post&k=V40%2BhwmGPD6ZRj5Z90zLkO3NpysBQ3inr4zaZWiLLGk%3D)
- [Gateway Aggregation pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-aggregation&si=swkawy5gliac&st=post&k=uAkHZ49WlCJNRoAnAOwR4Rl%2FlVQV6jeI0dDEp0Z0WlY%3D)
- [Gateway Offloading pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-offloading&si=swkawy5gliac&st=post&k=AJSN1sB5jdN7ChUKlKTaahqsNV3WZIUjSFMTXAPhg3c%3D)