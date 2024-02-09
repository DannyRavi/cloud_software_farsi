در این قسمت به سراغ پترن CQRS می رویم، CQRS مخفف Command and Query Responsibility Segregation است، الگویی که عملیات خواندن و به روز رسانی را برای ذخیره داده‌ها جدا می کند. پیاده سازی CQRS در برنامه شما می تواند performance، scalability و امنیت آن را به حداکثر برساند. انعطاف‌پذیری ایجاد شده توسط CQRS به سیستم اجازه می‌دهد در طول زمان بهتر تکامل یابد و از ایجاد merge conflicts توسط به‌روزرسانی‌ها جلوگیری می‌کند.

**طرح صورت مسئله:**

در معماری‌های قدیمی‌تر نرم افزار، از data model یکسانی برای query و به روز رسانی پایگاه داده استفاده می شود. این روش خیلی ساده بوده و برای عملیات اولیه و معمولی CRUD به خوبی کار می کند. با این حال، در برنامه های پیچیده تر، این روش خوب نیست. به عنوان مثال: در سمت خواندن پایگاه داده، برنامه ممکن است queryهای مختلفی را انجام دهد و data transfer objects (DTO) را با اشکال مختلف برگرداند. Object mapping می تواند پیچیده شود. در سمت نوشتن، مدل ممکن است validation و business logic پیچیده‌ای را پیاده سازی کند. در نتیجه؛ با یک مدل بیش از حد پیچیده روبرو می‌شویم که بیش از حد کار می کند و به نوعی تحت فشار است. حجم عملیات خواندن و نوشتن نامتقارن و نابرابر است و به جهت مشکلات performance در این موارد نیاز به scale برنامه بوجود می ‌آید.

![[Pasted image 20240209180928.png]]

- معمولا یک عدم تطابق بین نمایش خواندن و نوشتن داده ها وجود دارد، مانند ستون ها یا ویژگی های اضافی که باید به درستی به روز شوند، حتی اگر به عنوان بخشی از یک عملیات مورد نیاز نباشند.
- زمانی که عملیات به صورت موازی روی یک مجموعه از داده ها انجام شود، اختلاف داده ها ممکن است منجر به conflict شود.
- رویکرد قدیمی می تواند به دلیل بارگذاری روی ذخیره داده ها و لایه دسترسی به داده ها و پیچیدگی queryهای مورد نیاز برای بازیابی اطلاعات، تأثیر منفی بر performance داشته باشد.
- مدیریت امنیت و permissions می‌تواند پیچیده شود، زیرا هر entity تابع عملیات خواندن و نوشتن است، که ممکن است داده‌ها را در context اشتباه نشان دهد.

**راه حل:**

به طور کلی CQRS خواندن و نوشتن را در مدل‌های مختلف جدا می‌کند و از دستورات برای به‌روزرسانی داده‌ها و پرس و جو برای خواندن داده‌ها استفاده می‌کند.

- دستورات باید task-based باشند، نه داده محور (data centric).  
    ("Book hotel room", not "set ReservationStatus to Reserved").
- دستورات ممکن است در یک queue برای پردازش ناهمزمان قرار گیرند، نه اینکه به صورت همزمان پردازش شوند.
- کوئری ها هرگز پایگاه داده را تغییر نمی دهند. یک پرس و جو یک [DTO](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FData_transfer_object&si=fd2qjvfxnrbr&st=post&k=tbwZCD27PVOXLV89%2FFrEa%2BJ9OSCQ7vXzC5YsPlFf6v8%3D) را برمی گرداند که هیچ [domain knowledge](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDomain_knowledge&si=fd2qjvfxnrbr&st=post&k=Kh8yOzfix82qSQP%2FrNzVFp%2BDCan3K95cLi3A9ZjQ2II%3D) را کپسوله نمی کند.

سپس می توان مدل ها را جدا کرد، همانطور که در نمودار زیر نشان داده شده است.

