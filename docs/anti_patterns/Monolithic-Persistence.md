

ضدالگو (Antipattern): پایداری یکپارچه (Monolithic Persistence)

قرار دادن تمام داده‌های یک اپلیکیشن در یک انبار داده (data store) واحد می‌تواند عملکرد را تضعیف کند، یا به این دلیل که منجر به رقابت بر سر منابع (resource contention) می‌شود، یا به این دلیل که آن انبار داده برای برخی از انواع داده‌ها مناسب نیست.

زمینه و مشکل

در گذشته، اپلیکیشن‌ها بدون توجه به انواع مختلف داده‌هایی که ممکن بود نیاز به ذخیره‌سازی داشته باشند، از یک انبار داده واحد استفاده می‌کردند. سازمان‌ها از این روش برای ساده‌سازی طراحی اپلیکیشن یا تطبیق با مجموعه مهارت‌های موجود تیم توسعه استفاده می‌کردند.

سیستم‌های مدرن مبتنی بر ابر اغلب نیازمندی‌های عملکردی (functional) و غیرعملکردی (non-functional) بیشتری دارند. این سیستم‌ها نیاز به ذخیره‌سازی انواع ناهمگون داده‌ها مانند اسناد، تصاویر، داده‌های کش‌شده، پیام‌های در صف، لاگ‌های اپلیکیشن و تله‌متری دارند. پیروی از رویکرد سنتی و قرار دادن تمام این اطلاعات در یک انبار داده واحد می‌تواند به دو دلیل اصلی عملکرد را تضعیف کند:

ذخیره و بازیابی حجم زیادی از داده‌های نامرتبط در یک انبار داده واحد می‌تواند باعث رقابت بر سر منابع شود که منجر به زمان پاسخ‌دهی کند و شکست در اتصال (connection failures) می‌گردد.

صرف نظر از اینکه کدام انبار داده انتخاب شود، ممکن است برای همه انواع داده‌ها بهترین گزینه نباشد یا برای عملیات‌هایی که اپلیکیشن انجام می‌دهد، بهینه‌سازی نشده باشد.

مثال زیر یک کنترلر ASP.NET Web API را نشان می‌دهد که یک رکورد جدید به پایگاه داده اضافه کرده و نتیجه را نیز در یک لاگ ثبت می‌کند. لاگ در همان پایگاه داده‌ای که داده‌های تجاری قرار دارند، ذخیره می‌شود.

[کد نمونه]

نرخ تولید رکوردهای لاگ می‌تواند بر عملکرد عملیات‌های تجاری تأثیر بگذارد. و اگر مؤلفه دیگری، مانند یک ناظر فرآیند اپلیکیشن، به طور منظم داده‌های لاگ را بخواند و پردازش کند، آن نیز می‌تواند بر عملیات‌های تجاری تأثیرگذار باشد.

راه‌حل

داده‌ها را بر اساس کاربردشان جدا کنید. برای هر مجموعه داده، یک انبار داده انتخاب کنید که با نحوه استفاده شما از آن مجموعه داده بیشترین تطابق را داشته باشد. در مثال قبلی، اپلیکیشن باید لاگ‌ها را در یک انبار داده جدا از پایگاه داده‌ای که داده‌های تجاری را نگهداری می‌کند، ثبت نماید:

[کد نمونه]

مشکلات و ملاحظات

داده‌ها را بر اساس نحوه استفاده و دسترسی به آن‌ها جدا کنید. برای مثال، اطلاعات لاگ و داده‌های تجاری را در یک انبار داده یکسان ذخیره نکنید. این انواع داده، نیازمندی‌ها و الگوهای دسترسی متفاوتی دارند. رکوردهای لاگ ذاتاً ترتیبی (sequential) هستند، در حالی که داده‌های تجاری به احتمال زیاد نیاز به دسترسی تصادفی (random access) دارند و اغلب رابطه‌ای (relational) هستند.

الگوی دسترسی به داده را برای هر نوع داده در نظر بگیرید. برای مثال، گزارش‌ها و اسناد فرمت‌بندی‌شده را در یک پایگاه داده اسنادی (document database) مانند Azure Cosmos DB ذخیره کنید. از Azure Cache for Redis برای کش کردن داده‌های موقت استفاده نمایید.

اگر این راهنما را دنبال کردید اما همچنان به محدودیت‌های پایگاه داده رسیدید، پایگاه داده را ارتقا دهید (scale up). همچنین ارتقای افقی (scaling horizontally) و پارتیشن‌بندی بار کاری بین سرورهای پایگاه داده را در نظر بگیرید. با این حال، پارتیشن‌بندی ممکن است نیاز به طراحی مجدد اپلیکیشن داشته باشد. برای اطلاعات بیشتر، به بخش «پارتیشن‌بندی داده‌ها» مراجعه کنید.

شناسایی مشکل

سیستم می‌تواند به شدت کند شده و در نهایت زمانی که منابعی مانند اتصالات پایگاه داده به اتمام می‌رسند، از کار بیفتد.

برای شناسایی علت می‌توانید مراحل زیر را انجام دهید:

سیستم را ابزار دقیق‌سنجی (instrument) کنید تا آمار کلیدی عملکرد را ثبت نماید. اطلاعات زمانی را برای هر عملیات ثبت کنید و نقاطی را که اپلیکیشن داده‌ها را می‌خواند و می‌نویسد، مشخص کنید.

سیستم را برای چند روز در یک محیط در حال کار (production) نظارت کنید تا یک دید واقعی از نحوه استفاده از سیستم به دست آورید. اگر نمی‌توانید این فرآیند را انجام دهید، تست‌های بار اسکریپت‌شده را با حجم واقع‌بینانه‌ای از کاربران مجازی که یک سری عملیات معمول را انجام می‌دهند، اجرا کنید.

