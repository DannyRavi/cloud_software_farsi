اجزای یک برنامه کاربردی را در یک process یا container جداگانه برای ایجاد جداسازی و کپسوله سازی قرار دهید. این الگو همچنین می تواند برنامه ها را قادر سازد که از اجزا و فناوری های ناهمگن(heterogeneous) تشکیل شده باشند.

این الگوی Sidecar نامیده می شود زیرا شبیه یک کابین کناری متصل به موتور سیکلت است. در این الگو، Sidecar به یک برنامه والد متصل شده است و ویژگی های پشتیبانی را برای برنامه ارائه می دهد. سایدکار همچنین چرخه عمر یکسانی با برنامه والد دارد و در کنار والدین ایجاد و بازنشسته(retired) می شود. الگوی Sidecar گاهی اوقات به عنوان الگوی sidekick نامیده می شود و یک الگوی تجزیه(decomposition) است.

**طرح صورت مسئله:**

برنامه ها و سرویس ها اغلب به عملکردهای مرتبط مانند monitoring, logging, configuration و networking service نیاز دارند. این وظایف جانبی را می توان به عنوان اجزا یا سرویس های جداگانه پیاده سازی کرد.

اگر عملکردهای گفته شده به شدت با برنامه ادغام شوند، می توانند در همان فرآیند برنامه اجرا شوند و از منابع مشترک به صورت بهینه استفاده کنند. با این حال، این بدان معناست که آنها به خوبی ایزوله نیستند و قطع شدن یکی از این اجزا می تواند سایر اجزا یا کل برنامه را تحت تأثیر قرار دهد. همچنین، معمولاً باید با استفاده از همان زبان برنامه والد پیاده سازی شوند. در نتیجه، مؤلفه و برنامه دارای وابستگی متقابل نزدیک به یکدیگر هستند.

اگر برنامه به سرویس‌هایی تجزیه شود، هر سرویس می تواند با استفاده از زبان ها و فناوری های مختلف ساخته یا نوشته شود. این حالت انعطاف‌پذیری(flexibility) بیشتری می‌دهد و به این معنی است که هر component معمولا dependencyهای خاص خود را دارد و برای دسترسی به پلتفرم زیربنایی، هر resources shared که با برنامه والد به اشتراک گذاشته شده است، به language-specific libraries نیاز دارد. علاوه بر این، استقرار این ویژگی‌ها به عنوان سرویس‌های جداگانه می‌تواند latency را به برنامه اضافه کند. مدیریت کد و dependencyها برای این language-specific interface نیز می‌تواند پیچیدگی قابل‌توجهی را به خصوص برای hosting، deployment و management اضافه کند.

**راه حل:**

مجموعه منسجمی از taskها را با برنامه اصلی قرار دهید و آنها را در داخل process یا container خود قرار دهید و یک رابط همگن(homogeneous interface) برای platform services فراهم کنید.

![[Pasted image 20240209174916.png]]

یک سرویس sidecar لزوماً بخشی از برنامه نیست، بلکه به آن متصل است. هر جا که برنامه والد می رود به دنبال آن می رود. sidecarها processها یا serviceهایی را پشتیبانی می کنند که با برنامه اصلی deploy می شوند. در موتور سیکلت، sidecar به یک موتور سیکلت متصل می شود و هر موتورسیکلت می تواند sidecar مخصوص به خود را داشته باشد. به همین ترتیب، یک سرویس sidecar در سرنوشت برنامه اصلی خود سهیم است. برای هر نمونه از برنامه، یک نمونه از sidecar مستقر شده و در کنار آن میزبانی می شود.

**مزایای استفاده از الگوی سایدکار عبارتند از:**

- یک sidecar از نظر runtime environment و زبان برنامه نویسی از primary application مستقل است، بنابراین نیازی به توسعه یک sidecar برای هر زبان ندارید.
- سایدکار می‌تواند به resources مشابه برنامه اصلی دسترسی داشته باشد. برای مثال، یک سایدکار می‌تواند منابع سیستمی را که هم توسط سایدکار و هم برنامه اصلی استفاده می‌شود را monitor کند.
- به دلیل نزدیکی آن به برنامه اولیه، هیچ تأخیر قابل توجهی در برقراری ارتباط بین آنها وجود ندارد.
- حتی برای برنامه‌هایی که مکانیسم توسعه‌پذیری(extensibility) ارائه نمی‌دهند، می‌توانید از یک sidecar برای گسترش عملکرد با پیوست کردن آن به عنوان process خود در همان host یا sub-container برنامه اصلی استفاده کنید.

الگوی سایدکار اغلب با کانتینرها استفاده می شود و به آن کانتینر سایدکار یا کانتینر کناری می گویند.

**مسائل و ملاحظات:**

- نوع و نحوه deployment و packaging را که برای deploy services و processها یا کانتینرها استفاده خواهید کرد را در نظر بگیرید. کانتینرها به ویژه با sidecar pattern متناسب هستند.
- هنگام طراحی یک سرویس سایدکار، در مورد مکانیسم ارتباط بین فرآیندی با دقت تصمیم بگیرید. سعی کنید از فناوری‌های مبتنی بر زبان یا framework استفاده کنید، مگر اینکه الزامات عملکردی خاصی آن را غیرعملی کند.
- قبل از قرار دادن عملکردی در یک سایدکار، در نظر بگیرید که عملکرد مورد نظر آیا به عنوان یک separate service عملکرد بهتری از یک traditional daemon دارد یا خیر؟
- همچنین در نظر بگیرید که آیا این عملکرد می تواند به عنوان یک کتابخانه پیاده سازی شود یا با استفاده از مکانیزم توسعه سنتی، Language-specific libraries ممکن است سطح عمیق تری از یکپارچگی به همراه سربار شبکه کمتری داشته باشند.