![[Pasted image 20240209180941.png]]


داشتن مدل های پرس و جو و به روز رسانی جداگانه، طراحی و پیاده سازی را ساده می کند. با این حال، یک نقطه ضعف این است که کد CQRS نمی تواند به طور خودکار از یک طرح پایگاه داده با استفاده از مکانیسم هایی مانند ابزارهای O/RM تولید شود.

برای جداسازی بیشتر، می توانید داده های خوانده شده را از داده های نوشتن به صورت فیزیکی جدا کنید. در آن صورت، پایگاه داده خوانده شده می تواند از طرح داده های خود استفاده کند که برای پرس و جوها بهینه شده است. به عنوان مثال، می تواند یک نمای مادی از داده ها را ذخیره کند تا از اتصالات پیچیده یا O/RM mapping پیچیده جلوگیری کند. حتی ممکن است از نوع دیگری از ذخیره داده استفاده کند. به عنوان مثال، پایگاه داده نوشتن ممکن است relational باشد، در حالی که پایگاه داده خوانده شده یک پایگاه document database است.

اگر از پایگاه‌های داده خواندن و نوشتن جداگانه استفاده می‌شود، باید آنها را sync نگه داشت. این حالت معمولاً با publish شدن یک event توسط مدل نوشتن زمانی که پایگاه داده را به روز شود، انجام می شود. برای اطلاعات بیشتر در مورد استفاده از رویدادها، به سبک معماری [Event-driven architecture style](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fguide%2Farchitecture-styles%2Fevent-driven&si=fd2qjvfxnrbr&st=post&k=p1hxTt2gTdpS3j6BJGTD5NphLMt4R87wU9Ym6%2Bu1TW8%3D) مراجعه کنید. به روز رسانی پایگاه داده و publish شدن event باید در یک تراکنش (transaction) انجام شود.

![[Pasted image 20240209180954.png]]

ذخیره داده های خواندن می‌تواند یک (replica)کپی read-only از ذخیره داده های نوشتن باشد، یا ذخیره داده های خواندن و نوشتن می‌توانند ساختار متفاوتی داشته باشند. استفاده از چند replica به صورت read-only می تواند performance query را افزایش دهد، به خصوص در سناریوهای distributed شده که در آن replica های فقط خواندنی نزدیک به برنامه در حال اجرا قرار دارند.

جداسازی ذخیره داده‌های خواندن و نوشتن اجازه می‌دهد تا هر کدام به‌طور مناسب برای مطابقت با load برنامه و دستورات، scale شوند. به عنوان مثال، ذخیره داده های خواندن معمولاً با load بسیار بالاتری نسبت به ذخیره داده‌های نوشتن مواجه می شوند.

برخی از پیاده سازی های CQRS از الگوی Event Sourcing استفاده می کنند. با این الگو، وضعیت برنامه به عنوان یک دنباله‌ای از eventها ذخیره می شود. هر event نشان دهنده مجموعه ای از تغییرات در داده ها است. وضعیت فعلی با پخش مجدد رویدادها ساخته می شود. در موضوع CQRS، یکی از مزایای Event Sourcing این است که از همان رویدادها می توان برای اطلاع رسانی (notify) به اجزای دیگر استفاده کرد - به ویژه برای اطلاع رسانی(notify) به مدل خوانده شده(read model). مدل خواندن از eventها برای ایجاد یک snapshot از وضعیت فعلی استفاده می کند که برای پرس و جوها کارآمدتر است. با این حال، Event Sourcing پیچیدگی را به طراحی اضافه می کند.

**مزایای CQRS عبارتند از:**