از داده‌های تله‌متری برای شناسایی دوره‌های عملکرد ضعیف استفاده کنید.

شناسایی کنید که در طول آن دوره‌ها به کدام انبارهای داده دسترسی پیدا شده است.

منابع ذخیره‌سازی داده‌ای را که ممکن است با رقابت مواجه شوند، شناسایی کنید.

مثال تشخیصی

بخش‌های زیر این مراحل را برای اپلیکیشن نمونه‌ای که قبلاً شرح داده شد، به کار می‌گیرند.

نمودار زیر نتایج تست بار اپلیکیشن نمونه را نشان می‌دهد. این تست از یک بار پله‌ای تا ۱۰۰۰ کاربر همزمان استفاده می‌کند.

[نمودار ۱]

با افزایش بار به ۷۰۰ کاربر، توان عملیاتی نیز افزایش می‌یابد. اما در آن نقطه، توان عملیاتی ثابت می‌شود و به نظر می‌رسد سیستم با حداکثر ظرفیت خود کار می‌کند. میانگین زمان پاسخ‌دهی به تدریج با بار کاربران افزایش می‌یابد که نشان می‌دهد سیستم نمی‌تواند با تقاضا هماهنگ شود.

اگر سیستم در حال کار را نظارت کنید، ممکن است متوجه الگوهایی شوید. برای مثال، ممکن است زمان پاسخ‌دهی هر روز در یک زمان مشخص به طور قابل توجهی کاهش یابد. یک بار کاری منظم یا یک کار دسته‌ای (batch job) زمان‌بندی‌شده می‌تواند این نوسان را ایجاد کند. یا ممکن است سیستم در ساعات خاصی کاربران بیشتری داشته باشد. شما باید بر روی داده‌های تله‌متری مربوط به این رویدادها تمرکز کنید.

به دنبال ارتباط بین افزایش زمان پاسخ‌دهی و افزایش فعالیت پایگاه داده یا ورودی/خروجی (I/O) به منابع مشترک باشید. اگر ارتباطی وجود داشته باشد، به این معنی است که پایگاه داده ممکن است یک گلوگاه (bottleneck) باشد.

نمودار بعدی میزان استفاده از واحدهای توان عملیاتی پایگاه داده (DTUs) را در طول تست بار نشان می‌دهد. DTU معیاری از ظرفیت موجود است و ترکیبی از استفاده از CPU، تخصیص حافظه و نرخ ورودی/خروجی است. میزان استفاده از DTU به سرعت به ۱۰۰٪ می‌رسد. در نمودار قبلی، توان عملیاتی در این نقطه به اوج خود رسید. استفاده از پایگاه داده تا پایان تست بالا باقی می‌ماند. یک کاهش جزئی در انتها وجود دارد که می‌تواند ناشی از محدودسازی (throttling)، رقابت برای اتصالات پایگاه داده یا عوامل دیگر باشد.

[نمودار ۲]

انبارهای داده را ابزار دقیق‌سنجی کنید تا جزئیات سطح پایین فعالیت را ثبت نمایید. در اپلیکیشن نمونه، آمار دسترسی به داده حجم بالایی از عملیات درج (insert) را هم برای جدول PurchaseOrderHeader و هم برای جدول MonoLog نشان می‌دهد.

[جدول آمار]

در این مرحله، می‌توانید کد منبع را بازبینی کرده و بر نقاطی تمرکز کنید که اپلیکیشن به منابع مورد رقابت دسترسی پیدا می‌کند. به دنبال شرایطی مانند موارد زیر باشید:

داده‌هایی که منطقاً جدا هستند اما در یک انبار داده یکسان نوشته می‌شوند. داده‌هایی مانند لاگ‌ها، گزارش‌ها و پیام‌های در صف نباید در همان پایگاه داده‌ای که اطلاعات تجاری قرار دارند، نگهداری شوند.

عدم تطابق بین انتخاب انبار داده و نوع داده، مانند ذخیره کردن اشیاء بزرگ (blobs) یا اسناد XML در یک پایگاه داده رابطه‌ای.

داده‌هایی با الگوهای استفاده متفاوت که از یک انبار داده مشترک استفاده می‌کنند. یک مثال، ذخیره داده‌های با نرخ نوشتن بالا و خواندن پایین در کنار داده‌های با نرخ نوشتن پایین و خواندن بالا است.

در این مثال، اپلیکیشن به‌روزرسانی می‌شود تا لاگ‌ها را در یک انبار داده جداگانه بنویسد. نمودار زیر نتایج تست بار را نشان می‌دهد.

[نمودار ۳]

الگوی توان عملیاتی شبیه به نمودار قبلی است، اما نقطه‌ای که عملکرد به اوج می‌رسد تقریباً ۵۰۰ درخواست در ثانیه بالاتر است. میانگین زمان پاسخ‌دهی به طور جزئی پایین‌تر است. با این حال، این آمار تمام داستان را بیان نمی‌کند. تله‌متری برای پایگاه داده تجاری نشان می‌دهد که استفاده از DTU به جای ۱۰۰٪، در حدود ۷۵٪ به اوج می‌رسد.

[نمودار ۴]

به طور مشابه، حداکثر استفاده از DTU در پایگاه داده لاگ تنها به حدود ۷۰٪ می‌رسد. پایگاه‌های داده دیگر عامل محدودکننده در عملکرد سیستم نیستند.

منابع مرتبط

انتخاب انبار داده مناسب

معیارهای انتخاب یک انبار داده

دسترسی به داده برای راه‌حل‌های بسیار مقیاس‌پذیر با استفاده از SQL، NoSQL و پایداری چندگانه (polyglot persistence)

پارتیشن‌بندی داده‌ها