**چه زمانی از این الگو استفاده کنیم؟**

**از این الگو زمانی استفاده کنید که:**

- برنامه اصلی شما از مجموعه ای مختلفی از زبان ها و framework ها استفاده می کند. یک component واقع در یک سرویس sidecar می تواند توسط برنامه های کاربردی نوشته شده به زبان های مختلف با استفاده از framework های مختلف استفاده شود.
- یک component متعلق به یک تیم remote یا سازمان دیگری است.
- یک component یا feature باید در همان host برنامه قرار گیرد.
- شما به سرویسی نیاز دارید که lifecycle کلی برنامه اصلی شما را به اشتراک بگذارد، اما بتواند به طور مستقل به روز شود.
- شما به کنترل دقیقی بر resource limit برای یک resource یا component خاص نیاز دارید. برای مثال، ممکن است بخواهید مقدار حافظه استفاده شده از یک component خاص را محدود کنید. می توانید component را به عنوان یک سایدکار مستقر کنید و مصرف حافظه را مستقل از برنامه اصلی مدیریت کنید.

**این الگو ممکن است مناسب نباشد:**

- زمانی که ارتباطات بین فرآیندی(interprocess) باید بهینه شود. ارتباط بین یک برنامه والد و سرویس sidecar شامل مقداری سربار است، به ویژه latency در فراخوانی‌ها و این ممکن است یک trade-off مناسب برای [chatty interfaces](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.narendranaidu.com%2F2005%2F07%2Fchatty-interfaces-vs-chunky-interfaces.html&si=tamz0nilpyqb&st=post&k=D0IteyVK0OsTVVLSepofGzpq%2BlAwrTwL8U%2BsZqErtrs%3D) نباشد.
- برای small applications که در آن هزینه resource برای deploy یک سرویس sidecar برای هر instance ارزش استفاده از مزایای isolation را ندارد.
- زمانی که سرویس نیاز به scale متفاوت یا مستقل از برنامه های اصلی دارد. اگر چنین است، شاید بهتر باشد که این ویژگی را به عنوان یک سرویس جداگانه اجرا کنید.

### مثال:

الگوی sidecar برای بسیاری از سناریوها قابل استفاده است. چند مثال رایج:

- مورد Infrastructure API. تیم توسعه زیرساخت یک سرویس ایجاد می کند که در کنار هر برنامه کاربردی مستقر می شود، به جای یک language-specific client library برای دسترسی به زیرساخت. این سرویس به‌عنوان سایدکار بارگیری می‌شود و یک لایه مشترک برای سرویس infrastructure، از جمله logging، environment data، configuration store، discovery، health checks، و watchdog services فراهم می‌کند. سایدکار همچنین محیط host برنامه والد و process (یا container) را monitors می کند و اطلاعات را در یک سرویس متمرکز ثبت می کند.
- مدیریت NGINX/HAProxy: همیشه NGINX را با یک سرویس sidecar استقرار دهید که وضعیت environment را monitor می کند، سپس فایل پیکربندی NGINX را به روز می کند و در صورت نیاز به تغییر وضعیت، فرآیند را recycles می کند.
- روش Ambassador sidecar: یک سرویس [ambassador](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fambassador&si=tamz0nilpyqb&st=post&k=jn1yRD0WdNjD5ZGoyIFVuKKZoGC1ai4DYgODlSmtdQw%3D) را به عنوان یک sidecar مستقر(Deploy) کنید. برنامه از طریق Ambassador ارتباط می گیرد، که ثبت requestها، logging، routing و circuit breaking و سایر ویژگی های مرتبط با اتصال را مدیریت می کند.
- روش Offload proxy: یک پروکسی NGINX را در مقابل یک نمونه سرویس node.js قرار دهید تا محتوای فایل‌های static را برای سرویس ارائه دهد.

منابع مرتبط:

[Ambassador pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fambassador&si=tamz0nilpyqb&st=post&k=jn1yRD0WdNjD5ZGoyIFVuKKZoGC1ai4DYgODlSmtdQw%3D)  
[Strangler Fig pattern - Azure Architecture Center<br/>](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fstrangler-fig%3Fsource%3Drecommendations&si=tamz0nilpyqb&st=post&k=wCwLmYmvrOZDeu0QeKMi%2BCHDDnmZMfB6jCDi85dJ5Nk%3D)[Anti-corruption Layer pattern - Azure Architecture Center<br/>](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fanti-corruption-layer%3Fsource%3Drecommendations&si=tamz0nilpyqb&st=post&k=OcFKqIKkRnFSohaQTBwMWARWGuUDp2f9Y2awMhRW0Uw%3D)[Circuit Breaker pattern - Azure Architecture Center<br/>](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fcircuit-breaker%3Fsource%3Drecommendations&si=tamz0nilpyqb&st=post&k=d2WV4RAaxgIfM1XX2n9ivydfAf4fhDt8B5jo6uNQvlA%3D)[Bulkhead pattern - Azure Architecture Center](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fbulkhead%3Fsource%3Drecommendations&si=tamz0nilpyqb&st=post&k=AwA8JD8jhF7Vi6%2FrVlE6LzsGpmBZfUmxogiT98A8W8M%3D)