- مقیاس بندی مستقل (**Independent scaling**): CQRS اجازه می دهد تا حجم کار خواندن و نوشتن به طور مستقل scale شود و ممکن است منجر به conflict در داده‌ها کمتر شود.
- **حالت Optimized data schemas:** سمت خواندن داده ها می تواند از طرحی استفاده کند که برای queryها بهینه شده است، در حالی که سمت نوشتن داده ها از طرحی استفاده می کند که برای به روز رسانی بهینه شده است.
- **امنیت (Security)**: اطمینان از اینکه فقط موجودیت های(entities) مناسب روی نوشتن داده‌ها در پایگاه داده انجام می گیرد.
- **تفکیک نگرانی ها(Separation of concerns):** جداسازی دو طرف خواندن و نوشتن می‌تواند منجر به مدل‌هایی شود که قابلیت نگهداری و انعطاف‌پذیری بیشتری دارند. بیشتر منطق پیچیده کسب و کار وارد مدل نوشتن می شود. مدل خواندن می تواند نسبتا ساده باشد.
- **پرس و جوهای ساده تر (Simpler queries)**: با ذخیره به صورت [materialized view](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMaterialized_view&si=fd2qjvfxnrbr&st=post&k=Z8y8HLJ%2FluiZR2ObNS%2BQAgsl%2BstBq9zApj2O8V2stBA%3D) در پایگاه داده‌ای ‌ که جهت خواندن داده طراحی شده، برنامه می تواند از اتصالات پیچیده هنگام query زدن جلوگیری کند.

### مسائل و ملاحظات:

برخی از چالش های اجرای این الگو عبارتند از:

- **پیچیدگی (Complexity):** ایده اصلی CQRS ساده است. اما می‌تواند منجر به طراحی اپلیکیشن پیچیده‌تر شود، به‌ویژه اگر شامل الگوی Event Sourcing باشد.
- **پیام رسانی (Messaging):** اگرچه CQRS به messaging نیازی ندارد، استفاده از messaging برای پردازش دستورات و publish update events متداول است. در آن صورت، برنامه باید با failure شدن پیام ها یا پیام های تکراری مقابله کند. برای برخورد با دستوراتی که اولویت های متفاوتی دارند، راهنمای [Priority Queues](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fpriority-queue&si=fd2qjvfxnrbr&st=post&k=Cpx9OHhTfgzVPWdj1GYG05wMyzuspCEXTv1Obs0gRu0%3D) را ببینید.
- **استحکام احتمالی (Eventual consistency):** اگر پایگاه داده‌های خواندن و نوشتن ‌ را از هم جدا کنید، داده های خوانده شده ممکن است قدیمی شده باشند یا به روز نباشند. دیتابیس مربوط به خواندن باید بروز رسانی شود تا تغییرات در دریتابیس نوشتن را منعکس کند و تشخیص زمانی که کاربر براساس داده‌های خوانده شده قدیمی درخواستی صادر کرده است و این به اصطلاح sync شدن داده ها در دیتابیس بیس خواندن و نوشتن ایجاد کند، می‌تواند دشوار باشد.

**چه زمانی از این الگو استفاده کنیم؟**

برای CQRS سناریوهای زیر را در نظر بگیرید:

