عملکرد specialized service functionality را به صورت یک پروکسی gateway بارگیری کنید. این الگو می‌تواند توسعه برنامه را با انتقال shared service functionality، مانند استفاده از گواهی‌های SSL، از سایر بخش‌های برنامه به درون gateway ساده کند.

**طرح صورت مسئله:**

برخی از ویژگی‌ها معمولاً در چندین سرویس استفاده می‌شوند و این ویژگی‌ها نیاز به پیکربندی، مدیریت و نگهداری دارند. یک سرویس توزیع شده(distributed) به صورت shared یا specialized با هر deploy شدن مجدد برنامه administrative overhead و خطای deployment ا را افزایش می دهد .. هرگونه به‌روزرسانی برای یک ویژگی shared باید در همه سرویس‌هایی که آن ویژگی را به اشتراک می‌گذارند، اجرا شود.

مدیریت صحیح مسائل امنیتی (تأیید اعتبار رمز، رمزگذاری، مدیریت گواهینامه SSL) و سایر task پیچیده می تواند به اعضای تیمی نیازمند شود که مهارت های بسیار تخصصی داشته باشند. به عنوان مثال، گواهی مورد نیاز یک برنامه کاربردی باید پیکربندی و در تمام نمونه های برنامه مستقر شود. با هر استقرار جدید، گواهی باید مدیریت شود تا از منقضی نشدن آن اطمینان حاصل شود. هر گواهی متداول که به زودی منقضی می شود باید در هر استقرار برنامه به روز، آزمایش و تأیید شود.

سایر سرویس‌های رایج مانند authentication, authorization, logging, monitoring یا [throttling](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fthrottling&si=cruveu9lm1v5&st=post&k=VE2hRMMe8z7PVWkOJg43rIeCEUpZiZxMVUtiEVE3OSw%3D) ممکن است برای پیاده‌سازی و مدیریت در تعداد زیادی از deploymentها دشوار باشد. شاید بهتر باشد که این نوع عملکرد را یکپارچه کنید تا سربار و احتمال خطا کاهش یابد.

**راه حل:**

برخی از ویژگی ها را در یک gateway بارگذاری کنید، به ویژه نگرانی هایی مانند مدیریت گواهی، احراز هویت، خاتمه SSL، نظارت، ترجمه پروتکل یا [throttling](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fthrottling&si=cruveu9lm1v5&st=post&k=VE2hRMMe8z7PVWkOJg43rIeCEUpZiZxMVUtiEVE3OSw%3D).

نمودار زیر gateway ای را نشان می دهد که اتصالات SSL ورودی را خاتمه می دهد. داده‌ها را از طرف درخواست‌کننده اصلی از هر سرور HTTP بالادست gateway درخواست می‌کند.

![[Pasted image 20240209180210.png]]

مزایای این الگو عبارتند از:

- توسعه serviceها را با حذف نیاز به توزیع و نگهداری منابع پشتیبانی، مانند گواهی‌های وب سرور و پیکربندی برای وب‌سایت‌های امن، ساده کنید. پیکربندی ساده‌تر منجر به مدیریت و مقیاس‌پذیری آسان‌تر می‌شود و ارتقای serviceها را ساده‌تر می‌کند.
- به تیم‌های تخصصی اجازه دهید تا ویژگی‌هایی را اجرا کنند که به تخصصی خاصی مانند امنیت نیاز دارند. این به تیم اصلی شما اجازه می دهد تا روی عملکرد برنامه تمرکز کند و این نگرانی های تخصصی اما مقطعی را به کارشناسان مربوطه واگذار کند.
- برای ثبت request و logging response و monitoring مقداری ثبات و صبر داشته باشید. حتی اگر یک سرویس به درستی تنظیم نشده باشد، gateway را می توان برای اطمینان از حداقلی سطح monitoring و logging پیکربندی کرد.

### مسائل و ملاحظات:

- اطمینان حاصل کنید که gateway بسیار در دسترس است و در برابر failure و خطا مقاوم است. با اجرای چندین نمونه از gateway خود از نقاط failure اجتناب کنید.
- اطمینان حاصل کنید که gateway برای capacity و scaling مورد نیاز برنامه و endpoints شما به درستی طراحی شده است. اطمینان حاصل کنید که gateway به یک bottleneck برای برنامه تبدیل نمی شود و به اندازه کافی مقیاس پذیر (scalable) است.
- فقط ویژگی هایی را بارگیری کنید که توسط کل برنامه استفاده می شود، مانند امنیت یا انتقال داده.
- مورد Business logic هرگز نباید در gateway بارگذاری شود.
- اگر نیاز به track کردن تراکنش‌ها دارید، شناسه‌های (IDs) منحصر به فردی را برای اهداف ورود به سیستم ایجاد کنید.

### **چه زمانی از این الگو استفاده کنیم؟**

**از این الگو زمانی استفاده کنید که:**

- استقرار (deployment) برنامه یک نگرانی مشترک مانند گواهینامه های SSL یا رمزگذاری دارد.
- یک ویژگی که در بین deployment برنامه‌هایی که ممکن است نیازمندی منابع متفاوتی داشته باشند، مانند منابع حافظه، ظرفیت ذخیره‌سازی یا اتصالات شبکه، رایج است.
- زمانی که می خواهیم مسئولیت مسائلی مانند امنیت شبکه، throttling، یا سایر نگرانی های مربوط به مرز شبکه را به یک تیم تخصصی تر منتقل کنید.

این الگو در حالت coupling across services م مناسب نیست.

### مثال:

با استفاده از Nginx به عنوان ابزار offload SSL، پیکربندی زیر یک اتصال SSL ورودی را خاتمه می دهد و اتصال را به یکی از سه سرور HTTP بالادستی توزیع می کند.

```csharp
upstream iis {
        server  10.3.0.10    max_fails=3    fail_timeout=15s;
        server  10.3.0.20    max_fails=3    fail_timeout=15s;
        server  10.3.0.30    max_fails=3    fail_timeout=15s;
}

server {
        listen 443;
        ssl on;
        ssl_certificate /etc/nginx/ssl/domain.cer;
        ssl_certificate_key /etc/nginx/ssl/domain.key;

        location / {
                set $targ iis;
                proxy_pass http://$targ;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto https;
proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
        }
}
``` 

در Azure، با تنظیم [setting up SSL termination on Application Gateway](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Fapplication-gateway%2Ftutorial-ssl-cli&si=cruveu9lm1v5&st=post&k=0xWbM%2FZNRp7dpLCjZYOOj62KTDpM4MyW%2BZWzBh%2FsiTE%3D) می توان به این امر دست یافت.


**منابع مرتبط:**

[Backends for Frontends pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fbackends-for-frontends&si=cruveu9lm1v5&st=post&k=5jti2l5NNEpE3uQw8z5du36riXGhtoYGGpU0XDrQ%2BUs%3D)  
[Gateway Aggregation pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-aggregation&si=cruveu9lm1v5&st=post&k=IbF7GY03AEaXdfHjVP%2B2Qji6oa7Gj3%2F4Fu9gWBOU5vA%3D)  
[Gateway Routing pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fgateway-routing&si=cruveu9lm1v5&st=post&k=kiusKp6ywETq0vu1dq0SSSOO7zCiMZ9Ge1jaI1AmPGQ%3D)