- دامنه های مشارکتی که در آن بسیاری از کاربران به طور موازی به داده های مشابه دسترسی دارند. CQRS به شما اجازه می دهد تا دستوراتی را با جزئیات کافی تعریف کنید تا merge conflicts را در سطوح مختلف به حداقل برسانید و تداخل هایی که به وجود می آیند را می توان با command هایی merged کرد.
- رابط های کاربری Task-based که در آن کاربران از طریق یک فرآیند پیچیده به عنوان یک سری مراحل یا با domain models پیچیده هدایت می شوند. write model دارای یک command-processing stack کامل با business logic و validation ورودی و business validation است. write model ممکن است مجموعه‌ای از objects مرتبط را به‌عنوان یک واحد برای تغییرات داده‌ها (یک aggregate، در اصطلاح DDD) در نظر بگیرد و اطمینان حاصل کند که این object همیشه در یک حالت ثابت هستند. read model هیچ business logic یا validation stack ندارد و فقط یک DTO را برای استفاده در یک view model برمی گرداند. read model در نهایت با write model سازگار است.
- سناریوهایی که عملکرد خواندن داده ها باید جدا از عملکرد نوشتن داده ها تنظیم شود به خصوص زمانی که تعداد خواندن ها بسیار بیشتر از تعداد نوشتن ها باشد. در این سناریو، می توانید read model را scale out کنید، اما write model را فقط در چند instances محدود اجرا کنید. تعداد کمی از نمونه های write model نیز به به حداقل رساندن وقوع merge conflicts کمک می کند.
- سناریوهایی که در آن یک تیم از توسعه دهندگان می توانند بر روی [domain model](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDomain_model&si=fd2qjvfxnrbr&st=post&k=hMTWUJTUtIacVaJVLAZ3cVsplGPiI%2F%2FE9WET10dn0ac%3D) پیچیده که بخشی از write model است تمرکز کنند، و تیم دیگری می توانند بر روی read model و رابط های کاربری تمرکز کنند.
- سناریوهایی که در آن انتظار می‌رود سیستم در طول زمان تکامل یابد و ممکن است چندین ورژن از model را داشته باشد یا business rules به طور منظم تغییر می‌کنند.
- ادغام با سیستم های دیگر، به ویژه در ترکیب با [event sourcing](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fmicroservices.io%2Fpatterns%2Fdata%2Fevent-sourcing.html&si=fd2qjvfxnrbr&st=post&k=0SA%2FU67CKnVQqyRC%2FEuz6NIFOfC7HAjtDzyyQg3DcmI%3D) که در آن خرابی موقت یک زیرسیستم نباید بر در دسترس بودن سایر سیستم ها تأثیر بگذارد.

**این الگو در موارد زیر توصیه نمی شود:**

- دامنه یا [business rules](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FBusiness_rules_approach&si=fd2qjvfxnrbr&st=post&k=VkmUHRyBAoPrw6UrvM2hpCXY9bXC2UBC8KbcoLFTbvM%3D) ساده است.
- یک رابط کاربری ساده به سبک CRUD و عملیات دسترسی به داده برای برنامه مورد نظر مناسب باشد.

استفاده از CQRS را در بخش های محدودی از سیستم خود در نظر بگیرید که در آن بیشترین ارزشو حساسیت را دارد.

## معرفی Event Sourcing and CQRS pattern:

الگوی CQRS اغلب همراه با الگوی [Event Sourcing](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fmicroservices.io%2Fpatterns%2Fdata%2Fevent-sourcing.html&si=fd2qjvfxnrbr&st=post&k=0SA%2FU67CKnVQqyRC%2FEuz6NIFOfC7HAjtDzyyQg3DcmI%3D) استفاده می شود. سیستم‌های مبتنی بر CQRS از مدل‌های داده خواندن و نوشتن جداگانه استفاده می‌کنند که هر کدام برای وظایف مربوطه طراحی شده‌اند و اغلب در ذخیره سازی فیزیکی مجزا قرار دارند. هنگامی که با الگوی Event Sourcing استفاده می شود، ذخیره رویدادها write model است و منبع official source داده‌ها است. read model یک سیستم مبتنی بر CQRS که [materialized views](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMaterialized_view&si=fd2qjvfxnrbr&st=post&k=Z8y8HLJ%2FluiZR2ObNS%2BQAgsl%2BstBq9zApj2O8V2stBA%3D) داده‌ها معمولاً به صورت viewsهای بسیار denormalized شده ارائه می‌کند. این viewها بر اساس واسط ها و الزامات نمایش برنامه طراحی شده اند که به حداکثر رساندن performance نمایش و query کمک می کند.

استفاده از stream of events به‌عنوان ذخیره‌سازی نوشتن به‌جای داده‌های واقعی در یک نقطه خاصی از زمان قطعا از update conflicts در یک aggregate اجتناب می‌کند و performance و scalability را به حداکثر می‌رساند. از eventها می توان برای تولید ناهمزمان [materialized views](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMaterialized_view&si=fd2qjvfxnrbr&st=post&k=Z8y8HLJ%2FluiZR2ObNS%2BQAgsl%2BstBq9zApj2O8V2stBA%3D) داده هایی که برای پر کردن ذخیره‌سازی خوانده استفاده می شود استفاده کرد.

از آنجایی که ذخیره‌سازی رویداد (event store) منبع رسمی اطلاعات داده‌هاست، می‌توان [materialized views](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMaterialized_view&si=fd2qjvfxnrbr&st=post&k=Z8y8HLJ%2FluiZR2ObNS%2BQAgsl%2BstBq9zApj2O8V2stBA%3D) را حذف کرد و همه رویدادهای گذشته را مجدداً پخش کرد تا زمانی که سیستم تکامل می‌یابد یا زمانی که مدل read model باید تغییر کند یا نمایش جدیدی از وضعیت فعلی ایجاد شود. materialized views یافته در واقع یک حافظه cache پنهان read-only از داده ها هستند.

هنگام استفاده از CQRS همراه با الگوی Event Sourcing، موارد زیر را در نظر بگیرید:

- مانند هر سیستمی که دیتابیس ذخیره‌ داده‌های نوشتن و خواندن آن مجزا هستند، سیستم‌های مبتنی بر این الگو تنها [eventually consistent](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FEventual_consistency&si=fd2qjvfxnrbr&st=post&k=X61d6ZIuNqnnc%2FyTZZ9OxOXxENED7dDUxpKgXVs4HII%3D) هستند و بین ایجاد رویداد و به‌روزرسانی ذخیره‌ داده ها تأخیر و ناهمزمانی وجود خواهد داشت.
- این الگو پیچیدگی می‌افزاید، زیرا کد باید برای شروع و مدیریت رویدادها، و جمع‌آوری یا به‌روزرسانی view یا objects مناسب مورد نیاز کوئری‌ها یا مدل خوانده شده ایجاد شود. پیچیدگی الگوی CQRS هنگامی که با الگوی Event Sourcing استفاده می‌شود، می‌تواند implementation موفقیت‌آمیز را دشوارتر کند و نیاز به رویکرد متفاوتی برای طراحی سیستم‌ها دارد. با این حال، Event Sourcing می‌تواند [model domain](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDomain_model&si=fd2qjvfxnrbr&st=post&k=hMTWUJTUtIacVaJVLAZ3cVsplGPiI%2F%2FE9WET10dn0ac%3D) را آسان‌تر کند، و بازسازی viewها یا ایجاد viewهای جدید را آسان‌تر می‌کند زیرا هدف از تغییرات در داده‌ها حفظ می‌شود.
- ایجاد materialized views برای استفاده در read model یا پیش بینی داده ها با replaying و handling رویدادها برای entities یا مجموعه های خاص می تواند به زمان پردازش و استفاده از منابع سخت افزاری قابل توجهی نیاز داشته باشد. این امر به ویژه در صورتی صادق است که به جمع بندی یا آنالیز مقادیر در زمان‌های طولانی نیاز داشته باشد، زیرا ممکن است همه رویدادهای مرتبط نیاز به بررسی داشته باشند. این مشکل را با implementing snapshots از داده‌ها در فواصل زمانی برنامه‌ریزی‌شده مانند شمارش کل تعداد یک عمل خاص که رخ داده است (total count of the number of a specific action that has occurred) یا وضعیت فعلی یک entity، حل کنید.

### مثالی از الگوی CQRS:

کد زیر چکیده ای از پیاده سازی CQRS را نشان می دهد که از تعاریف مختلفی برای مدل های خواندن و نوشتن استفاده می کند. رابط‌های مدل هیچ ویژگی ذخیره‌سازی داده‌های زیربنایی را تعیین نمی‌کنند و می‌توانند به طور مستقل توسعه یافته و تنظیم شوند زیرا این رابط‌ها از هم جدا هستند.

کد زیر تعریف مدل خوانده شده را نشان می دهد.


```csharp
// Query interface
namespace ReadModel
{
  public interface ProductsDao
  {
    ProductDisplay FindById(int productId);
    ICollection<ProductDisplay> FindByName(string name);
    ICollection<ProductInventory> FindOutOfStockProducts();
    ICollection<ProductDisplay> FindRelatedProducts(int productId);
  }

  public class ProductDisplay
  {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal UnitPrice { get; set; }
    public bool IsOutOfStock { get; set; }
    public double UserRating { get; set; }
  }

  public class ProductInventory
  {
    public int Id { get; set; }
    public string Name { get; set; }
    public int CurrentStock { get; set; }
  }
}
```


این سیستم به کاربران اجازه می دهد تا محصولات را رتبه بندی کنند. کد برنامه این کار را با استفاده از دستور RateProduct نشان داده شده در کد زیر انجام می دهد.

```csharp
public interface ICommand
{
  Guid Id { get; }
}

public class RateProduct : ICommand
{
  public RateProduct()
  {
    this.Id = Guid.NewGuid();
  }
  public Guid Id { get; set; }
  public int ProductId { get; set; }
  public int Rating { get; set; }
  public int UserId {get; set; }
}
```

این سیستم از کلاس ProductsCommandHandler برای کنترل دستورات ارسال شده توسط برنامه استفاده می کند. کلاینت ها معمولاً دستورات را از طریق یک سیستم messaging مانند queue به دامنه ارسال می کنند. کنترل کننده فرمان این دستورات را می پذیرد و متدهای رابط دامنه را فراخوانی می کند. جزئیات هر فرمان به گونه ای طراحی شده است که شانس درخواست های conflict را کاهش دهد. کد زیر یک طرح کلی از کلاس ProductsCommandHandler را نشان می _دهد._

```csharp
public class ProductsCommandHandler :
    ICommandHandler<AddNewProduct>,
    ICommandHandler<RateProduct>,
    ICommandHandler<AddToInventory>,
    ICommandHandler<ConfirmItemShipped>,
    ICommandHandler<UpdateStockFromInventoryRecount>
{
  private readonly IRepository<Product> repository;

  public ProductsCommandHandler (IRepository<Product> repository)
  {
    this.repository = repository;
  }

  void Handle (AddNewProduct command)
  {
    ...
  }

  void Handle (RateProduct command)
  {
    var product = repository.Find(command.ProductId);
    if (product != null)
    {
      product.RateProduct(command.UserId, command.Rating);
      repository.Save(product);
    }
  }

  void Handle (AddToInventory command)
  {
    ...
  }

  void Handle (ConfirmItemsShipped command)
  {
    ...
  }

  void Handle (UpdateStockFromInventoryRecount command)
  {
    ...
  }
}
```



###   مراحل بعدی

**الگوها و راهنمایی های زیر هنگام اجرای این الگو مفید هستند**

- مورد [Data Consistency Primer](https://virgool.io/p/fd2qjvfxnrbr/edit). مشکلاتی را که معمولاً به دلیل سازگاری نهایی بین ذخیره‌های داده‌های خواندن و نوشتن در هنگام استفاده از الگوی CQRS با آن مواجه می‌شوند و اینکه چگونه می‌توان این مسائل را حل کرد، توضیح می‌دهد.
- مورد [Horizontal, vertical, and functional data partitioning](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fbest-practices%2Fdata-partitioning&si=fd2qjvfxnrbr&st=post&k=I0Z87fDhU2W1U89wmWdnMmGKX%2BJxRAaHt%2F5L%2BY0zgR4%3D). بهترین روش‌ها را برای تقسیم داده‌ها به پارتیشن‌هایی توصیف می‌کند که می‌توانند به طور جداگانه مدیریت شوند و به آن‌ها دسترسی داشته باشیم تا مقیاس‌پذیری را بهبود ببخشیم، اختلافات را کاهش دهیم و عملکرد را بهینه کنیم.
- الگوها و شیوه‌های راهنمایی [CQRS Journey](https://virgool.io/p/fd2qjvfxnrbr/edit). به طور خاص، معرفی الگوی[Introducing the Command Query Responsibility Segregation pattern](https://virgool.io/p/fd2qjvfxnrbr/edit) و زمان مفید بودن آن را بررسی می‌کند و [Epilogue: Lessons Learned](https://virgool.io/p/fd2qjvfxnrbr/edit) به شما کمک می‌کند تا برخی از مسائلی را که هنگام استفاده از این الگو پیش می‌آیند، درک کنید.

### **مراجع مرتبط**

- الگوی موجود در لینک [Event Sourcing pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fevent-sourcing&si=fd2qjvfxnrbr&st=post&k=C%2BFIDsF2%2B8eEglx%2FHJ10%2F7mIxNucakBn04N86p7U8t0%3D) با جزئیات بیشتری توضیح می دهد که چگونه می توان از Event Sourcing با الگوی CQRS برای ساده کردن وظایف در حوزه های پیچیده و در عین حال بهبود عملکرد، مقیاس پذیری و پاسخگویی استفاده کرد. و همچنین نحوه ارائه یکپارچگی برای transactional data را نشان میدهد.
- الگوی [Materialized View pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fmaterialized-view&si=fd2qjvfxnrbr&st=post&k=TvOT1z3kisnINYSfnFp3o%2F99NY5nMr4%2FmaTLa576weg%3D). یک read model پیاده سازی CQRS می تواند حاوی materialized views یافته از داده های write model باشد، یا مدل read model شده می تواند برای تولید materialized views استفاده شود.

- [munication in a microservice architecture](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fdotnet%2Farchitecture%2Fmicroservices%2Farchitect-microservice-container-applications%2Fcommunication-in-microservice-architecture%3Fsource%3Drecommendations&si=fd2qjvfxnrbr&st=post&k=eXs6bs9%2FWyzJH%2F%2Fyxol55%2FTS9GRHGXBZ7FO0b8AitrI%3D)
- [Saga pattern - Azure Design Patterns](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Freference-architectures%2Fsaga%2Fsaga%3Fsource%3Drecommendations&si=fd2qjvfxnrbr&st=post&k=OJWjmot9SIXpIIGVi8RUkKJFKmLGSI4g7CoeFVz2g40%3D)
- [Event Sourcing pattern - Azure Architecture Center](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fazure%2Farchitecture%2Fpatterns%2Fevent-sourcing%3Fsource%3Drecommendations&si=fd2qjvfxnrbr&st=post&k=Sp2fKYloZLuuu6B03uNxzKCXj5bkiSxR064wjBKXrxk%3D)
- [Designing a DDD-oriented microservice](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fdotnet%2Farchitecture%2Fmicroservices%2Fmicroservice-ddd-cqrs-patterns%2Fddd-oriented-microservice%3Fsource%3Drecommendations&si=fd2qjvfxnrbr&st=post&k=H4NICAramIFUJowcr7Tijr7etFuelX9nHS3OSX7FT5E%3D)

[cqrs](https://virgool.io/tag/cqrs)[kubernetes](https://virgool.io/tag/kubernetes)[microservice](https://virgool.io/tag/microservice)[سبک معماری مایکروسرویس ها (Microservices Architecture](https://virgool.io/tag/%D8%B3%D8%A8%DA%A9%20%D9%85%D8%B9%D9%85%D8%A7%D8%B1%DB%8C%20%D9%85%D8%A7%DB%8C%DA%A9%D8%B1%D9%88%D8%B3%D8%B1%D9%88%DB%8C%D8%B3%20%D9%87%D8%A7%20(Microservices%20Architecture)[cloud design pattern](https://virgool.io/tag/cloud%20design%20pattern)