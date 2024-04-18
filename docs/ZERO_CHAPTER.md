

# What is System Design?

System design is the process of defining the elements of a system, as well as their interactions and relationships, in order to satisfy a set of specified requirements.

It involves taking a problem statement, breaking it down into smaller components and designing each component to work together effectively to achieve the overall goal of the system. This process typically includes analyzing the current system (if any) and determining any deficiencies, creating a detailed plan for the new system, and testing the design to ensure that it meets the requirements. It is an iterative process that may involve multiple rounds of design, testing, and refinement.

In software engineering, system design is a phase in the software development process that focuses on the high-level design of a software system, including the architecture and components.

It is also one of the important aspects of the interview process for software engineers. Most of the companies have a dedicated system design interview round, where they ask the candidates to design a system for a given problem statement. The candidates are expected to come up with a detailed design of the system, including the architecture, components, and their interactions. They are also expected to discuss the trade-offs involved in their design and the alternatives that they considered.


طراحی سیستم فرآیند تعریف عناصر یک سیستم و همچنین تعاملات و روابط آنها برای برآورده کردن مجموعه ای از الزامات خاص است.

این شامل گرفتن یک صورت مسئله، شکستن آن به اجزای کوچکتر و طراحی هر جزء برای کارکرد موثر با هم برای دستیابی به هدف کلی سیستم است. این فرآیند به طور معمول شامل تجزیه و تحلیل سیستم فعلی (در صورت وجود) و تعیین هر گونه نیازمندی و ایجاد یک برنامه دقیق برای سیستم جدید و آزمایش طراحی برای اطمینان از مطابقت آن با الزامات است. این یک فرآیند تکرارشونده است که ممکن است شامل چندین دور طراحی، آزمایش و اصلاح باشد.

در مهندسی نرم افزار، طراحی سیستم مرحله ای در فرآیند توسعه نرم افزار است که بر روی طراحی سطح بالای یک سیستم نرم افزاری از جمله معماری و اجزا، تمرکز می‌کند.

این همچنین یکی از جنبه‌های مهم فرآیند مصاحبه برای مهندسان نرم افزار است. اکثر شرکت‌ها یک دور مصاحبه اختصاصی برای طراحی سیستم دارند، جایی که از نامزدها می‌خواهند سیستمی را برای یک صورت مسئله خاص طراحی کنند. از انتظار می‌رود که نامزدها با یک طراحی دقیق از سیستم، از جمله معماری، اجزا و تعاملات آنها ارائه شوند. همچنین انتظار می‌رود که آنها در مورد معاوضه‌های موجود در طراحی خود و گزینه‌های جایگزینی که در نظر گرفته‌اند بحث کنند.

-------------

---------------


------------------------

# Performance vs Scalability

A service is **scalable** if it results in increased **performance** in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.

Another way to look at performance vs scalability:

- If you have a **performance** problem, your system is slow for a single user.
- If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.


## کارایی در برابر مقیاس‌پذیری

یک سرویس مقیاس‌پذیر (scalable) است که  افزایش منابع اضافه شده منجر به افزایش **کارایی برنامه** شده باشد. به طور کلی، افزایش کارایی به معنای ارائه واحدهای کاری و عملیاتی بیشتر است، اما همچنین می‌تواند به معنای مدیریت واحدهای کاری و عملیاتی بزرگتر، مانند زمانی که مجموعه داده‌ها افزایش می‌یابد، باشد.

یک راه دیگر برای نگاه کردن به عملکرد در برابر مقیاس پذیری:

- اگر مشکل **عملکرد** دارید، سیستم شما برای یک کاربر کند است.
- اگر مشکل **مقیاس پذیری** دارید، سیستم شما برای یک کاربر سریع است اما تحت بار سنگین کند است.



--------------------

# Ambassador

Create helper services that send network requests on behalf of a consumer service or application. An ambassador service can be thought of as an out-of-process proxy that is co-located with the client.

This pattern can be useful for offloading common client connectivity tasks such as monitoring, logging, routing, security (such as TLS), and resiliency patterns in a language agnostic way. It is often used with legacy applications, or other applications that are difficult to modify, in order to extend their networking capabilities. It can also enable a specialized team to implement those features.

-----



سرویس‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌های واسطه، سرویس‌های کمکی‌ای(helper services) هستند که به جای یک سرویس مصرف‌کننده یا برنامه، درخواست‌های شبکه را ارسال می‌کنند. این سرویس را می‌توان به عنوان یک پروکسی خارج از فرایند در نظر گرفت که در کنار سرویس گیرنده (کلاینت) قرار دارد.

این الگو برای برون‌سپاری کارهای مشترک ارتباط با کلاینت، مانند مانیتورینگ، لاگ‌گیری، مسیریابی، امنیت (مانند TLS) و الگوهای مقاومت به خطا به روشی مستقل از زبان برنامه‌نویسی، مناسب است. 

از این الگو اغلب برای برنامه‌های  قدیمی یا سایر برنامه‌هایی که تغییر آن‌ها دشوار است، به منظور گسترش قابلیت‌های شبکه‌ای آن‌ها استفاده می‌شود. همچنین می‌تواند به یک تیم متخصص امکان پیاده‌سازی این ویژگی‌ها را بدهد.


# Anti-corruption Layer

Implement a facade or adapter layer between different subsystems that don’t share the same semantics. This layer translates requests that one subsystem makes to the other subsystem. Use this pattern to ensure that an application’s design is not limited by dependencies on outside subsystems. This pattern was first described by Eric Evans in Domain-Driven Design.


این سناریو نیازمند پیاده‌سازی یک لایه نمایشی (Facade) یا آداپتور بین دو زیرسامانه با معنای متفاوت است. این لایه واسط، درخواست‌های ارسالی از یک زیرسامانه به زیرسامانه دیگر را ترجمه می‌کند. با استفاده از این الگو، اطمینان حاصل می‌شود که طراحی یک برنامه توسط وابستگی به زیرسامانه‌های خارجی محدود نمی‌شود. این الگو برای اولین بار توسط اریک ایوانز در کتاب "طراحی هدایت شده توسط دامنه" (Domain-Driven Design) توصیف شده است.

---------------

# Backends for Frontend

Create separate backend services to be consumed by specific frontend applications or interfaces. This pattern is useful when you want to avoid customizing a single backend for multiple interfaces. This pattern was first described by Sam Newman.

الگوی سرویس‌های اختصاصی برای فرانت‌اند (BFF) به این معناست که برای هر برنامه یا رابط کاربری فرانت‌اند، یک سرویس بک‌اند جداگانه ایجاد شود. این الگو زمانی مفید است که نمی‌خواهید یک بک‌اند واحد را برای چندین رابط کاربری مختلف سفارشی کنید.

------------------

# CQRS

CQRS stands for Command and Query Responsibility Segregation, a pattern that separates read and update operations for a data store. Implementing CQRS in your application can maximize its performance, scalability, and security. The flexibility created by migrating to CQRS allows a system to better evolve over time and prevents update commands from causing merge conflicts at the domain level.


CQRS مخفف عبارت **Command Query Responsibility Segregation** (جداسازی مسئولیت فرمان و پرس و جو) است، الگویی که عملیات خواندن و به‌روزرسانی را برای یک ذخیره‌گاه داده جدا می‌کند. پیاده‌سازی CQRS در برنامه شما می‌تواند عملکرد، قابلیت مقیاس‌پذیری و امنیت آن را به طور قابل توجه‌ای افزایش دهد.


--------------------------------

# Compute Resource Consolidation

Consolidate multiple tasks or operations into a single computational unit. This can increase compute resource utilization, and reduce the costs and management overhead associated with performing compute processing in cloud-hosted applications.


## تجميع منابع محاسباتی (Compute Resource Consolidation)

تجميع منابع محاسباتی به معنای **ادغام چندین کار یا عملیات در یک واحد محاسباتی واحد** است. این کار می‌تواند منجر به افزایش **بهره‌وری منابع محاسباتی** و در نتیجه **کاهش هزینه‌ها و سربار مدیریت** مرتبط با انجام پردازش‌های محاسباتی در برنامه‌های کاربردی میزبانی‌شده در ابر شود.

--------------

# External Configuration Store

Move configuration information out of the application deployment package to a centralized location. This can provide opportunities for easier management and control of configuration data, and for sharing configuration data across applications and application instances.

## ذخیره‌گاه پیکربندی خارجی (External Configuration Store)

ذخیره‌گاه پیکربندی خارجی به این معناست که **اطلاعات پیکربندی به جای جایگذاری در بسته استقرار برنامه، در یک مکان مرکزی نگهداری شود**. این کار مزایای متعددی را به همراه دارد:

--------------------


# Gateway Aggregation

Use a gateway to aggregate multiple individual requests into a single request. This pattern is useful when a client must make multiple calls to different backend systems to perform an operation.

## تجميع درخواست با دروازه (Gateway Aggregation)

تجميع درخواست با دروازه (Gateway Aggregation) الگویی است که در معماری میکروسرویس‌ها کاربرد دارد. در این الگو، یک **سرویس دروازه** به عنوان واسطه بین **سرویس گیرنده (کلاینت)** و **سرویس‌های پشتیبان (بک‌اند)** عمل می‌کند. دروازه وظیفه دارد چندین درخواست مجزا از سرویس گیرنده را به درخواست‌های واحد برای سرویس‌های بک‌اند مربوطه تبدیل کند و نتایج آن‌ها را جمع‌آوری و به کلاینت بازگرداند.

---------------


# Gateway Routing

Route requests to multiple services or multiple service instances using a single endpoint. The pattern is useful when you want to:

- Expose multiple services on a single endpoint and route to the appropriate service based on the request

- Expose multiple instances of the same service on a single endpoint for load balancing or availability purposes

- Expose differing versions of the same service on a single endpoint and route traffic across the different versions

## مسیریابی دروازه (Gateway Routing)

مسیریابی دروازه به شما این امکان را می‌دهد تا درخواست‌ها را با استفاده از یک نقطه ورود (endpoint) واحد، به سرویس‌های مختلف یا نمونه‌های متعدد از یک سرویس هدایت کنید. این الگو در سناریوهای زیر کاربرد مفیدی دارد:

- **قرار دادن چندین سرویس پشت یک نقطه ورود واحد:** با این الگو می‌توانید چندین سرویس مجزا را در معرض دید قرار دهید، در حالی که تنها با یک آدرس و پورت واحد سروکار دارید. دروازه بر اساس اطلاعات موجود در درخواست (مانند مسیر URL، هدر درخواست و غیره) تصمیم می‌گیرد که درخواست را به کدام سرویس ارسال کند.
- **توزیع بار بین نمونه‌های سرویس:** اگر از چندین نمونه از یک سرویس برای افزایش قابلیت تحمل خطا (fault tolerance) و توزیع بار استفاده می‌کنید، مسیریابی دروازه می‌تواند درخواست‌ها را به صورت چرخشی (round-robin) یا بر اساس الگوریتم‌های دیگر بین این نمونه‌ها توزیع کند.
- **مدیریت نسخه‌های سرویس:** در صورتی که سرویس شما چندین نسخه با قابلیت‌های مختلف دارد، می‌توانید از مسیریابی دروازه برای مسیریابی درخواست‌ها به نسخه‌های مختلف بر اساس نیازهای کاربر استفاده کنید.



--------------

# Leader Election

Coordinate the actions performed by a collection of collaborating instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the others. This can help to ensure that instances don’t conflict with each other, cause contention for shared resources, or inadvertently interfere with the work that other instances are performing.

در یک برنامه توزیع شده، با انتخاب یک نمونه به عنوان رهبر که مسئولیت مدیریت سایر نمونه‌ها را بر عهده می‌گیرد، اقدامات انجام شده توسط مجموعه ای از نمونه‌های همکاری کننده را هماهنگ و کنترل کنید. این کار می‌تواند به اطمینان از عدم تداخل نمونه‌ها با یکدیگر، ایجاد رقابت نمونه‌ها برای استفاده از منابع مشترک یا دخالت ناخواسته در کاری که سایر نمونه‌ها انجام می‌دهند، کمک کند.


# Pipes and Filters

Decompose a task that performs complex processing into a series of separate elements that can be reused. This can improve performance, scalability, and reusability by allowing task elements that perform the processing to be deployed and scaled independently.

یک وظیفه پردازش پیچیده را به مجموعه ای از عناصر جداگانه که قابل استفاده مجدد هستند تجزیه کنید. این کار با اجازه دادن به عناصر وظیفه که پردازش را انجام می‌دهند تا به طور مستقل مستقر و مقیاس‌پذیر شوند، می‌تواند کارایی، قابلیت مقیاس پذیری و قابلیت استفاده مجدد را بهبود بخشد.

# Sidecar

Deploy components of an application into a separate process or container to provide isolation and encapsulation. This pattern can also enable applications to be composed of heterogeneous components and technologies.

This pattern is named Sidecar because it resembles a sidecar attached to a motorcycle. In the pattern, the sidecar is attached to a parent application and provides supporting features for the application. The sidecar also shares the same lifecycle as the parent application, being created and retired alongside the parent. The sidecar pattern is sometimes referred to as the sidekick pattern and is a decomposition pattern.


اجزای یک برنامه را برای ایزوله سازی و کپسوله سازی در یک فرایند یا کانتینر (container) جداگانه مستقر کنید. این الگو همچنین می‌تواند به برنامه‌ها اجازه دهد تا از اجزا و فناوری‌های ناهمگون تشکیل شوند.

این الگو به دلیل شباهت به سایدکار متصل به موتورسیکلت، سایدکار نامیده می‌شود. در این الگو، سایدکار به یک برنامه اصلی متصل است و ویژگی‌های حمایتی را برای برنامه فراهم می‌کند. سایدکار همچنین دارای چرخه عمر مشابه با برنامه اصلی است و در کنار برنامه اصلی ایجاد و حذف می‌شود. الگوی سایدکار گاهی اوقات به عنوان الگوی همکار (sidekick) شناخته می‌شود و یک الگوی از نوع تجزیه (decomposition) است. 


# Static Content Hosting

Deploy static content to a cloud-based storage service that can deliver them directly to the client. This can reduce the need for potentially expensive compute instances.

محتوای ایستا را در یک سرویس ذخیره سازی ابری مستقر کنید که بتواند آنها را به طور مستقیم به کاربر تحویل دهد. این کار می‌تواند نیاز به نمونه‌های محاسباتی بالقوه گران قیمت را کاهش دهد.

# Cache Aside

Load data on demand into a cache from a data store. This can improve performance and also helps to maintain consistency between data held in the cache and data in the underlying data store.

داده‌ها را پس از درخواست و واکشی از یک ذخیره‌گاه داده در یک حافظه کَش بارگذاری کنید. این کار می‌تواند کارایی برنامه را بهبود بخشد و همچنین به حفظ سازگاری (consistency) بین داده‌های موجود در حافظه کَش و داده‌های موجود در ذخیره‌گاه داده اصلی کمک کند. 


# Event Sourcing

Instead of storing just the current state of the data in a domain, use an append-only store to record the full series of actions taken on that data. The store acts as the system of record and can be used to materialize the domain objects. This can simplify tasks in complex domains, by avoiding the need to synchronize the data model and the business domain, while improving performance, scalability, and responsiveness. It can also provide consistency for transactional data, and maintain full audit trails and history that can enable compensating actions.

به جای اینکه فقط وضعیت فعلی داده را در یک حوزه ذخیره کنید، از یک ذخیره‌گاه append-only برای ثبت همه‌ی سری اقداماتی که روی آن داده انجام شده استفاده کنید. این ذخیره‌گاه به عنوان سیستم ثبت عمل می‌کند و می‌تواند برای مادی سازی اشیاء دامنه (materialize the domain objects) استفاده شود. این کار می‌تواند با اجتناب از نیاز به همگام سازی مدل داده و حوزه تجاری، کارایی، قابلیت مقیاس پذیری و پاسخگویی را در حوزه‌های پیچیده ساده کند. همچنین می‌تواند سازگاری داده تراکنشی را فراهم کند و ردیابی و تاریخچه کاملی را برای انجام اقدامات جبرانی حفظ کند.

# Index Table

Create indexes over the fields in data stores that are frequently referenced by queries. This pattern can improve query performance by allowing applications to more quickly locate the data to retrieve from a data store.

برای بهبود عملکرد کوئری، فیلدهای پرکاربرد در ذخیره‌گاه داده را ایندکس کنید. این الگو به برنامه‌ها این امکان را می‌دهد تا داده‌های مورد نظر را سریع‌تر از ذخیره‌گاه داده بازیابی کنند.

# Materialized View

Generate prepopulated views over the data in one or more data stores when the data isn’t ideally formatted for required query operations. This can help support efficient querying and data extraction, and improve application performance.

داده‌های موجود در یک یا چند ذخیره‌گاه داده را به صورت پیش‌ساخته (prepopulated) نمایش دهید (view) به خصوص زمانی که این داده‌ها به طور بهینه برای عملیات پرس و جو (query) فرمت بندی نشده اند. این کار می‌تواند از کوئری کارآمد و استخراج داده پشتیبانی کند و عملکرد برنامه را بهبود بخشد. 


# Sharding

Sharding is a technique used to horizontally partition a large data set across multiple servers, in order to improve the performance, scalability, and availability of a system. This is done by breaking the data set into smaller chunks, called shards, and distributing the shards across multiple servers. Each shard is self-contained and can be managed and scaled independently of the other shards. Sharding can be used in scenarios like scalability, availability, and geo-distribution. Sharding can be implemented using several different algorithms such as range-based sharding, hash-based sharding, and directory-based sharding.

شاردینگ (Shardding) تکنیکی است که برای پارتیشن بندی افقی (تقسیم بندی بر اساس ردیف‌های جدول) یک مجموعه داده بزرگ در چندین سرور استفاده می‌شود تا عملکرد، مقیاس پذیری و در دسترس بودن یک سیستم را بهبود بخشد. این کار با شکستن مجموعه داده به بخش‌های کوچکتر به نام shard و توزیع آنها در چندین سرور انجام می‌شود. هر shard مستقل است و می‌توان آن را به طور مستقل از shardهای دیگر مدیریت و مقیاس بندی کرد. شاردینگ را می‌توان در سناریوهایی مانند مقیاس پذیری، در دسترس بودن و توزیع جغرافیایی (ذخیره داده در سرورهای مختلف مناطق جغرافیایی) استفاده کرد. شاردینگ را می‌توان با استفاده از الگوریتم‌های مختلفی مانند شاردینگ مبتنی بر محدوده (range-based sharding)، شاردینگ مبتنی بر hash و شاردینگ مبتنی بر دایرکتوری اجرا کرد.


# Valet Key

Use a token that provides clients with restricted direct access to a specific resource, in order to offload data transfer from the application. This is particularly useful in applications that use cloud-hosted storage systems or queues, and can minimize cost and maximize scalability and performance.

از یک توکن برای دسترسی مستقیم و محدود به یک منبع خاص برای کلاینت‌ها استفاده کنید تا انتقال داده از برنامه را کاهش دهد. این امر به خصوص در برنامه هایی که از سیستم‌های ذخیره سازی ابری یا صف‌های ابری استفاده می‌کنند مفید است و می‌تواند هزینه را به حداقل برساند و قابلیت مقیاس پذیری و کارایی را به حداکثر برساند. 

# الگوهای پیام‌رسانی

# Asynchronous Request-Reply

Decouple backend processing from a frontend host, where backend processing needs to be asynchronous, but the frontend still needs a clear response.

چندین الگو برای جدا کردن بک‌اند از host فرانت‌اند در سناریویی که پردازش بک‌اند ناهمزمان است اما فرانت‌اند همچنان به پاسخ مشخصی نیاز دارد، وجود دارد. 
# Claim Check

Split a large message into a claim check and a payload. Send the claim check to the messaging platform and store the payload to an external service. This pattern allows large messages to be processed, while protecting the message bus and the client from being overwhelmed or slowed down. This pattern also helps to reduce costs, as storage is usually cheaper than resource units used by the messaging platform.

پیام‌های حجیم را به یک رسید (claim check) و محموله (payload) تقسیم کنید. رسید را به پلتفرم پیام رسانی ارسال کرده و محموله را در یک سرویس خارجی ذخیره کنید. این الگو به پردازش پیام‌های حجیم کمک می‌کند و در عین حال از کند شدن یا تحت فشار قرار گرفتن پلتفرم پیام رسانی و  پلتفرم کلاینت جلوگیری می‌کند. همچنین این الگو به کاهش هزینه‌ها کمک می‌کند، زیرا ذخیره سازی معمولا ارزان‌تر از واحدهای منابع استفاده شده توسط پلتفرم پیام رسانی است. 

# Choreography

Have each component of the system participate in the decision-making process about the workflow of a business transaction, instead of relying on a central point of control.

بجای تکیه بر یک نقطه مرکزی کنترل، به جای هر جزء از سیستم در فرآیند تصمیم گیری در مورد گردش کار یک تراکنش تجاری مشارکت دهید.

# Competing Consumers

Enable multiple concurrent consumers to process messages received on the same messaging channel. With multiple concurrent consumers, a system can process multiple messages concurrently to optimize throughput, to improve scalability and availability, and to balance the workload.

چندین مصرف کننده همزمان را فعال کنید تا پیام‌های دریافت شده در یک کانال پیام رسانی را پردازش کنند. با چندین مصرف کننده همزمان، یک سیستم می‌تواند چندین پیام را به طور همزمان پردازش کند تا توان عملیاتی را بهینه کند، مقیاس پذیری(scalability) و در دسترس بودن (availability) را بهبود بخشد و بار کاری را متعادل کند.
# Priority Queue

Prioritize requests sent to services so that requests with a higher priority are received and processed more quickly than those with a lower priority. This pattern is useful in applications that offer different service level guarantees to individual clients.

درخواست‌های ارسال‌شده به سرویس‌ها را می‌توان اولویت‌بندی کرد تا درخواست‌های با اولویت بالاتر سریع‌تر از درخواست‌های با اولویت پایین‌تر دریافت و پردازش شوند. این الگو در برنامه‌ها و اپلیکیشن‌هایی که ضمانت‌های سطح خدمات (SLA) مختلفی را به مشتریان ارائه می‌دهند، مفید است. 

# Publisher Subscriber

Enable an application to announce events to multiple interested consumers asynchronously, without coupling the senders to the receivers.

الگوی انتشار-اشتراک (Pub/Sub) این امکان را برای یک اپلیکیشن فراهم می‌کند تا رویدادها را به صورت غیرهمزمان برای چندین مصرف‌کننده علاقه‌مند اعلام کند، بدون اینکه فرستنده‌ها با گیرنده‌ها وابستگی داشته باشند.

# Queue Based Load Leveling

Use a queue that acts as a buffer between a task and a service it invokes in order to smooth intermittent heavy loads that can cause the service to fail or the task to time out. This can help to minimize the impact of peaks in demand on availability and responsiveness for both the task and the service.

با استفاده از یک صف (queue) به عنوان بافر بین یک وظیفه (task) و سرویسی که فراخوانی می‌کند، می‌توان نوسانات بار سنگین را که می‌تواند باعث خرابی سرویس یا time out تسک(task) شود را تعدیل کرد. این کار به کم کردن تاثیر اوج‌های تقاضا روی هر دو موردِ در دسترس بودن (availability) و پاسخگویی (responsiveness) در حالت  تسک و  سرویس، کمک می‌کند.

# Scheduling Agent Supervisor

Coordinate a set of distributed actions as a single operation. If any of the actions fail, try to handle the failures transparently, or else undo the work that was performed, so the entire operation succeeds or fails as a whole. This can add resiliency to a distributed system, by enabling it to recover and retry actions that fail due to transient exceptions, long-lasting faults, and process failures.

مجموعه‌ای از اقدامات توزیع‌شده را به‌عنوان یک عملیات واحد هماهنگ کنید. در صورت خرابی هر یک از اقدامات، سعی کنید خرابی‌ها را به طور شفاف مدیریت کنید، در غیر این صورت، کار انجام‌شده را لغو کنید تا کل عملیات به طور کلی موفق یا شکست بخورد. این کار با امکان بازیابی و تلاش مجدد برای اقداماتی که به دلیل استثنائات گذرا، خرابی‌های طولانی‌مدت و خرابی‌های فرآیند، می‌تواند به انعطاف‌پذیری یک سیستم توزیع‌شده بیافزاید. 

# Sequential Convoy

Sequential Convoy is a pattern that allows for the execution of a series of tasks, or convoy, in a specific order. This pattern can be used to ensure that a set of dependent tasks are executed in the correct order and to handle errors or failures during the execution of the tasks. It can be used in scenarios like workflow and transaction. It can be implemented using a variety of technologies such as state machines, workflows, and transactions.

مجموعه دنباله‌دار (Sequential Convoy) الگویی است که اجرای مجموعه‌ای از تسک‌ها را به ترتیب خاصی اجازه می‌دهد. این الگو را می‌توان برای اطمینان از اجرای صحیح مجموعه‌ای از وظایف وابسته و رسیدگی به خطاها یا خرابی‌ها در طول اجرای وظایف به کار برد. این الگو در سناریوهایی مانند گردش کار(workflow) و تراکنش (transaction) قابل استفاده است و می‌توان آن را با استفاده از فناوری‌های مختلفی مانند ماشین‌های حالت (state machines)، گردش کار و تراکنش‌ها پیاده‌سازی کرد. 

### 
deployment
###


# Deployment Stamps

The deployment stamp pattern involves provisioning, managing, and monitoring a heterogeneous group of resources to host and operate multiple workloads or tenants. Each individual copy is called a stamp, or sometimes a service unit, scale unit, or cell. In a multi-tenant environment, every stamp or scale unit can serve a predefined number of tenants. Multiple stamps can be deployed to scale the solution almost linearly and serve an increasing number of tenants. This approach can improve the scalability of your solution, allow you to deploy instances across multiple regions, and separate your customer data.


الگوی deployment stamp شامل تهیه، مدیریت و نظارت بر گروهی ناهمگون از منابع برای میزبانی و اجرای چندین بار کاری (workloads) یا مستاجر(tenants) است. هر نسخه جداگانه تمایز، واحد سرویس، واحد مقیاس یا سلول نامیده می‌شود. در یک محیط چند مستاجری، هر تمایز یا واحد مقیاس می‌تواند به تعداد از پیش تعریف شده ای از مستاجران خدمات ارائه دهد. تمایزهای متعدد را می‌توان برای مقیاس بندی تقریباً خطی راه حل و خدمت رسانی به تعداد فزاینده ای از مستاجران مستقر کرد. این رویکرد می‌تواند مقیاس پذیری راه حل شما را بهبود بخشد، امکان استقرار نمونه‌ها را در چندین منطقه فراهم کند و داده‌های مشتری شما را جدا کند.

# Geodes

The Geode pattern involves deploying a collection of backend services into a set of geographical nodes, each of which can service any request for any client in any region. This pattern allows serving requests in an active-active style, improving latency and increasing availability by distributing request processing around the globe.

الگوی ژئود (Geode) شامل استقرار مجموعه ای از سرویس‌های backend در مجموعه ای از گره‌های جغرافیایی است که هر کدام می‌توانند به هر درخواست از هر مشتری در هر منطقه سرویس دهند. این الگو به سرویس دهی درخواست‌ها به صورت فعال-فعال (active-active) اجازه می‌دهد، تأخیر را بهبود می‌بخشد و با توزیع پردازش درخواست در سراسر جهان، در دسترس بودن را افزایش می‌دهد.

# Health Endpoint Monitoring

Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals. This can help to verify that applications and services are performing correctly.

برای اطمینان از عملکرد صحیح برنامه‌ها و سرویس ها، می‌توانید بررسی‌های عملکردی آن را در برنامه‌های که به کمک ابزارهای خارجی می‌توانند از طریق نقاط انتهایی (endpoints) در معرض دید قرار داده و در فواصل منظم زمانی به آنها دسترسی داشته باشند را پیاده سازی کنید.

# Throttling

Control the consumption of resources used by an instance of an application, an individual tenant, or an entire service. This can allow the system to continue to function and meet service level agreements, even when an increase in demand places an extreme load on resources.

مصرف منابع مورد استفاده توسط یک نمونه از یک اپلیکیشن، یک مستاجر (tenant) مستقل یا یک سرویس کامل را کنترل کنید. این می‌تواند به سیستم اجازه دهد تا به عملکرد خود ادامه دهد و توافقات سطح خدمات (SLA) را برآورده کند، حتی زمانی که افزایش تقاضا بار شدیدی را بر منابع وارد می‌کند.

# Bulkhead

The Bulkhead pattern is a type of application design that is tolerant of failure. In a bulkhead architecture, elements of an application are isolated into pools so that if one fails, the others will continue to function. It’s named after the sectioned partitions (bulkheads) of a ship’s hull. If the hull of a ship is compromised, only the damaged section fills with water, which prevents the ship from sinking.

الگوی بخش‌بندی (Bulkhead) نوعی طراحی برای نرم‌افزار است که در برابر خرابی مقاوم است. در معماری بخش‌بندی، اجزای یک برنامه در گروه‌های مجزا قرار می‌گیرند تا در صورت خرابی یکی، بقیه به کار خود ادامه دهند. این الگو برگرفته از بخش‌های جداگانه (بخش‌بندی) بدنه کشتی است. اگر بدنه کشتی آسیب ببیند، تنها بخش آسیب دیده از آب پر می‌شود و این مانع از غرق شدن کشتی می‌شود.

# Circuit Breaker

Handle faults that might take a variable amount of time to recover from, when connecting to a remote service or resource. This can improve the stability and resiliency of an application.

هنگام اتصال به یک سرویس یا منبع ریموت، خطاهایی را که ممکن است زمان متغیری برای بازیابی آنها طول بکشد، مدیریت کنید. این می‌تواند پایداری و انعطاف پذیری یک برنامه را بهبود بخشد.


# Compensating Transaction

Undo the work performed by a series of steps, which together define an eventually consistent operation, if one or more of the steps fail. Operations that follow the eventual consistency model are commonly found in cloud-hosted applications that implement complex business processes and workflows.

اگر یک یا چند مرحله با شکست مواجه شوند، کار انجام شده توسط یک سری مراحل را لغو کنید، که با هم یک عملیات در  eventually consistent  را تعریف می‌کنند. عملیات‌هایی که از مدل  eventually consistent  پیروی می‌کنند معمولاً در برنامه‌های میزبانی ابری یافت می‌شوند که فرآیندها و گردش‌های کاری پیچیده تجاری را پیاده‌سازی می‌کنند.

لغو کردن کار انجام شده توسط یک سری از مراحل، که در مجموع یک عملیات با  eventually consistent  را تعریف می‌کنند، در صورتی که یک یا چند مرحله با شکست مواجه شوند. عملیات‌هایی که از مدل  eventually consistent  پیروی می‌کنند، معمولاً در برنامه‌ها و اپلیکیشن‌های ابری که فرآیندهای تجاری و گردش کار پیچیده را اجرا می‌کنند، یافت می‌شوند.

# Leader Election

Coordinate the actions performed by a collection of collaborating instances in a distributed application by electing one instance as the leader that assumes responsibility for managing the others. This can help to ensure that instances don’t conflict with each other, cause contention for shared resources, or inadvertently interfere with the work that other instances are performing.

برای هماهنگ سازی فعالیت‌های مجموعه ای از نمونه‌های همکاری کننده در یک برنامه توزیع شده، می‌توان از الگوی رهبر-پیرو (Leader-Follower) استفاده کرد. در این الگو، یک نمونه به عنوان رهبر انتخاب می‌شود و مسئولیت مدیریت سایر نمونه‌ها (پیروها) را بر عهده می‌گیرد. این روش به جلوگیری از تداخل نمونه‌ها با یکدیگر، رقابت برای منابع مشترک و اختلال ناخواسته در کار سایر نمونه‌ها کمک می‌کند.

# Retry

Enable an application to handle transient failures when it tries to connect to a service or network resource, by transparently retrying a failed operation. This can improve the stability of the application.

الگوی تلاش مجدد (Retry Pattern) به برنامه این امکان را می‌دهد تا در صورت برقراری ارتباط با یک سرویس یا منبع شبکه با خرابی‌های گذرا برخورد کند، با تلاش مجدد شفاف برای عملیات با شکست، ثبات برنامه را بهبود بخشد.



# Federated Identity pattern

Delegate authentication to an external identity provider. This can simplify development, minimize the requirement for user administration, and improve the user experience of the application.

احراز هویت را به یک ارائه دهنده هویت خارجی واگذار کنید. این می‌تواند توسعه را ساده کند، نیاز به مدیریت کاربر را به حداقل برساند و تجربه کاربری برنامه را بهبود بخشد.



# Gatekeeper

Protect applications and services using a dedicated host instance that acts as a broker between clients and the application or service, validates and sanitizes requests, and passes requests and data between them. This can provide an additional layer of security and limit the system’s attack surface.


برای محافظت از برنامه‌ها و سرویس‌ها می‌توانید از یک نمونه میزبان اختصاصی به عنوان واسطه بین مشتریان و برنامه یا سرویس استفاده کنید، درخواست‌ها را اعتبارسنجی و پاکسازی می‌کند و درخواست‌ها و داده‌ها را بین آنها منتقل می‌کند. این می‌تواند یک لایه امنیتی اضافی ایجاد کند و سطح حمله سیستم را محدود کند.



------------------------
**********************

# Performance Antipatterns

Performance antipatterns in system design refer to common mistakes or suboptimal practices that can lead to poor performance in a system. These patterns can occur at different levels of the system and can be caused by a variety of factors such as poor design, lack of optimization, or lack of understanding of the workload.

Some of the examples of performance antipatterns include:

- **N+1 queries:** This occurs when a system makes multiple queries to a database to retrieve related data, instead of using a single query to retrieve all the necessary data.
- **Chatty interfaces:** This occurs when a system makes too many small and frequent requests to an external service or API, instead of making fewer, larger requests.
- **Unbounded data:** This occurs when a system retrieves or processes more data than is necessary for the task at hand, leading to increased resource usage and reduced performance.
- **Inefficient algorithms:** This occurs when a system uses an algorithm that is not well suited to the task at hand, leading to increased resource usage and reduced performance.

## ضدالگوهای عملکرد (Performance Antipatterns)

ضدالگوهای عملکرد در طراحی سیستم به اشتباهات رایج یا شیوه‌های غیراصولی اشاره دارند که می‌توانند منجر به عملکرد ضعیف در یک سیستم شوند. این الگوها می‌توانند در سطوح مختلف سیستم رخ دهند و ناشی از عوامل متعددی مانند طراحی ضعیف، نبود بهینه‌سازی یا درک ناکافی از حجم کاری باشند.

برخی از نمونه‌های ضدالگوهای عملکرد عبارتند از:

* **کوئر‌های N+1**: این اتفاق زمانی می‌افتد که یک سیستم برای بازیابی داده‌های مرتبط، چندین کوئری به پایگاه داده ارسال می‌کند، به جای اینکه از یک کوئری واحد برای بازیابی تمام داده‌های مورد نیاز استفاده کند.

* **رابط‌های پرحرف (Chatty interfaces)**: این اتفاق زمانی می‌افتد که یک سیستم درخواست‌های کوچک و مکرر زیادی به یک سرویس یا API خارجی ارسال کند، به جای اینکه درخواست‌های کمتر و بزرگ‌تر ارسال کند.

* **داده‌های نامحدود**: این اتفاق زمانی می‌افتد که یک سیستم، داده‌های بیشتری نسبت به آنچه برای کار مورد نظر لازم است، بازیابی یا پردازش کند که منجر به افزایش استفاده از منابع و کاهش عملکرد می‌شود.

* **الگوریتم‌های ناکارآمد**: این اتفاق زمانی می‌افتد که یک سیستم از الگوریتمی استفاده کند که برای کار مورد نظر مناسب نباشد و منجر به افزایش استفاده از منابع و کاهش عملکرد شود.

--------------------

# Busy Database

A busy database in system design refers to a database that is handling a high volume of requests or transactions, this can occur when a system is experiencing high traffic or when a database is not properly optimized for the workload it is handling. This can lead to Performance degradation, Increased resource utilization, Deadlocks and contention, Data inconsistencies. To address a busy database, a number of approaches can be taken such as Scaling out, Optimizing the schema, Caching, and Indexing.

## پایگاه داده شلوغ (Busy Database)


پایگاه داده شلوغ در طراحی سیستم به پایگاه داده ای اطلاق می‌شود که حجم بالایی از درخواست‌ها یا تراکنش‌ها را مدیریت می‌کند، این می‌تواند زمانی اتفاق بیفتد که یک سیستم ترافیک بالایی را تجربه می‌کند یا زمانی که پایگاه داده به درستی برای حجم کاری که مدیریت می‌کند بهینه سازی نشده باشد. این می‌تواند منجر به کاهش عملکرد، افزایش استفاده از منابع، بن بست و مشاجره، ناسازگاری داده‌ها شود. برای پرداختن به یک پایگاه داده شلوغ، تعدادی از رویکردها مانند Scaling out، Optimizing the schema، Caching و Indexing را می‌توان در نظر گرفت.

---------------

# Busy Frontend

Performing asynchronous work on a large number of background threads can starve other concurrent foreground tasks of resources, decreasing response times to unacceptable levels.

Resource-intensive tasks can increase the response times for user requests and cause high latency. One way to improve response times is to offload a resource-intensive task to a separate thread. This approach lets the application stay responsive while processing happens in the background. However, tasks that run on a background thread still consume resources. If there are too many of them, they can starve the threads that are handling requests.

This problem typically occurs when an application is developed as monolithic piece of code, with all of the business logic combined into a single tier shared with the presentation layer.



## فرونت-اند شلوغ (Busy Frontend)

انجام کارهای ناهمزمان (asynchronous) روی تعداد زیادی از رشته‌های پس‌زمینه (background threads) می‌تواند منجر به کمبود منابع برای سایر وظایف همزمان در پیش‌زمینه (foreground tasks) شود و در نتیجه زمان پاسخگویی را به سطوح غیرقابل قبولی کاهش دهد.

وظایف با مصرف منابع بالا می‌توانند زمان پاسخگویی به درخواست‌های کاربر را افزایش دهند و باعث تأخیر زیاد (high latency) شوند. یک راه برای بهبود زمان پاسخگویی، انتقال یک کار با مصرف منابع بالا به یک رشته جداگانه است. این رویکرد به برنامه اجازه می‌دهد در حالی که پردازش در پس‌زمینه اتفاق می‌افتد، همچنان پاسخگو باقی بماند. با این حال، کارهایی که روی یک رشته پس‌زمینه اجرا می‌شوند همچنان منابع را مصرف می‌کنند. اگر تعداد آن‌ها زیاد باشد، می‌توانند باعث کمبود منابع برای رشته‌هایی شوند که درخواست‌ها را مدیریت می‌کنند.

این مشکل معمولاً زمانی رخ می‌دهد که یک برنامه به عنوان یک قطعه کد یکپارچه (monolithic) توسعه داده شود، به طوری که تمام منطق تجاری در یک لایه واحد با لایه نمایش (presentation layer) ترکیب شود.


------------

# Chatty I/O

The cumulative effect of a large number of I/O requests can have a significant impact on performance and responsiveness.

Network calls and other I/O operations are inherently slow compared to compute tasks. Each I/O request typically has significant overhead, and the cumulative effect of numerous I/O operations can slow down the system. Here are some common causes of chatty I/O.

- Reading and writing individual records to a database as distinct requests
- Implementing a single logical operation as a series of HTTP requests
- Reading and writing to a file on disk

## I/O پرحرف (Chatty I/O)

  تاثیر تجمعی تعداد زیادی از درخواست‌های I/O می‌تواند تأثیر قابل توجهی بر عملکرد و پاسخگویی سیستم داشته باشد.

   در مقایسه با وظایف محاسباتی، فراخوانی‌های شبکه و سایر عملیات I/O ذاتاً کند هستند. هر درخواست I/O معمولاً دارای سربار قابل توجهی است و اثر تجمعی چندین عملیات I/O می‌تواند سیستم را آهسته کند. در اینجا برخی از دلایل رایج I/O پرحرف آورده شده است:

  * خواندن و نوشتن تک تک رکوردها در پایگاه داده به عنوان درخواست‌های مجزا
  * اجرای یک عملیات منطقی واحد به عنوان یک سری درخواست‌های HTTP
  * خواندن و نوشتن روی یک فایل روی دیسک

------------------------------------
# Extraneous Fetching

Extraneous fetching in system design refers to the practice of retrieving more data than is needed for a specific task or operation. This can occur when a system is not optimized for the specific workload or when the system is not properly designed to handle the data requirements.

Extraneous fetching can lead to a number of issues, such as:

- Performance degradation
- Increased resource utilization
- Increased network traffic
- Poor user experience


## فراخوانی اضافی (Extraneous Fetching)

فراخوانی اضافی در طراحی سیستم به رویه دریافت داده بیشتر از مقدار مورد نیاز برای یک کار یا عملیات خاص اشاره دارد. این می‌تواند زمانی رخ دهد که یک سیستم برای حجم کاری خاص بهینه سازی نشده باشد یا زمانی که سیستم برای مدیریت نیازمندی‌های داده به درستی طراحی نشده باشد.

فراخوانی اضافی می‌تواند منجر به مشکلات زیر شود:

* **افت عملکرد (Performance degradation)**: افزایش زمان پاسخگویی به دلیل بازیابی و پردازش داده‌های غیرضروری
* **افزایش استفاده از منابع (Increased resource utilization)**: مصرف بالای منابع پردازشی و حافظه به دلیل پردازش داده‌های اضافی
* **افزایش ترافیک شبکه (Increased network traffic):** انتقال غیر ضروری حجم زیادی از داده‌ها روی شبکه
* **تجربه کاربری ضعیف (Poor user experience):‏** کندی و تأخیر در عملکرد برنامه به دلیل بار اضافی ناشی از فراخوانی‌های اضافی


----------------------

# Improper Instantiation

Improper instantiation in system design refers to the practice of creating unnecessary instances of an object, class or service, which can lead to performance and scalability issues. This can happen when the system is not properly designed, when the code is not written in an efficient way, or when the code is not optimized for the specific use case.

## ایجاد غلط اشیاء (Improper Instantiation)

ایجاد غلط اشیاء در طراحی سیستم به رویه ایجاد نمونه‌های غیرضروری از یک شیء، کلاس یا سرویس اشاره دارد که می‌تواند منجر به مشکلات عملکرد و مقیاس‌پذیری شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی طراحی نشده باشد، کد به روش کارآمد نوشته نشده باشد، یا کد برای سناریوی استفاده خاص بهینه نشده باشد.


--------------


# Monolithic Persistence

Monolithic Persistence refers to the use of a single, monolithic database to store all of the data for an application or system. This approach can be used for simple, small-scale systems but as the system grows and evolves it can become a bottleneck, resulting in poor scalability, limited flexibility, and increased complexity. To address these limitations, a number of approaches can be taken such as Microservices, Sharding, and NoSQL databases.

## پایگاه داده یکپارچه (Monolithic Persistence)

پایگاه داده یکپارچه به روشی گفته می‌شود که در آن از یک پایگاه داده‌ی تنها برای ذخیره‌ی کل اطلاعات یک برنامه یا سیستم استفاده می‌شود. این روش برای سیستم‌های ساده و کوچک کاربرد دارد، اما با رشد و پیچیدگی سیستم، این پایگاه داده می‌تواند به یک گلوگاه تبدیل شود و در نتیجه منجر به مقیاس‌پذیری ضعیف، انعطاف‌پذیری محدود و پیچیدگی بیشتر شود. برای رفع این محدودیت‌ها، می‌توان از روش‌هایی مانند میکروسرویس‌ها، پارتیشن‌بندی (Sharding) و پایگاه‌های داده‌ی NoSQL استفاده کرد.


-------------


# No Caching

No caching antipattern occurs when a cloud application that handles many concurrent requests, repeatedly fetches the same data. This can reduce performance and scalability.

When data is not cached, it can cause a number of undesirable behaviors, including:

- Repeatedly fetching the same information from a resource that is expensive to access, in terms of I/O overhead or latency.
- Repeatedly constructing the same objects or data structures for multiple requests.
- Making excessive calls to a remote service that has a service quota and throttles clients past a certain limit.

In turn, these problems can lead to poor response times, increased contention in the data store, and poor scalability.


## بدون کش (No Caching)

ضد الگوی بدون کش زمانی رخ می‌دهد که یک برنامه ابری که با حجم زیادی از درخواست‌های همزمان سروکار دارد، به طور مکرر داده‌های یکسانی را بازیابی می‌کند. این کار باعث کاهش عملکرد و مقیاس‌پذیری می‌شود.

زمانی که داده‌ها در حافظه کش ذخیره نمی‌شوند، می‌تواند منجر به رفتارهای نامطلوبی شود، از جمله:

* بازیابی مکرر اطلاعات یکسان از منبعی که دسترسی به آن پرهزینه است، مانند سربار I/O یا تأخیر بالا.
* ساختن مکرر اشیاء یا ساختارهای داده‌ای یکسان برای درخواست‌های متعدد.
* برقراری تماس‌های بیش از حد با یک سرویس از راه دور که دارای سهمیه‌ی سرویس‌دهی است و فراتر از محدودیت مشخص‌شده، سرعت سرویس‌دهی را کاهش می‌دهد (throttles clients).

این مشکلات در نهایت می‌توانند منجر به زمان پاسخگویی ضعیف، افزایش رقابت در پایگاه داده و مقیاس‌پذیری پایین شوند.



-------------------------

# Noisy Neighbor

Noisy neighbor refers to a situation in which one or more components of a system are utilizing a disproportionate amount of shared resources, leading to resource contention and reduced performance for other components. This can occur when a system is not properly designed or configured to handle the workload, or when a component is behaving unexpectedly.

Examples of noisy neighbor scenarios include:

- One user on a shared server utilizing a large amount of CPU or memory, leading to reduced performance for other users on the same server.
- One process on a shared server utilizing a large amount of I/O, causing other processes to experience slow I/O and increased latency.
- One application consuming a large amount of network bandwidth, causing other applications to experience reduced throughput.




## همسایه پر سر و صدا (Noisy Neighbor)

همسایه پر سر و صدا به وضعیتی گفته می‌شود که در آن یکی از اجزای سیستم یا چندتای آنها، از منابع مشترک به میزان نامتناسبی استفاده می‌کنند و این باعث درگیری بر سر منابع و در نهایت کاهش عملکرد سایر اجزا می‌شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی برای مدیریت حجم کاری طراحی یا پیکربندی نشده باشد، یا زمانی که یک جزء رفتاری غیرمنتظره داشته باشد.

نمونه‌هایی از سناریوهای همسایه پر سر و صدا عبارتند از:

- یک کاربر روی یک سرور مشترک که از مقدار زیادی CPU یا حافظه استفاده می‌کند و در نتیجه عملکرد سایر کاربران روی همان سرور را کاهش می‌دهد.
- یک فرایند روی یک سرور مشترک که از مقدار زیادی I/O استفاده می‌کند و باعث می‌شود سایر فرایندها با I/O کند و تأخیر زیاد مواجه شوند.
- یک برنامه که مقدار زیادی از پهنای باند شبکه را مصرف می‌کند و در نتیجه باعث کاهش توان عملیاتی (throughput) سایر برنامه‌ها می‌شود.

--------------


# Retry Storm

Retry Storm refers to a situation in which a large number of retries are triggered in a short period of time, leading to a significant increase in traffic and resource usage. This can occur when a system is not properly designed to handle failures or when a component is behaving unexpectedly. This can lead to Performance degradation, Increased resource utilization, Increased network traffic, and Poor user experience. To address retry storms, a number of approaches can be taken such as Exponential backoff, Circuit breaking, and Monitoring and alerting.

To learn more, visit the following links:


## طوفان بازپخش (Retry Storm)

طوفان بازپخش به وضعیتی گفته می‌شود که در آن تعداد زیادی از تلاش‌های مجدد (retry) در مدت زمان کوتاهی راه‌اندازی می‌شوند و این امر منجر به افزایش قابل توجه ترافیک و استفاده از منابع می‌شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی برای مدیریت خطاها طراحی نشده باشد یا زمانی که یک جزء رفتاری غیرمنتظره داشته باشد. این وضعیت می‌تواند منجر به موارد زیر شود:

- **افت عملکرد (Performance degradation):** کند شدن سیستم به دلیل حجم بالای درخواست‌های بازپخش
- **افزایش استفاده از منابع (Increased resource utilization):** مصرف بالای منابع پردازشی و حافظه به دلیل پردازش درخواست‌های تکراری
- **افزایش ترافیک شبکه (Increased network traffic):** ایجاد بار اضافی روی شبکه به علت ارسال مجدد حجم زیادی از درخواست‌ها
- **تجربه کاربری ضعیف (Poor user experience):‏** کندی و تأخیر در عملکرد برنامه به دلیل بار اضافی ناشی از طوفان بازپخش

برای مقابله با طوفان بازپخش، می‌توان از روش‌هایی مانند تأخیر تصاعدی (Exponential backoff)، قطع‌کنندگی مدار (Circuit breaking) و نظارت و هشدار (Monitoring and alerting) استفاده کرد.


----------------


# Synchronous I/O

Blocking the calling thread while I/O completes can reduce performance and affect vertical scalability.

A synchronous I/O operation blocks the calling thread while the I/O completes. The calling thread enters a wait state and is unable to perform useful work during this interval, wasting processing resources.

Common examples of I/O include:

- Retrieving or persisting data to a database or any type of persistent storage.
- Sending a request to a web service.
- Posting a message or retrieving a message from a queue.
- Writing to or reading from a local file.

This antipattern typically occurs because:

- It appears to be the most intuitive way to perform an operation.
    
- The application requires a response from a request.
    
- The application uses a library that only provides synchronous methods for I/O.
    
- An external library performs synchronous I/O operations internally. A single synchronous I/O call can block an entire call chain.

## I/O همزمان (Synchronous I/O)

در I/O همزمان، تا زمانی که عملیات ورودی/خروجی تکمیل شود، رشته (thread) فراخواننده بلاک می‌شود. این موضوع می‌تواند باعث کاهش عملکرد و تحت تاثیر قرار دادن مقیاس‌پذیری عمودی (vertical scalability) شود.

در یک عملیات I/O همزمان، رشته فراخواننده تا زمان تکمیل I/O، بلوکه شده و در وضعیت انتظار قرار می‌گیرد. در این مدت رشته نمی‌تواند کار مفیدی انجام دهد و در نتیجه منابع پردازشی هدر می‌رود. 

نمونه‌های رایج I/O عبارتند از:

* بازیابی یا ذخیره‌سازی داده‌ها در پایگاه داده یا هر نوع حافظه دائمی.
* ارسال درخواست به یک سرویس وب.
* ارسال یا دریافت پیام از صف پیام.
* نوشتن یا خواندن از یک فایل محلی.

این ضدالگو معمولا به دلایل زیر رخ می‌دهد:

* به نظر می‌رسد این روش، بدیهی‌ترین راه برای انجام یک عملیات است.
* برنامه نیازمند پاسخی برای درخواستی که ارسال کرده است.
* برنامه از کتابخانه‌ای استفاده می‌کند که فقط متدهای همزمان برای I/O ارائه می‌دهد.
* یک کتابخانه خارجی در درون خود عملیات I/O همزمان انجام می‌دهد. تنها یک فراخوانی I/O همزمان می‌تواند کل زنجیره فراخوانی را بلوکه کند.


---------------------
***************************

# Databases

Picking the right database for a system is an important decision, as it can have a significant impact on the performance, scalability, and overall success of the system. Some of the key reasons why it’s important to pick the right database include:

- Performance: Different databases have different performance characteristics, and choosing the wrong one can lead to poor performance and slow response times.
- Scalability: As the system grows and the volume of data increases, the database needs to be able to scale accordingly. Some databases are better suited for handling large amounts of data than others.
- Data Modeling: Different databases have different data modeling capabilities and choosing the right one can help to keep the data consistent and organized.
- Data Integrity: Different databases have different capabilities for maintaining data integrity, such as enforcing constraints, and can have different levels of data security.
- Support and maintenance: Some databases have more active communities and better documentation, making it easier to find help and resources.

Overall, by choosing the right database, you can ensure that your system will perform well, scale as needed, and be maintainable in the long run.


پایگاه‌داده‌ها

انتخاب پایگاه‌داده مناسب برای یک سیستم، یک تصمیم حیاتی است، زیرا می‌تواند تأثیر قابل توجهی بر عملکرد، مقیاس‌پذیری و موفقیت کلی سیستم داشته باشد. برخی از دلایل کلیدی که چرا انتخاب پایگاه‌داده مناسب مهم است عبارتند از:

* **عملکرد (Performance):** پایگاه‌داده‌های مختلف، ویژگی‌های عملکردی متفاوتی دارند و انتخاب اشتباه می‌تواند منجر به عملکرد ضعیف و زمان پاسخگویی کند شود.
* **مقیاس‌پذیری (Scalability):** با رشد سیستم و افزایش حجم داده‌ها، پایگاه‌داده باید بتواند به طور مناسب مقیاس‌بندی شود. برخی از پایگاه‌داده‌ها برای مدیریت حجم زیادی از داده‌ها نسبت به سایرین مناسب‌تر هستند.
* **مدل‌سازی داده (Data Modeling):** پایگاه‌داده‌های مختلف قابلیت‌های مدل‌سازی داده متفاوتی دارند و انتخاب گزینه مناسب می‌تواند به حفظ انسجام و سازماندهی داده‌ها کمک کند.
* **یکپارچگی داده (Data Integrity):** پایگاه‌داده‌های مختلف قابلیت‌های متفاوتی برای حفظ یکپارچگی داده، مانند اعمال محدودیت‌ها دارند و می‌توانند سطوح مختلفی از امنیت داده را ارائه دهند.
* **پشتیبانی و نگهداری (Support and maintenance):** برخی از پایگاه‌داده‌ها جوامع فعال‌تر و مستندات بهتری دارند و یافتن کمک و منابع را آسان‌تر می‌کنند.

به طور کلی، با انتخاب پایگاه‌داده مناسب، می‌توانید اطمینان حاصل کنید که سیستم شما عملکرد خوبی خواهد داشت، در صورت نیاز مقیاس‌بندی می‌شود و در بلندمدت قابل نگهداری است.




---------------------

# SQL vs noSQL

SQL databases, such as MySQL and PostgreSQL, are best suited for structured, relational data and use a fixed schema. They provide robust ACID (Atomicity, Consistency, Isolation, Durability) transactions and support complex queries and joins.

NoSQL databases, such as MongoDB and Cassandra, are best suited for unstructured, non-relational data and use a flexible schema. They provide high scalability and performance for large amounts of data and are often used in big data and real-time web applications.

The choice between SQL and NoSQL depends on the specific use case and requirements of the project. If you need to store and query structured data with complex relationships, an SQL database is likely a better choice. If you need to store and query large amounts of unstructured data with high scalability and performance, a NoSQL database may be a better choice.



پایگاه‌داده‌های SQL مثل MySQL و PostgreSQL برای داده‌های ساختاریافته و رابطه‌ای با اسکیمای ثابت مناسب‌تر هستند. این پایگاه‌داده‌ها تراکنش‌های ACID (اتمسیت، سازگاری، جداسازی، دوام) قوی و پشتیبانی از پرس‌وجوهای پیچیده و JOIN را ارائه می‌دهند.

پایگاه‌داده‌های NoSQL مثل MongoDB و Cassandra برای داده‌های غیرساختاریافته و غیررابطه‌ای با اسکیمای انعطاف‌پذیر مناسب‌تر هستند. آن‌ها مقیاس‌پذیری و عملکرد بالایی را برای حجم زیادی از داده ارائه می‌دهند و اغلب در وب‌سایت‌های بلادرنگ و داده‌های حجیم به کار می‌روند.

بهترین انتخاب بین SQL و NoSQL به نیازمندی‌ها و سناریوی خاص پروژه بستگی دارد. اگر نیاز به ذخیره و پرس‌وجوی داده‌های ساختاریافته با روابط پیچیده دارید، پایگاه‌داده SQL گزینه بهتری است. در صورتی که نیاز به ذخیره و پرس‌وجوی حجم زیادی از داده‌های غیرساختاریافته با مقیاس‌پذیری و عملکرد بالا دارید، پایگاه‌داده NoSQL می‌تواند انتخاب مناسب‌تری باشد.


---------------------------

# NoSQL

NoSQL is a collection of data items represented in a key-value store, document store, wide column store, or a graph database. Data is denormalized, and joins are generally done in the application code. Most NoSQL stores lack true ACID transactions and favor eventual consistency.

BASE is often used to describe the properties of NoSQL databases. In comparison with the CAP Theorem, BASE chooses availability over consistency.

- Basically available - the system guarantees availability.
- Soft state - the state of the system may change over time, even without input.
- Eventual consistency - the system will become consistent over a period of time, given that the system doesn’t receive input during that period.


## NoSQL

NoSQL مجموعه‌ای از داده‌ها است که به صورت key-value store، document store، wide column store یا graph database نمایش داده می‌شود. داده‌ها در این نوع پایگاه‌داده غیرعادی‌سازی شده‌اند (denormalized) و اتصالات (join) معمولا در کد برنامه انجام می‌شوند. 

اکثر پایگاه‌داده‌های NoSQL از تراکنش‌های ACID واقعی پشتیبانی نمی‌کنند و قوام نهایی (eventual consistency) را ترجیح می‌دهند.

از واژه BASE (Basically Available, Soft state, Eventual consistency) برای توصیف ویژگی‌های پایگاه‌داده‌های NoSQL استفاده می‌شود. BASE در مقایسه با قضیه CAP، در دسترس‌بودن را بر قوام ترجیح می‌دهد.

* **Basically Available (در دسترس بودن اساسی):** سیستم در دسترس بودن را تضمین می‌کند.
* **Soft state (حالت نرم):** وضعیت سیستم ممکن است حتی بدون ورودی در طول زمان تغییر کند.
* **Eventual consistency (سازگاری احتمالی):** سیستم در نهایت و با گذشت زمان سازگاری و یکپارچگی پیدا می‌کند، به شرطی که در آن مدت ورودی جدیدی به سیستم وارد نشود.



-------------------------

# Document Store

A document store is centered around documents (XML, JSON, binary, etc), where a document stores all information for a given object. Document stores provide APIs or a query language to query based on the internal structure of the document itself. Note, many key-value stores include features for working with a value’s metadata, blurring the lines between these two storage types.

Based on the underlying implementation, documents are organized by collections, tags, metadata, or directories. Although documents can be organized or grouped together, documents may have fields that are completely different from each other.


## پایگاه داده سندگرا (Document Store)

پایگاه داده سندگرا بر محور اسناد (داده‌هایی با فرمت XML، JSON، باینری و غیره) بنا شده است، جایی که هر سند تمام اطلاعات مربوط به یک شیء خاص را ذخیره می‌کند. این نوع پایگاه‌داده‌ها API یا زبانی برای پرس‌وجو/کوئری بر اساس ساختار داخلی خود سند ارائه می‌دهند. توجه داشته باشید که بسیاری از پایگاه‌های داده کلید-مقدار شامل ویژگی‌هایی برای کار با فرا داده‌های (metadata) یک مقدار هستند که خط تمایز بین این دو نوع ذخیره‌سازی را کمرنگ می‌کند.

بر اساس پیاده‌سازی زمینه‌ای، اسناد بر اساس مجموعه‌ها (collections)، تگ‌ها (tags)، فرا داده‌ها (metadata) یا دایرکتوری‌ها سازماندهی می‌شوند. اگرچه اسناد می‌توانند سازماندهی یا گروه بندی شوند، اما ممکن است دارای فیلدهایی باشند که کاملاً با یکدیگر متفاوت هستند.


-----------


# Wide Column Store

A wide column store’s basic unit of data is a column (name/value pair). A column can be grouped in column families (analogous to a SQL table). Super column families further group column families. You can access each column independently with a row key, and columns with the same row key form a row. Each value contains a timestamp for versioning and for conflict resolution.

Google introduced Bigtable as the first wide column store, which influenced the open-source HBase often-used in the Hadoop ecosystem, and Cassandra from Facebook. Stores such as BigTable, HBase, and Cassandra maintain keys in lexicographic order, allowing efficient retrieval of selective key ranges.


## پایگاه داده ستون‌ گسترده (Wide Column Store)

واحد اساسی داده در یک ستون‌ گسترده، یک ستون (جفت نام/مقدار) است. ستون‌ها را می‌توان در خانواده‌های ستونی (مشابه یک جدول SQL) گروه بندی کرد. اَبَر خانواده‌های ستونی، گروهی از خانواده‌های ستونی را سازماندهی می‌کنند. شما می‌توانید به هر ستون به طور مستقل با یک کلید ردیف (row key) دسترسی داشته باشید و ستون‌هایی با کلید ردیف یکسان، یک ردیف را تشکیل می‌دهند. هر مقدار برای کنترل نسخه و حل تضاد، شامل یک برچسب زمانی (timestamp) است.

گوگل با معرفی Bigtable، اولین ستون‌گستر پهن را ارائه کرد که بر HBase متن‌باز، اغلب استفاده‌شده در اکوسیستم Hadoop، و Cassandra از فیس‌بوک تأثیرگذار بود. ذخیره‌سازهایی مانند Bigtable، HBase و Cassandra کلیدها را به ترتیب واژه‌نامه‌ای (lexicographic order) نگهداری می‌کنند که امکان بازیابی کارآمد محدوده‌های کلید انتخابی را فراهم می‌کند.


-----------------------
# RDBMS

A relational database like SQL is a collection of data items organized in tables. ACID is a set of properties of relational database transactions.

- **Atomicity** - Each transaction is all or nothing
- **Consistency** - Any transaction will bring the database from one valid state to another
- **Isolation** - Executing transactions concurrently has the same results as if the transactions were executed serially
- **Durability** - Once a transaction has been committed, it will remain so

There are many techniques to scale a relational database: master-slave replication, master-master replication, federation, sharding, denormalization, and SQL tuning.


## پایگاه داده رابطه‌ای (RDBMS)

پایگاه داده رابطه‌ای، مانند پایگاه‌داده‌های SQL، مجموعه‌ای از داده‌ها است که در جداول سازماندهی شده‌اند. ACID مجموعه‌ای از ویژگی‌های تراکنش‌های پایگاه‌داده رابطه‌ای است.

* **اتمسیت (Atomicity):‏** هر تراکنش به صورت کامل انجام می‌شود یا اصلاً انجام نمی‌شود.
* **سازگاری (Consistency):‏** هر تراکنش پایگاه داده را از یک وضعیت معتبر به وضعیت معتبر دیگری منتقل می‌کند.
* **جداسازی (Isolation):‏** اجرای همزمان تراکنش‌ها نتایجی مشابه با اجرای سریال تراکنش‌ها به دست می‌دهد.
* **دوام (Durability):‏** پس از اینکه یک تراکنش کامیت شد (committed)، برای همیشه پایدار باقی می‌ماند.

  برای مقیاس‌بندی یک پایگاه داده رابطه‌ای تکنیک‌های زیادی وجود دارد، از جمله: تکثیر اصلی-فرعی (master-slave replication)، تکثیر اصلی-اصلی (master-master replication)، فدراسیون (federation)، پارتیشن‌بندی (sharding)، غیرعادی‌سازی (denormalization) و تنظیم SQL (SQL tuning).

---------------------

# Replication

Replication is the process of copying data from one database to another. Replication is used to increase availability and scalability of databases. There are two types of replication: master-slave and master-master.

## Master-slave Replication:

The master serves reads and writes, replicating writes to one or more slaves, which serve only reads. Slaves can also replicate to additional slaves in a tree-like fashion. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a master or a new master is provisioned.

## Master-master Replication:

Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.


## تکثیر (Replication)

تکثیر فرآیند کپی کردن داده از یک پایگاه داده به پایگاه داده دیگر است. از تکثیر برای افزایش در دسترس‌بودن و مقیاس‌پذیری پایگاه‌های داده استفاده می‌شود. دو نوع اصلی تکثیر وجود دارد: اصلی-فرعی (master-slave) و اصلی-اصلی (master-master).

###  تکثیر اصلی-فرعی (Master-slave Replication)

در این نوع تکثیر، پایگاه داده اصلی (master) مسئولیت عملیات خواندن و نوشتن را بر عهده دارد و به‌صورت همزمان، عملیات نوشتن را بر روی یک یا چند پایگاه داده فرعی (slave) تکرار می‌کند. پایگاه‌های داده فرعی تنها قادر به خواندن داده‌ها هستند. این پایگاه‌های فرعی همچنین می‌توانند داده‌ها را بر روی سایر پایگاه‌های داده فرعی دیگر تکثیر کنند که این کار به شکل درختی انجام می‌شود. اگر پایگاه داده اصلی از دسترس خارج شود، سیستم می‌تواند در حالت فقط خواندن به کار خود ادامه دهد تا زمانی که یکی از پایگاه‌های داده فرعی به عنوان اصلی ارتقا پیدا کند یا یک پایگاه داده اصلی جدید راه‌اندازی شود.

### تکثیر اصلی-اصلی (Master-master Replication)

در تکثیر اصلی-اصلی، هر دو پایگاه داده اصلی می‌توانند عملیات خواندن و نوشتن را انجام دهند و در زمان عملیات نوشتن، با یکدیگر هماهنگ می‌شوند. اگر یکی از پایگاه‌های داده اصلی از کار بیفتد، سیستم می‌تواند همچنان به کار خود ادامه دهد و هر دو عملیات خواندن و نوشتن را انجام دهد.

------------------------

# Federation

Federation (or functional partitioning) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag. Smaller databases result in more data that can fit in memory, which in turn results in more cache hits due to improved cache locality. With no single central master serializing writes you can write in parallel, increasing throughput.

## فدراسیون (Federation)

فدراسیون (یا پارتیشن بندی عملکردی) پایگاه داده‌ها را بر اساس عملکرد آن تقسیم می‌کند. به عنوان مثال، به جای یک پایگاه داده واحد و یکپارچه، می‌توانید سه پایگاه داده داشته باشید: انجمن‌ها، کاربران و محصولات، در نتیجه ترافیک خواندن و نوشتن کمتری برای هر پایگاه داده و در نتیجه تاخیر کمتری در تکرار وجود دارد. پایگاه داده‌های کوچکتر منجر به داده‌های بیشتری می‌شوند که می‌توانند در حافظه قرار بگیرند، که به نوبه خود به دلیل بهبود موقعیت حافظه کَش، بازدیدهای کَش بیشتری را به همراه دارد. بدون نوشتن سریال اصلی مرکزی واحد، می‌توانید به صورت موازی بنویسید و توان عملیاتی را افزایش دهید.

--------------


## Denormalization

Denormalization attempts to improve read performance at the expense of some write performance. Redundant copies of the data are written in multiple tables to avoid expensive joins. Some RDBMS such as PostgreSQL and Oracle support materialized views which handle the work of storing redundant information and keeping redundant copies consistent.

Once data becomes distributed with techniques such as federation and sharding, managing joins across data centers further increases complexity. Denormalization might circumvent the need for such complex joins.


غیرعادی‌سازی (Denormalization) روشی است برای بهبود عملکرد خواندن با فدا کردن بخشی از عملکرد نوشتن. برای اجتناب از اتصالات پرهزینه (joins) در بانک‌های اطلاعاتی رابطه‌ای، کپی‌های اضافی از داده‌ها در جداول چندگانه نوشته می‌شوند. برخی از سیستم‌های مدیریت پایگاه‌داده رابطه‌ای مانند PostgreSQL و Oracle از نماهای مادی (materialized views) پشتیبانی می‌کنند که وظیفه ذخیره اطلاعات اضافی و همگام نگه داشتن کپی‌های اضافی را برعهده دارند.

با توزیع داده‌ها با تکنیک‌هایی مانند فدراسیون و پارتیشن‌بندی (sharding)، مدیریت اتصالات بین مراکز داده (data centers) پیچیدگی بیشتری پیدا می‌کند. غیرعادی‌سازی می‌تواند نیاز به چنین اتصالات پیچیده‌ای را دور بزند.

------------


×××××××××××××××××××××××××

# Asynchronism

Asynchronous workflows help reduce request times for expensive operations that would otherwise be performed in-line. They can also help by doing time-consuming work in advance, such as periodic aggregation of data.

## ناهمزمانی (Asynchronism)

گردش کارهای ناهمزمان (Asynchronism) به کاهش زمان پاسخگویی برای عملیات پرهزینه‌ای که به صورت درون خطی (همزمان) انجام می‌شوند، کمک می‌کنند. همچنین می‌توانند با انجام کارهای زمان بر به صورت  مناسب تر، مانند تجمیع دوره‌ای داده ها، به بهبود عملکرد سیستم کمک کنند.

---------------

# Idempotent Operations

Idempotent operations are operations that can be applied multiple times without changing the result beyond the initial application. In other words, if an operation is idempotent, it will have the same effect whether it is executed once or multiple times.

It is also important to understand the benefits of [idempotent](https://en.wikipedia.org/wiki/Idempotence#Computer_science_meaning) operations, especially when using message or task queues that do not guarantee _exactly once_ processing. Many queueing systems guarantee _at least once_ message delivery or processing. These systems are not completely synchronized, for instance, across geographic regions, which simplifies some aspects of their implemntation or design. Designing the operations that a task queue executes to be idempotent allows one to use a queueing system that has accepted this design trade-off.

## عملیات ایدمپوتنت (Idempotent Operations)

عملیات ایدمپوتنت، عملیاتی هستند که می‌توان آن‌ها را چندین بار اجرا کرد بدون اینکه نتیجه نهایی فراتر از اولین اجرا تغییر کند. به عبارتی دیگر، اگر یک عملیات ایدمپوتنت باشد، فرقی نمی‌کند که یکبار یا چندین بار آن را اجرا کنید، نتیجه نهایی یکسان خواهد بود.

درک مزایای عملیات ایدمپوتنت، به خصوص هنگام استفاده از صف‌های پیام یا task که پردازش دقیقاً یکبار را تضمین نمی‌کنند، بسیار مهم است. بسیاری از سیستم‌های صف، تحویل یا پردازش پیام "حداقل یکبار" را تضمین می‌کنند. این سیستم‌ها به طور کامل همزمان سازی نشده اند، به عنوان مثال در سراسر مناطق جغرافیایی، که برخی از جنبه‌های پیاده سازی یا طراحی آنها را ساده می‌کند. طراحی عملیات اجرا شده توسط یک صف وظیفه برای اینکه ایدمپوتنت باشند، به فرد اجازه می‌دهد تا از سیستم صف بندی استفاده کند که این مصالحه طراحی را پذیرفته است.


----------------

# Back Pressure

If queues start to grow significantly, the queue size can become larger than memory, resulting in cache misses, disk reads, and even slower performance. [Back pressure](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html) can help by limiting the queue size, thereby maintaining a high throughput rate and good response times for jobs already in the queue. Once the queue fills up, clients get a server busy or HTTP 503 status code to try again later. Clients can retry the request at a later time, perhaps with [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff).


## فشار معکوس (Back Pressure)

زمانی که صف‌ها (queue) به طور قابل توجهی رشد می‌کنند، اندازه صف می‌تواند از حافظه فراتر رود و در نتیجه منجر به خطاهای حافظه کَش (cache miss)، خواندن از دیسک و در نهایت عملکرد پایین‌تر شود. «فشار معکوس» با محدود کردن اندازه صف به حفظ نرخ خروجی بالا و زمان پاسخگویی مناسب برای کارهایی که قبلاً در صف قرار دارند، کمک می‌کند.

هنگامی که صف پر می‌شود، سرور به کلاینت‌ها (clients) سیگنال "سرور مشغول است" یا کد وضعیت HTTP 503 ارسال می‌کند تا بعداً دوباره تلاش کنند. کلاینت‌ها می‌توانند درخواست را در زمان دیگری، احتمالاً با تأخیر تصاعدی (exponential backoff)، دوباره امتحان کنند.


----------

# Task Queues

Tasks queues receive tasks and their related data, runs them, then delivers their results. They can support scheduling and can be used to run computationally-intensive jobs in the background.

[Celery](https://docs.celeryproject.org/en/stable/) has support for scheduling and primarily has python support.

## صف‌های وظیفه (Task Queues)

صف‌های وظیفه، وظایف (task) و داده‌های مرتبط با آن‌ها را دریافت می‌کنند، آن‌ها را اجرا کرده و سپس نتایج را تحویل می‌دهند. این صف‌ها می‌توانند زمان‌بندی را پشتیبانی کنند و برای اجرای کارهای محاسباتی سنگین در پس‌زمینه مورد استفاده قرار گیرند.

یکی از ابزارهای محبوب برای صف‌های وظیفه، **Celery** است که از زمان‌بندی پشتیبانی می‌کند و عمدتاً از زبان برنامه‌نویسی پایتون (Python) پشتیبانی می‌کند. 

-------------


# Message Queues

Message queues receive, hold, and deliver messages. If an operation is too slow to perform inline, you can use a message queue with the following workflow:

- An application publishes a job to the queue, then notifies the user of job status
- A worker picks up the job from the queue, processes it, then signals the job is complete

The user is not blocked and the job is processed in the background. During this time, the client might optionally do a small amount of processing to make it seem like the task has completed. For example, if posting a tweet, the tweet could be instantly posted to your timeline, but it could take some time before your tweet is actually delivered to all of your followers.

- [Redis](https://redis.io/) is useful as a simple message broker but messages can be lost.
- [RabbitMQ](https://www.rabbitmq.com/) is popular but requires you to adapt to the ‘AMQP’ protocol and manage your own nodes.
- [AWS SQS](https://aws.amazon.com/sqs/) is hosted but can have high latency and has the possibility of messages being delivered twice.
- [Apache Kafka](https://kafka.apache.org/) is a distributed event store and stream-processing platform.


## صف‌های پیام (Message Queues)

صف‌های پیام، پیام‌ها را دریافت، نگهداری و تحویل می‌دهند.  اگر انجام یک عملیات به صورت درون‌خطی (inline)  بسیار زمانبر است، می‌توانید با استفاده از یک صف پیام و با گردش کار زیر، آن را بهبود بخشید:

* یک برنامه، یک کار (job) را در صف منتشر و سپس  وضعیت کار  را به کاربر اطلاع می‌دهد.
* یک پردازشگر (worker)، کار را از صف برداشته، آن را پردازش کرده و سپس تکمیل کار را اعلام می‌کند.
* کاربر مسدود نشده و تسک در پس زمینه پردازش می‌شود. در این مدت، کاربر (اختیاری) می‌تواند تسک‌ها یا کارهای کوچک دیگری را برای نمایش پیشرفت کار انجام دهد. به عنوان مثال، هنگام ارسال یک توییت، ممکن است بلافاصله در تایم‌لاین شما ارسال شود، اما ممکن است مدتی طول بکشد تا واقعاً برای همه دنبال کنندگان شما ارسال شود.

برخی از صف‌های پیام محبوب به شرح زیر هستند:

* **Redis:** به عنوان یک کارگزار ساده پیام (message broker) مفید است، اما پیام‌ها ممکن است از بین بروند.
* **RabbitMQ:** محبوب است، اما نیازمند سازگاری با پروتکل AMQP و مدیریت گره‌های خود (nodes) است.
* **AWS SQS:** میزبانی شده است، اما می‌تواند تأخیر زیادی داشته باشد و احتمال ارسال دو باره پیام‌ها وجود دارد.
* **Apache Kafka:** یک پلتفرم توزیع شده برای ذخیره رویدادها (event store) و پردازش جریان (stream-processing) است.


--------------
****************************
+++++++++++++++++++++++++++++++++

# Caching

Caching is the process of storing frequently accessed data in a temporary storage location, called a cache, in order to quickly retrieve it without the need to query the original data source. This can improve the performance of an application by reducing the number of times a data source must be accessed.

There are several caching strategies:

- Refresh Ahead
- Write-Behind
- Write-through
- Cache Aside

Also, you can have the cache in several places, examples include:

- Client Caching
- CDN Caching
- Web Server Caching
- Database Caching
- Application Caching

## ذخیره سازی کَش (Caching)

ذخیره سازی کَش (Caching) فرآیندی است که در آن داده‌های پرکاربرد به صورت موقت در مکانی به نام حافظه کَش (cache) ذخیره می‌شوند تا بازیابی سریع آنها بدون نیاز به مراجعه مستقیم به منبع اصلی داده انجام شود. این کار با کاهش تعداد دفعات دسترسی به منبع اصلی داده، باعث بهبود عملکرد برنامه می‌شود.

چندین استراتژی برای ذخیره سازی کَش وجود دارد، از جمله:

* ‏Refresh Ahead: با نزدیک شدن به زمان انقضای داده در حافظه کَش، داده‌های جدید از منبع اصلی فراخوانده و جایگزین داده‌های قدیمی در حافظه کَش می‌شوند.
* نوشتن پس‌زمینه (Write-Behind):  داده‌ها ابتدا در حافظه کَش نوشته شده و سپس با تاخیر به منبع اصلی داده نوشته می‌شوند.
* نوشتن همزمان (Write-Through): همزمان با نوشتن داده در حافظه کَش، داده در منبع اصلی نیز نوشته می‌شود.
*  Cache Aside: در این روش، منطق اصلی برنامه برای خواندن و نوشتن داده بدون در نظر گرفتن حافظه کَش نوشته می‌شود. سپس، یک لایه جداگانه برای بررسی حافظه کَش قبل از دسترسی به منبع اصلی داده اضافه می‌شود.

حافظه کَش همچنین می‌تواند در مکان‌های مختلفی قرار گیرد، از جمله:

* حافظه کَش کاربر (Client Caching): حافظه کَشی که در دستگاه کاربر قرار دارد.
* حافظه کَش توزیع محتوا (CDN Caching): حافظه کَشی که در شبکه توزیع محتوا (CDN) قرار دارد.
* حافظه کَش وب سرور (Web Server Caching): حافظه کَشی که در وب سرور قرار دارد.
* حافظه کَش پایگاه داده (Database Caching): حافظه کَشی که در کنار پایگاه داده قرار دارد.
* حافظه کَش برنامه (Application Caching): حافظه کَشی که درون برنامه تعبیه شده است.

--------------------------

نکته :

اینا رو توضیح ندادم 

![[Pasted image 20240414145312.png]]

----------------

++++++++++++++++++++++++++++


# Consistency Patterns

Consistency patterns refer to the ways in which data is stored and managed in a distributed system, and how that data is made available to users and applications. There are three main types of consistency patterns:

- Strong consistency
- Weak consistency
- Eventual Consistency

Each of these patterns has its own advantages and disadvantages, and the choice of which pattern to use will depend on the specific requirements of the application or system.


## الگوهای سازگاری (Consistency Patterns)

الگوهای سازگاری به روش‌هایی اشاره دارند که داده در یک سیستم توزیع‌شده ذخیره و مدیریت می‌شود و چگونه این داده در دسترس کاربران و برنامه‌ها قرار می‌گیرد. سه نوع اصلی از الگوهای سازگاری وجود دارد:

* **سازگاری قوی (Strong consistency):**  در این الگو، تمام گره‌های (nodes) سیستم یک نسخه یکسان و به‌روز از داده را در هر لحظه در اختیار دارند. هر تغییری در داده بلافاصله به همه گره‌ها منتقل می‌شود. این الگو بیشترین سازگاری را ارائه می‌دهد، اما دستیابی به آن در سیستم‌های توزیع‌شده می‌تواند دشوار و پرهزینه باشد.
* **سازگاری ضعیف (Weak consistency):** در این الگو، الزامی برای اینکه همه گره‌ها بلافاصله از به‌روزرسانی داده مطلع شوند، وجود ندارد. ممکن است مدتی طول بکشد تا تغییرات به همه گره‌ها منتقل شود و کاربران یا برنامه‌ها ممکن است نسخه‌های متفاوتی از داده را در زمان‌های مختلف مشاهده کنند. این الگو می‌تواند عملکرد و مقیاس‌پذیری را بهبود بخشد، اما برای برنامه‌هایی که به داده‌های کاملاً به‌روز نیاز دارند، مناسب نیست.
* **سازگاری نهایی (Eventual consistency):**  این الگو تضمین می‌کند که در نهایت، همه گره‌ها نسخه یکسانی از داده را در اختیار خواهند داشت. با این حال، ممکن است مدتی طول بکشد تا این اتفاق بیفتد. این الگو معمولاً در سیستم‌هایی که تأخیر در به‌روزرسانی داده قابل قبول است و در اولویت قرار دادن در دسترس بودن و مقیاس‌پذیری است، استفاده می‌شود.

انتخاب هر یک از این الگوها به نیازمندی‌های خاص برنامه یا سیستم بستگی دارد. برای مثال، یک سیستم بانکی ممکن است به سازگاری قوی برای اطمینان از صحت تراکنش‌ها نیاز داشته باشد، در حالی که یک شبکه اجتماعی ممکن است از سازگاری نهایی برای بهینه‌سازی عملکرد استفاده کند.



--------------
++++++++++++++

# Load Balancers

Load balancers distribute incoming client requests to computing resources such as application servers and databases. In each case, the load balancer returns the response from the computing resource to the appropriate client. Load balancers are effective at:

- Preventing requests from going to unhealthy servers
- Preventing overloading resources
- Helping to eliminate a single point of failure

Load balancers can be implemented with hardware (expensive) or with software such as HAProxy. Additional benefits include:

- **SSL termination** - Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations
    - Removes the need to install X.509 certificates on each server
- **Session persistence** - Issue cookies and route a specific client’s requests to same instance if the web apps do not keep track of sessions

## Disadvantages of load balancer

- The load balancer can become a performance bottleneck if it does not have enough resources or if it is not configured properly.
- Introducing a load balancer to help eliminate a single point of failure results in increased complexity.
- A single load balancer is a single point of failure, configuring multiple load balancers further increases complexity.



## توزیع کننده بار (Load Balancers)

توزیع کننده بار وظیفه توزیع درخواست‌های ورودی از سمت کاربران (client) به منابع محاسباتی مانند سرورهای برنامه و پایگاه‌های داده را بر عهده دارد. در هر مورد، توزیع کننده بار پاسخ را از منبع محاسباتی به کاربر مربوطه ارسال می‌کند. توزیع کننده‌های بار در موارد زیر موثر هستند:

* **جلوگیری از ارسال درخواست به سرورهای معیوب:** با توزیع بار، درخواست‌ها به سرورهای سالم هدایت شده و از ارسال به سرورهای از کار افتاده جلوگیری می‌شود.
* **جلوگیری از اضافه بار منابع:** توزیع کننده بار با مدیریت ترافیک ورودی مانع از بارگذاری بیش از حد منابع محاسباتی می‌شود.
* **حذف نقطه واحد شکست (Single Point of Failure):** با حذف وابستگی به یک سرور واحد، در صورت خرابی یک سرور، سایر سرورها همچنان به سرویس‌دهی ادامه می‌دهند.

توزیع کننده بار را می‌توان به صورت سخت‌افزاری (پرهزینه) یا نرم‌افزاری مانند HAProxy پیاده‌سازی کرد. مزایای اضافی توزیع کننده بار شامل موارد زیر است:

* **اتمام SSL:** رمزگشایی درخواست‌های ورودی و رمزگذاری پاسخ‌های سرور به گونه‌ای که سرورهای پشتیبان نیازی به انجام این عملیات پرهزینه نداشته باشند.
* **حذف نیاز به نصب گواهینامه‌های X.509 روی هر سرور:** با اتمام SSL توسط توزیع کننده بار، دیگر نیازی به نصب جداگانه این گواهینامه‌ها روی تک تک سرورها نیست.
* **پایداری نشست (Session Persistence):** صدور کوکی (cookie) و مسیریابی درخواست‌های یک کاربر خاص به همان نمونه سرور (در صورتی که برنامه‌های وب، خودشان مدیریت نشست را انجام ندهند).

**معایب توزیع کننده بار:**

* **تبدیل شدن به گلوگاه عملکردی (Performance Bottleneck):** در صورتی که توزیع کننده بار منابع کافی نداشته باشد یا به درستی پیکربندی نشود، خود می‌تواند به یک گلوگاه عملکرد تبدیل شود و باعث کاهش کارایی شود.
* **افزایش پیچیدگی:** معرفی یک توزیع کننده بار برای حذف نقطه واحد شکست، منجر به افزایش پیچیدگی سیستم می‌شود.
* **نقطه واحد شکست در توزیع کننده بار:** خود توزیع کننده بار نیز می‌تواند یک نقطه واحد ضعف و شکست باشد. پیکربندی چندین توزیع کننده بار برای رفع این مشکل، پیچیدگی را بیش از پیش افزایش می‌دهد.

-------------------


# Load Balancer vs Reverse Proxy

- Deploying a load balancer is useful when you have multiple servers. Often, load balancers route traffic to a set of servers serving the same function.
- Reverse proxies can be useful even with just one web server or application server, opening up the benefits described in the previous section.
- Solutions such as NGINX and HAProxy can support both layer 7 reverse proxying and load balancing.

## Disadvantages of Reverse Proxy:

- Introducing a reverse proxy results in increased complexity.
- A single reverse proxy is a single point of failure, configuring multiple reverse proxies (ie a failover) further increases complexity.


## توزیع کننده بار در مقابل پروکسی معکوس (Load Balancer vs Reverse Proxy)

در نگاه اول، توزیع کننده بار (Load Balancer) و پروکسی معکوس (Reverse Proxy) ممکن است شبیه به هم به نظر برسند، اما در واقع نقش‌های متفاوتی در یک سیستم توزیع شده ایفا می‌کنند. در اینجا به بررسی تفاوت‌های کلیدی آن‌ها می‌پردازیم:

**توزیع کننده بار (Load Balancer):**

* **کاربرد:** در صورتی که چندین سرور داشته باشید که یک کارکرد مشابه را انجام می‌دهند، توزیع کننده بار مفید است. 
* **وظیفه:** وظیفه اصلی توزیع کننده بار، هدایت و توزیع درخواست‌های ورودی از سمت کاربران (client) به سرورهای مختلف است. این توزیع با در نظر گرفتن عواملی مانند بار کاری سرورها و سلامت آن‌ها انجام می‌شود.
* **مزایا:**
    * جلوگیری از ارسال درخواست به سرورهای معیوب
    * جلوگیری از اضافه بار سرورها
    * حذف نقطه واحد شکست (Single Point of Failure)
* **معایب:**
    * تبدیل شدن به گلوگاه عملکرد در صورت عدم پیکربندی صحیح یا کمبود منابع
    * افزایش پیچیدگی با معرفی توزیع کننده بار

**پروکسی معکوس (Reverse Proxy):**

* **کاربرد:** پروکسی معکوس حتی با یک سرور وب یا سرور برنامه نیز کاربردی است.
* **وظیفه:** پروکسی معکوس به عنوان واسطه‌ای بین کاربر و سرور عمل می‌کند. درخواست‌های کاربر را دریافت کرده و ممکن است پیش از ارسال به سرور اصلی، تغییراتی روی آن‌ها اعمال کند (مانند مسیریابی، امنیت، کش). سپس پاسخ سرور را دریافت و برای کاربر ارسال می‌کند.
* **مزایا:**
    * امنیت: پروکسی معکوس می‌تواند برخی از وظایف امنیتی مانند اتمام SSL را انجام دهد.
    * بهبود عملکرد: با استفاده از کَش می‌توان سرعت دسترسی به منابع را افزایش داد.
    * مدیریت بار: در حد ابتدایی می‌تواند به مدیریت بار روی سرور اصلی کمک کند.
* **معایب:**
    * افزایش پیچیدگی با معرفی پروکسی معکوس
    * نقطه واحد شکست در صورت استفاده از تک پروکسی معکوس

**نکات کلیدی:**

* توزیع کننده بار برای توزیع ترافیک بر روی چندین سرور با کارکرد مشابه استفاده می‌شود.
* پروکسی معکوس واسطه‌ای بین کاربر و سرور اصلی است و می‌تواند وظایف مختلفی مانند امنیت، کش و مدیریت ابتدایی بار را انجام دهد.
* راه‌کارهایی مانند NGINX و HAProxy از هر دو قابلیت توزیع بار و نقش پروکسی معکوس در لایه ۷ (لایه اپلیکیشن) پشتیبانی می‌کنند.




-------------

# Load Balancing Algorithms

A load balancer is a software or hardware device that keeps any one server from becoming overloaded. A load balancing algorithm is the logic that a load balancer uses to distribute network traffic between servers (an algorithm is a set of predefined rules).

There are two primary approaches to load balancing. Dynamic load balancing uses algorithms that take into account the current state of each server and distribute traffic accordingly. Static load balancing distributes traffic without making these adjustments. Some static algorithms send an equal amount of traffic to each server in a group, either in a specified order or at random.
## الگوریتم‌های توزیع بار (Load Balancing Algorithms)

توزیع کننده بار (load balancer) یک دستگاه نرم افزاری یا سخت افزاری است که از اضافه بارگذاری روی هر یک از سرورها جلوگیری می‌کند. الگوریتم توزیع بار منطقی است که توزیع کننده بار از آن برای توزیع ترافیک شبکه بین سرورها استفاده می‌کند (الگوریتم مجموعه ای از قوانین از پیش تعریف شده است).

دو رویکرد اصلی برای توزیع بار وجود دارد:

* **توزیع بار پویا (Dynamic load balancing):** در این روش، الگوریتم‌ها با در نظر گرفتن وضعیت فعلی هر سرور، ترافیک را توزیع می‌کنند. به عنوان مثال، ممکن است سروری شلوغ باشد در حالی که سرور دیگری بیکار باشد. الگوریتم توزیع بار پویا، ترافیک را به سمت سرور کم کار هدایت می‌کند تا بار به طور مساوی توزیع شود.
* **توزیع بار ایستا (Static load balancing):** در این روش، ترافیک بدون در نظر گرفتن وضعیت سرورها توزیع می‌شود.  برخی از الگوریتم‌های ایستا، ترافیک را به طور مساوی بین سرورها توزیع می‌کنند، این توزیع می‌تواند به صورت گردشی (round robin) یا تصادفی (random) باشد.

در اینجا برخی از رایج ترین الگوریتم‌های توزیع بار آورده شده است:

* **Round Robin (گردشی):** این الگوریتم ساده، درخواست‌ها را به صورت نوبت به سرورهای موجود ارسال می‌کند. 
* **Least Connections (کمترین اتصال):** در این روش، سروری که در حال حاضر کمترین تعداد اتصال را دارد، برای درخواست بعدی انتخاب می‌شود. 
* **Weighted Round Robin (گردشی با وزن):** این الگوریتم مشابه گردشی (round robin) است، اما به هر سرور وزنی اختصاص داده می‌شود. سرورهای با وزن بالاتر، درخواست‌های بیشتری را دریافت می‌کنند. این روش برای سرورهایی با توانایی پردازش متفاوت مناسب است.
* **Least Response Time (کمترین زمان پاسخ):** در این روش، سروری که کمترین زمان پاسخگویی را در آخرین بررسی‌ها داشته، برای درخواست بعدی انتخاب می‌شود.

انتخاب الگوریتم مناسب به عوامل مختلفی از جمله تعداد سرورها، نوع ترافیک و نیازمندی‌های برنامه بستگی دارد.



------------------


# Layer 7 Load Balancing

Layer 7 load balancers look at the application layer to decide how to distribute requests. This can involve contents of the header, message, and cookies. Layer 7 load balancers terminate network traffic, reads the message, makes a load-balancing decision, then opens a connection to the selected server. For example, a layer 7 load balancer can direct video traffic to servers that host videos while directing more sensitive user billing traffic to security-hardened servers.

At the cost of flexibility, layer 4 load balancing requires less time and computing resources than Layer 7, although the performance impact can be minimal on modern commodity hardware.

## توزیع بار لایه 7 (Layer 7 Load Balancing)

توزیع کننده بار لایه 7 با بررسی لایه کاربرد (application layer) تصمیم می‌گیرد که درخواست‌ها را چگونه توزیع کند. این بررسی می‌تواند شامل محتویات هدر (header)، پیام و کوکی‌ها (cookies) باشد. توزیع کننده بار لایه 7 ترافیک شبکه را خاتمه می‌دهد، پیام را می‌خواند، تصمیم توزیع بار را می‌گیرد و سپس اتصالی را به سرور انتخاب شده باز می‌کند. به عنوان مثال، یک توزیع کننده بار لایه 7 می‌تواند ترافیک ویدیو را به سرورهایی که میزبان ویدیوها هستند هدایت کند و در عین حال ترافیک حساس‌تر صورتحساب کاربران را به سرورهایی با امنیت بالا هدایت کند.

--------------------


# Layer 4 Load Balancing

Layer 4 load balancers look at info at the transport layer to decide how to distribute requests. Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet. Layer 4 load balancers forward network packets to and from the upstream server, performing Network Address Translation (NAT).


## توزیع بار لایه 4 (Layer 4 Load Balancing)

توزیع کننده بار لایه 4 با بررسی اطلاعات موجود در لایه حمل و نقل (transport layer) شبکه، در مورد توزیع درخواست‌ها تصمیم گیری می‌کند. به طور کلی، این اطلاعات شامل آدرس‌های IP منبع و مقصد و پورت‌ها در هدر بسته (packet header) می‌شود، اما محتوای داخل بسته در نظر گرفته نمی‌شود. توزیع کننده بار لایه 4 بسته‌های شبکه را به سرورهای بالادستی (upstream servers) ارسال و از آنها دریافت می‌کند و در عین حال ترجمه آدرس شبکه (NAT) را انجام می‌دهد.

---------------------



# Horizontal Scaling

Load balancers can also help with horizontal scaling, improving performance and availability. Scaling out using commodity machines is more cost efficient and results in higher availability than scaling up a single server on more expensive hardware, called Vertical Scaling. It is also easier to hire for talent working on commodity hardware than it is for specialized enterprise systems.

## Disadvantages of horizontal scaling

- Scaling horizontally introduces complexity and involves cloning servers
    - Servers should be stateless: they should not contain any user-related data like sessions or profile pictures
    - Sessions can be stored in a centralized data store such as a database (SQL, NoSQL) or a persistent cache (Redis, Memcached)
- Downstream servers such as caches and databases need to handle more simultaneous connections as upstream servers scale out.



## مقیاس‌بندی افقی (Horizontal Scaling)

توزیع کننده‌های بار همچنین می‌توانند با مقیاس‌بندی افقی به بهبود عملکرد و در دسترس بودن سیستم کمک کنند. 
مقیاس‌بندی با استفاده از سخت‌افزارهای رایج از نظر هزینه مؤثرتر است و منجر به در دسترس بودن بالاتری نسبت به ارتقای یک سرور واحد با سخت‌افزار گران‌تر (مقیاس‌بندی عمودی) می‌شود. همچنین استخدام افرادی که روی سخت‌افزارهای رایج کار می‌کنند، نسبت به سیستم‌های تخصصی سازمانی آسان‌تر است.

## معایب مقیاس‌بندی افقی

مقیاس‌بندی افقی باعث افزایش پیچیدگی شده و شامل کلون کردن سرورها می‌شود.
سرورها باید stateless باشند: یعنی نباید هیچ داده مرتبط با کاربر مانند جلسات یا تصاویر پروفایل را در خود ذخیره کنند.
جلسات (Sessions) را می‌توان در یک ذخیره‌گاه داده مرکزی مانند پایگاه داده (SQL، NoSQL) یا یک حافظه کَش دائمی (Redis، Memcached) ذخیره کرد.
سرورهای پایین‌دستی مانند حافظه‌های کَش و پایگاه‌های داده با افزایش سرورهای بالادستی نیاز به مدیریت اتصالات همزمان بیشتری دارند.


-----------
# Microservices

Related to the “Application Layer” discussion are microservices, which can be described as a suite of independently deployable, small, modular services. Each service runs a unique process and communicates through a well-defined, lightweight mechanism to serve a business goal. 1

Pinterest, for example, could have the following microservices: user profile, follower, feed, search, photo upload, etc.

## میکروسرویس‌ها (Microservices)

مایکروسرویس‌ها وجود دارند که می‌توان آن‌ها را مجموعه‌ای از سرویس‌های مستقل، کوچک و ماژولار توصیف کرد که قابلیت استقرار مستقل دارند. هر سرویس یک فرآیند منحصر به فرد را اجرا می‌کند و از طریق یک مکانیزم سبک و از پیش تعریف شده برای خدمت به یک هدف تجاری خاص، ارتباط برقرار می‌کند.

به عنوان مثال، پینترست می‌تواند دارای میکروسرویس‌های زیر باشد: پروفایل کاربری، دنبال‌کننده، فید، جستجو، آپلود عکس و غیره.


-----------------------

# Service Discovery

Systems such as [Consul](https://www.consul.io/docs/index.html), [Etcd](https://coreos.com/etcd/docs/latest), and [Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) can help services find each other by keeping track of registered names, addresses, and ports. [Health checks](https://www.consul.io/intro/getting-started/checks.html) help verify service integrity and are often done using an HTTP endpoint. Both Consul and Etcd have a built in key-value store that can be useful for storing config values and other shared data.


## کشف سرویس (Service Discovery)

در سیستم‌های توزیع‌شده‌ای که از میکروسرویس‌ها استفاده می‌شود، سرویس‌ها برای برقراری ارتباط با یکدیگر به روشی برای کشف هم نیاز دارند. کشف سرویس به این معنی است که سرویس‌ها بتوانند موقعیت (آدرس و پورت) سایر سرویس‌ها را پیدا کنند.

ابزارهایی مانند Consul، Etcd و Zookeeper به کشف سرویس کمک می‌کنند. این ابزارها با ثبت نام سرویس‌ها به همراه نام، آدرس و پورت آن‌ها، به سایر سرویس‌ها امکان می‌دهند موقعیت یکدیگر را پیدا کنند.

علاوه بر کشف سرویس، این ابزارها اغلب از بررسی‌های سلامت (Health Checks) نیز پشتیبانی می‌کنند. بررسی‌های سلامت با استفاده از یک نقطه انتهایی HTTP (معمولا یک API) انجام می‌شود تا از در دسترس بودن و سلامت سرویس‌ها اطمینان حاصل شود.

برخی از این ابزارها مانند Consul و Etcd همچنین دارای یک حافظه کلید-مقدار (Key-Value Store) داخلی هستند که برای ذخیره مقادیر پیکربندی (Configuration Values) و سایر داده‌های مشترک بین سرویس‌ها مفید است.


-------------

# Fail-Over

Failover is an availability pattern that is used to ensure that a system can continue to function in the event of a failure. It involves having a backup component or system that can take over in the event of a failure.

In a failover system, there is a primary component that is responsible for handling requests, and a secondary (or backup) component that is on standby. The primary component is monitored for failures, and if it fails, the secondary component is activated to take over its duties. This allows the system to continue functioning with minimal disruption.

Failover can be implemented in various ways, such as active-passive, active-active, and hot-standby.

## Active-passive

With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active’s IP address and resumes service.

The length of downtime is determined by whether the passive server is already running in ‘hot’ standby or whether it needs to start up from ‘cold’ standby. Only the active server handles traffic.

Active-passive failover can also be referred to as master-slave failover.

## Active-active

In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers. If the servers are internal-facing, application logic would need to know about both servers.

Active-active failover can also be referred to as master-master failover.

## Disadvantages of Failover

- Fail-over adds more hardware and additional complexity.
- There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.


##  failover

Failover یک الگوی در دسترس بودن (availability) است که برای اطمینان از ادامه کارکرد یک سیستم در صورت بروز خرابی استفاده می‌شود. این شامل داشتن یک مؤلفه یا سیستم پشتیبان است که می‌تواند در صورت خرابی، مسئولیت را بر عهده گیرد.

در یک سیستم Failover، یک مؤلفه اصلی وجود دارد که وظیفه رسیدگی به درخواست‌ها را بر عهده دارد و یک مؤلفه ثانویه (یا پشتیبان) به صورت آماده به کار (standby) وجود دارد. مؤلفه اصلی برای خرابی‌ها مانیتور می‌شود و در صورت خرابی، مؤلفه ثانویه برای انجام وظایف آن فعال می‌شود. این امر به سیستم اجازه می‌دهد تا با حداقل اختلال به کار خود ادامه دهد.

Failover را می‌توان به روش‌های مختلفی مانند فعال-پسیو، فعال-فعال و آماده به کار گرم (hot-standby) اجرا کرد.

### فعال-پسیو (Active-Passive)

با Failover فعال-پسیو، سیگنال‌هایی معروف به ضربان قلبی(heartbeats) بین سرور فعال و سرور آماده به کار رد و بدل می‌شود. اگر این سیگنال قطع شود، سرور آماده به کار، آدرس IP سرور فعال را به عهده گرفته و سرویس دهی را از سر می‌گیرد.

مدت زمان خرابی بستگی به این دارد که آیا سرور آماده به کار از قبل در حالت "گرم" (hot) اجرا شده باشد یا اینکه نیاز به راه اندازی از حالت "سرد" (cold) داشته باشد. فقط سرور فعال ترافیک را مدیریت می‌کند.

Failover فعال-پسیو را همچنین می‌توان Failover اصلی-فرعی (master-slave) نامید.

### فعال-فعال (Active-Active)

در حالت فعال-فعال، هر دو سرور ترافیک را مدیریت می‌کنند و بار را بین خود تقسیم می‌کنند.

اگر سرورها رو به بیرون (public-facing) باشند، DNS باید از IP‌های عمومی هر دو سرور مطلع باشد. اگر سرورها رو به داخل (internal-facing) باشند، منطق برنامه باید از هر دو سرور مطلع باشد.

Failover فعال-فعال را همچنین می‌توان Failover اصلی-اصلی (master-master) نامید.

## معایب Failover

Failover سخت افزار بیشتر و پیچیدگی بیشتری را به همراه دارد.
در صورتی که سیستم فعال قبل از تکثیر داده‌های تازه نوشته شده به سیستم غیرفعال، با مشکل مواجه شود، احتمال از دست رفتن داده‌ها وجود دارد.

-------------

# Replication

Replication is an availability pattern that involves having multiple copies of the same data stored in different locations. In the event of a failure, the data can be retrieved from a different location. There are two main types of replication: Master-Master replication and Master-Slave replication.

- **Master-Master replication:** In this type of replication, multiple servers are configured as “masters,” and each one can accept read and write operations. This allows for high availability and allows any of the servers to take over if one of them fails. However, this type of replication can lead to conflicts if multiple servers update the same data at the same time, so some conflict resolution mechanism is needed to handle this.
    
- **Master-Slave replication:** In this type of replication, one server is designated as the “master” and handles all write operations, while multiple “slave” servers handle read operations. If the master fails, one of the slaves can be promoted to take its place. This type of replication is simpler to set up and maintain compared to Master-Master replication.

## تکثیر داده (Replication)

تکثیر داده یک الگوی در دسترس بودن است که شامل داشتن چندین کپی از داده‌های یکسان در مکان‌های مختلف است. در صورت خرابی، داده‌ها را می‌توان از مکان دیگری بازیابی کرد. دو نوع اصلی تکثیر وجود دارد: تکثیر اصلی-اصلی (Master-Master) و تکثیر اصلی-فرعی (Master-Slave).

### تکثیر اصلی-اصلی (Master-Master):

در این نوع تکثیر، چندین سرور به عنوان "اصلی" پیکربندی می‌شوند و هر کدام می‌توانند عملیات خواندن و نوشتن را انجام دهند. این امر به در دسترس بودن بالا و امکان جانشینی هر یک از سرورها در صورت خرابی یکی از آنها کمک می‌کند. با این حال، این نوع تکثیر در صورت به‌روزرسانی همزمان داده‌های یکسان توسط چندین سرور در یک زمان، می‌تواند منجر به تضاد شود، بنابراین برای رسیدگی به این موضوع به یک مکانیزم حل تضاد نیاز است.

### تکثیر اصلی-فرعی (Master-Slave):‏

در این نوع تکثیر، یک سرور به عنوان "اصلی" تعیین می‌شود و تمام عملیات نوشتن را کنترل می‌کند، در حالی که چندین سرور "فرعی" عملیات خواندن را انجام می‌دهند. اگر سرور اصلی با مشکل مواجه شود، یکی از سرورهای فرعی می‌تواند ارتقا پیدا کند و جایگزین آن شود. راه اندازی و نگهداری این نوع تکثیر نسبت به تکثیر اصلی-اصلی ساده تر است.


----------

# Content Delivery Networks

A content delivery network (CDN) is a globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon’s CloudFront support dynamic content. The site’s DNS resolution will tell clients which server to contact.

Serving content from CDNs can significantly improve performance in two ways:

- Users receive content from data centers close to them
- Your servers do not have to serve requests that the CDN fulfills


## شبکه توزیع محتوا (Content Delivery Networks - CDN)

شبکه توزیع محتوا (CDN) شبکه‌ای سراسری از سرورهای واسط (proxy server) است که محتوا را از مکان‌هایی نزدیک‌تر به کاربر ارائه می‌دهد. به طور کلی، فایل‌های استاتیک مانند HTML/CSS/JS، عکس‌ها و ويديوها از طریق CDN ارائه می‌شوند، اگرچه برخی از CDNها مانند CloudFront آمازون از محتوای پویا نیز پشتیبانی می‌کنند. تفکیک‌پذیری DNS سایت به کاربران می‌گوید که با کدام سرور تماس بگیرند.

ارائه محتوا از طریق CDN می‌تواند عملکرد را از دو طریق به طور قابل توجهی بهبود بخشد:

* کاربران محتوا را از مراکز داده نزدیک به خود دریافت می‌کنند.
* سرورهای شما مجبور نیستند به درخواست‌هایی که CDN برآورده می‌کند، پاسخ دهند.



--------------
# Background Jobs

Background jobs in system design refer to tasks that are executed in the background, independently of the main execution flow of the system. These tasks are typically initiated by the system itself, rather than by a user or another external agent.

Background jobs can be used for a variety of purposes, such as:

- Performing maintenance tasks: such as cleaning up old data, generating reports, or backing up the database.
- Processing large volumes of data: such as data import, data export, or data transformation.
- Sending notifications or messages: such as sending email notifications or push notifications to users.
- Performing long-running computations: such as machine learning or data analysis.




## کارهای پس‌زمینه (Background Jobs)

در طراحی سیستم، کارهای پس‌زمینه به وظایفی گفته می‌شود که به صورت مجزا از جریان اجرای اصلی سیستم، در پس‌زمینه اجرا می‌شوند. این وظایف معمولاً به جای کاربر یا عامل خارجی دیگر، توسط خود سیستم آغاز می‌گردند.

کارهای پس‌زمینه را می‌توان برای اهداف مختلفی به کار گرفت، از جمله:

* **انجام وظایف نگهداری:** مانند پاکسازی داده‌های قدیمی، تهیه گزارش یا پشتیبان‌گیری از پایگاه داده.
* **پردازش حجم زیادی از داده‌ها:** مانند وارد کردن داده‌ها، خروجی گرفتن داده‌ها یا تبدیل داده‌ها.
* **فرستادن اعلان‌ها یا پیام‌ها:** مانند ارسال ایمیل‌های اطلاع‌رسانی یا   push notification به کاربران.
* **انجام محاسبات طولانی‌مدت:** مانند یادگیری ماشین یا تحلیل داده‌ها.


----------

# Latency vs Throughput

Latency and throughput are two important measures of a system’s performance. **Latency** refers to the amount of time it takes for a system to respond to a request. **Throughput** refers to the number of requests that a system can handle at the same time.

Generally, you should aim for maximal throughput with acceptable latency.

## تأخیر (Latency) در مقابل توان عملیاتی (Throughput)

تأخیر (Latency) و توان عملیاتی (Throughput) دو معیار مهم برای سنجش عملکرد یک سیستم هستند.

* **تأخیر (Latency):** مدت زمانی است که طول می‌کشد تا یک سیستم به یک درخواست پاسخ دهد. تأخیر کم نشان دهنده پاسخگویی سریع سیستم است. برای مثال، تأخیر کم در هنگام بارگذاری یک صفحه وب به معنای نمایش سریعتر صفحه برای کاربر است.
* **توان عملیاتی (Throughput):** تعداد درخواست‌هایی است که یک سیستم می‌تواند در یک دوره زمانی مشخص مدیریت کند. توان عملیاتی بالا نشان می‌دهد که سیستم ظرفیت پاسخگویی به حجم زیادی از درخواست‌ها را دارد. برای مثال، توان عملیاتی بالای یک سرور وب به این معنی است که می‌تواند به طور همزمان به درخواست‌های کاربران زیادی پاسخ دهد.

به طور کلی، شما باید به دنبال حداکثر توان عملیاتی با تأخیر قابل قبول باشید. در حالت ایده‌آل، می‌خواهید سیستم شما بتواند درخواست‌های زیادی را به طور همزمان مدیریت کند (توان عملیاتی بالا) در حالی که همچنان پاسخگویی سریعی برای هر درخواست (تأخیر کم) داشته باشد. 

این دو معیار اغلب با هم در ارتباط هستند. برای مثال، افزایش توان عملیاتی گاهی می‌تواند منجر به افزایش تأخیر شود، زیرا سیستم با حجم بیشتری از درخواست‌ها سروکار دارد. 

بنابراین، هنگام تنظیم و بهینه‌سازی یک سیستم، باید تعادلی بین تأخیر و توان عملیاتی برقرار کنید. 


-------------



# Event Driven

Event-driven invocation uses a trigger to start the background task. Examples of using event-driven triggers include:

- The UI or another job places a message in a queue. The message contains data about an action that has taken place, such as the user placing an order. The background task listens on this queue and detects the arrival of a new message. It reads the message and uses the data in it as the input to the background job. This pattern is known as asynchronous message-based communication.
- The UI or another job saves or updates a value in storage. The background task monitors the storage and detects changes. It reads the data and uses it as the input to the background job.
- The UI or another job makes a request to an endpoint, such as an HTTPS URI, or an API that is exposed as a web service. It passes the data that is required to complete the background task as part of the request. The endpoint or web service invokes the background task, which uses the data as its input.

## فراخوان مبتنی بر رویداد (Event-Driven Invocation)

فراخوان مبتنی بر رویداد (Event-Driven Invocation) از یک رویداد محرک (trigger) برای شروع کارهای پس‌زمینه استفاده می‌کند. نمونه‌هایی از استفاده از محرک‌های مبتنی بر رویداد عبارتند از:

* رابط کاربری (UI) یا یک کار پس‌زمینه دیگر، پیامی را در یک صف (queue) قرار می‌دهد. این پیام حاوی داده‌هایی در مورد عملی است که انجام شده است، مانند ثبت سفارش توسط کاربر.
* کار پس‌زمینه به این صف گوش می‌دهد و رسیدن یک پیام جدید را تشخیص می‌دهد. این کار پیام را خوانده و از داده‌های موجود در آن به عنوان ورودی برای کار پس‌زمینه استفاده می‌کند. این الگو به عنوان ارتباط ناهمزمان مبتنی بر پیام شناخته می‌شود (Asynchronous Message-Based Communication).
* رابط کاربری یا یک کار پس‌زمینه دیگر، مقداری را در حافظه ذخیره‌سازی کرده یا به‌روزرسانی می‌کند. کار پس‌زمینه، حافظه ذخیره‌سازی را کنترل کرده و تغییرات را تشخیص می‌دهد. این کار داده‌ها را خوانده و از آنها به عنوان ورودی برای کار پس‌زمینه استفاده می‌کند.
* رابط کاربری یا یک کار پس‌زمینه دیگر درخواستی را به یک نقطه انتهایی (endpoint)، مانند یک URI HTTPS یا یک API که به عنوان یک سرویس وب در معرض دید قرار گرفته است، ارسال می‌کند. این درخواست، داده‌های مورد نیاز برای تکمیل کار پس‌زمینه را به عنوان بخشی از درخواست منتقل می‌کند. نقطه انتهایی یا سرویس وب، کار پس‌زمینه را فراخوانی می‌کند که از داده‌ها به عنوان ورودی خود استفاده می‌کند.

در فراخوان مبتنی بر رویداد، به جای اینکه مستقیماً یک کار پس‌زمینه را راه‌اندازی کنید، رویدادی را ایجاد می‌کنید که نشان‌دهنده نیاز به انجام یک کار خاص است. این رویداد به عنوان محرک عمل می‌کند و کار پس‌زمینه در زمان مناسب با توجه به رویداد شروع می‌شود. این رویکرد مزایای مختلفی از جمله:

* **کوپِلینگ شل (Loose Coupling):** اجزای سیستم به هم وابسته نیستند. هر جزء می‌تواند رویدادها را منتشر کند و به رویدادهای مورد علاقه‌اش گوش دهد، بدون اینکه بداند چه کسی رویدادها را تولید یا مصرف می‌کند.
* **مقیاس‌پذیری (Scalability):** شما می‌توانید به راحتی کارهای پس‌زمینه بیشتری را برای پردازش رویدادها اضافه کنید تا با افزایش حجم کار مطابقت داشته باشید.
* **انعطاف‌پذیری (Resilience):** اگر یک کار پس‌زمینه با مشکل مواجه شود، رویداد همچنان در صف باقی می‌ماند و بعداً توسط یک کارگر دیگر پردازش می‌شود.
-----------------

# Schedule Driven

Schedule-driven invocation uses a timer to start the background task. Examples of using schedule-driven triggers include:

- A timer that is running locally within the application or as part of the application’s operating system invokes a background task on a regular basis.
- A timer that is running in a different application, such as Azure Logic Apps, sends a request to an API or web service on a regular basis. The API or web service invokes the background task.
- A separate process or application starts a timer that causes the background task to be invoked once after a specified time delay, or at a specific time.

Typical examples of tasks that are suited to schedule-driven invocation include batch-processing routines (such as updating related-products lists for users based on their recent behavior), routine data processing tasks (such as updating indexes or generating accumulated results), data analysis for daily reports, data retention cleanup, and data consistency checks.


## فراخوان مبتنی بر زمان‌بندی (Schedule-Driven Invocation)

فراخوان مبتنی بر زمان‌بندی (Schedule-Driven Invocation) از یک زمان‌بندی‌کننده (timer) برای راه‌اندازی کارهای پس‌زمینه استفاده می‌کند. نمونه‌هایی از استفاده از محرک‌های مبتنی بر زمان‌بندی عبارتند از:

* یک زمان‌بندی‌کننده که به صورت محلی در داخل برنامه یا به عنوان بخشی از سیستم عامل برنامه در حال اجرا است، به طور منظم یک کار پس‌زمینه را فراخوانی می‌کند.
* یک زمان‌بندی‌کننده که در یک برنامه کاربردی دیگر مانند Azure Logic Apps در حال اجرا است، به طور منظم درخواستی را به یک API یا سرویس وب ارسال می‌کند. API یا سرویس وب، کار پس‌زمینه را فراخوانی می‌کند.
* یک فرآیند یا برنامه جداگانه، زمان‌بندی‌کننده‌ای را راه‌اندازی می‌کند که باعث می‌شود کار پس‌زمینه یک بار پس از تأخیر زمانی مشخص یا در زمان خاصی فراخوانی شود.

نمونه‌های معمولی از کارهایی که برای فراخوان مبتنی بر زمان‌بندی مناسب هستند عبارتند از:

* روال‌های پردازش دسته‌ای (مانند به‌روزرسانی لیست‌های محصولات مرتبط برای کاربران بر اساس رفتار اخیر آنها)
* کارهای روتین پردازش داده‌ها (مانند به‌روزرسانی فهرست‌ها یا ایجاد نتایج انباشته)
* تجزیه و تحلیل داده‌ها برای گزارش‌های روزانه
* پاکسازی نگهداری داده‌ها
* بررسی‌های ثبات داده‌ها

در فراخوان مبتنی بر زمان‌بندی، شما یک زمان‌بندی‌کننده راه‌اندازی می‌کنید که در فواصل زمانی مشخص یا در زمان‌های خاصی، کار پس‌زمینه را فراخوانی می‌کند. این رویکرد مزایای مختلفی از جمله:

* **سادگی (Simplicity):** راه‌اندازی و مدیریت کارهای زمان‌بندی‌شده نسبتاً ساده است. 
* **قابلیت اطمینان (Reliability):**  اطمینان حاصل می‌کند که کارها در زمان‌های تعیین شده اجرا می‌شوند، حتی اگر رویداد خاصی برای تحریک آنها وجود نداشته باشد.
* **پیش‌بینی‌پذیری (Predictability):**  می‌توانید دقیقاً پیش‌بینی کنید که چه زمانی کارها اجرا می‌شوند، که برای برخی از سناریوها، مانند تهیه گزارش‌های دوره‌ای، مفید است.

با این حال، فراخوان مبتنی بر زمان‌بندی همچنین می‌تواند معایبی داشته باشد، از جمله:

* **عدم انعطاف‌پذیری (Less Flexibility):**  برخلاف فراخوان مبتنی بر رویداد، زمان‌بندی را نمی‌توان به راحتی بر اساس رویدادهای خاص تنظیم کرد.
* **مصرف غیرضروری منابع (Potential Resource Waste):**  ممکن است کارهایی را در زمان‌هایی اجرا کنید که واقعاً مورد نیاز نیستند، که منجر به مصرف غیرضروری منابع می‌شود.

بنابراین، انتخاب بین فراخوان مبتنی بر زمان‌بندی و فراخوان مبتنی بر رویداد به نیازمندی‌های خاص کار پس‌زمینه شما بستگی دارد.

------------

# Returning Results

Background jobs execute asynchronously in a separate process, or even in a separate location, from the UI or the process that invoked the background task. Ideally, background tasks are “fire and forget” operations, and their execution progress has no impact on the UI or the calling process. This means that the calling process does not wait for completion of the tasks. Therefore, it cannot automatically detect when the task ends.

## بازگرداندن نتایج (Returning Results)

کارهای پس‌زمینه به صورت ناهمزمان در یک فرآیند جداگانه یا حتی در یک مکان جداگانه از رابط کاربری (UI) یا فرآیندی که کار پس‌زمینه را فراخوانی کرده است، اجرا می‌شوند. در حالت ایده‌آل، کارهای پس‌زمینه عملیات «اجرا کن و فراموش کن» هستند و روند اجرای آنها تأثیری روی رابط کاربری یا فرآیند فراخوانی ندارد. این بدان معنی است که فرآیند فراخوانی منتظر تکمیل کارها نمی‌ماند. بنابراین، نمی‌تواند به طور خودکار تشخیص دهد که چه زمانی کار به پایان می‌رسد.

برای اطلاع از وضعیت یا خروجی کارهای پس‌زمینه، چندین استراتژی رایج وجود دارد:

* **صفوط (Queues):** در این روش، کار پس‌زمینه نتایج خود را در یک صف (Queue) قرار می‌دهد. یک فرآیند جداگانه دیگر به طور مداوم این صف را نظارت می‌کند و در صورت وجود هر گونه نتیجه جدید، آن را پردازش می‌کند. این فرآیند جداگانه می‌تواند نتایج را در رابط کاربری نمایش دهد، آن‌ها را در پایگاه داده ذخیره کند، یا هر اقدام دیگری را که با خروجی کار پس‌زمینه ضروری است انجام دهد.
* **رویدادها (Events):** در این روش، کار پس‌زمینه هنگام تکمیل، رویدادی را منتشر می‌کند. هر فرآیندی که علاقه‌مند به دریافت نتایج کار پس‌زمینه باشد، می‌تواند برای این رویداد گوش دهد. هنگامی که رویداد منتشر می‌شود، فرآیند گیرنده می‌تواند اطلاعات مربوط به نتایج را از رویداد استخراج کند.
* **ذخیره‌سازی موقت (Temporary Storage):** در این روش، کار پس‌زمینه نتایج خود را در یک مکان ذخیره‌سازی موقت، مانند یک پایگاه داده موقت یا حافظه پنهان (Cache)، ذخیره می‌کند. فرآیند فراخوانی یا یک فرآیند جداگانه دیگر می‌تواند بعداً وضعیت یا خروجی کار را از این مکان ذخیره‌سازی موقت بازیابی کند.
* **پینگ‌مانیتورینگ (Polling):** در این روش، فرآیند فراخوانی به طور دوره‌ای با یک نقطه انتهایی (endpoint) ارتباط برقرار می‌کند تا بررسی کند که آیا کار پس‌زمینه تکمیل شده است یا خیر. اگر کار تکمیل شده باشد، نقطه انتهایی نتایج را به فرآیند فراخوانی برمی‌گرداند. این رویکرد نسبت به سایر استراتژی‌ها کارآمدتر نیست و می‌تواند منجر به مصرف غیرضروری منابع شود، بنابراین باید با احتیاط استفاده شود.

انتخاب بهترین استراتژی برای بازگرداندن نتایج به عوامل مختلفی از جمله حجم داده‌های خروجی، الزامات تأخیر، و پیچیدگی کلی سیستم بستگی دارد.

----------

# CAP Theorem

According to CAP theorem, in a distributed system, you can only support two of the following guarantees:

- **Consistency** - Every read receives the most recent write or an error
- **Availability** - Every request receives a response, without guarantee that it contains the most recent version of the information
- **Partition Tolerance** - The system continues to operate despite arbitrary partitioning due to network failures

Networks aren’t reliable, so you’ll need to support partition tolerance. You’ll need to make a software tradeoff between consistency and availability.

## CP - consistency and partition tolerance

Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.

## AP - availability and partition tolerance

Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.

AP is a good choice if the business needs to allow for [eventual consistency](https://github.com/donnemartin/system-design-primer#eventual-consistency) or when the system needs to continue working despite external errors.


## قضیه CAP (CAP Theorem)

در یک سیستم توزیع‌شده، طبق قضیه CAP، شما تنها می‌توانید از دو مورد از تضمین‌های زیر پشتیبانی کنید:

* **سازگاری (Consistency):** هر خواندن، جدیدترین نوشتن را دریافت می‌کند یا با خطا مواجه می‌شود.
* **در دسترس بودن (Availability):** هر درخواست پاسخی دریافت می‌کند، بدون اینکه تضمینی بر حاوی بودن آخرین نسخه از اطلاعات وجود داشته باشد.
* **تحمل پارتیشن‌بندی (Partition Tolerance):** سیستم علی‌رغم پارتیشن‌بندی دلخواه ناشی از خرابی‌های شبکه، به کار خود ادامه می‌دهد.

از آنجایی که شبکه‌ها قابل اعتماد نیستند، شما نیاز به پشتیبانی از تحمل پارتیشن‌بندی دارید. بنابراین باید بین سازگاری و در دسترس بودن، یک مصالحه نرم‌افزاری انجام دهید.

**گزینه‌های CAP:**

* **CP (سازگاری و تحمل پارتیشن‌بندی):**

    * انتظار برای پاسخ از گره پارتیشن‌شده ممکن است منجر به خطای زمان خروج (timeout) شود.
    * CP انتخاب مناسبی است اگر نیازهای کسب‌وکار شما به خواندن و نوشتن اتمی (atomic) نیاز داشته باشد.

* **AP (در دسترس بودن و تحمل پارتیشن‌بندی):**

    * پاسخ‌ها، در دسترس‌ترین نسخه داده موجود در هر گره را برمی‌گردانند که ممکن است آخرین نسخه نباشد.
    * ممکن است مدتی طول بکشد تا نوشته‌ها پس از رفع پارتیشن، منتشر شوند.
    * AP انتخاب مناسبی است اگر نیازهای کسب‌وکار به سازگاری نهایی (eventual consistency) اجازه دهد یا زمانی که سیستم نیاز به ادامه کار علی‌رغم خطاهای خارجی داشته باشد.    


-------------



## tenants

در مهندسی رایانه، **tenants** (در لغت به معنای مستاجر یا ساکن) به واحدهای جداگانه ای از یک سیستم یا برنامه اطلاق می شود که به طور مستقل عمل می کنند و منابع را به اشتراک می گذارند. این مفهوم شبیه به آپارتمان های یک ساختمان است که هر کدام واحد مجزا با حریم خصوصی و منابع مختص به خود هستند، اما همگی در یک ساختمان مشترک و زیر یک سقف قرار دارند.

در زمینه مهندسی رایانه، tenants می توانند به روش های مختلفی پیاده سازی شوند، از جمله:

* **مجازی سازی:** در این روش، از نرم افزار یا سخت افزار برای ایجاد چندین محیط مجازی جداگانه در یک سیستم واحد استفاده می شود. هر tenant مجازی می تواند سیستم عامل، برنامه ها و داده های خود را داشته باشد و به طور مستقل از tenants دیگر عمل کند.
* **چند مستاجری:** در این روش، یک برنامه یا سیستم به گونه ای طراحی می شود که بتواند توسط چندین کاربر یا سازمان به طور همزمان استفاده شود. هر tenant می تواند فضای ذخیره سازی، پردازش و سایر منابع خود را داشته باشد و به طور مستقل از tenants دیگر عمل کند.
* **محاسبات ابری:** در این روش، منابع محاسباتی، مانند سرورها، ذخیره سازی و شبکه، از طریق اینترنت به کاربران ارائه می شود. هر tenant می تواند فضای ذخیره سازی، پردازش و سایر منابع خود را در ابر داشته باشد و به طور مستقل از tenants دیگر عمل کند.

استفاده از tenants در مهندسی رایانه مزایای متعددی دارد، از جمله:

* **افزایش امنیت:** tenants می توانند به طور جداگانه ای ایمن شوند، که به این معنی است که یک tenant آسیب دیده نمی‌تواند به tenants دیگر دسترسی داشته باشد.
* **افزایش مقیاس پذیری:** tenants می توانند به طور مستقل از یکدیگر مقیاس بندی شوند، که به این معنی است که می توان منابع را به tenants خاص در صورت نیاز اختصاص داد.
* **کاهش هزینه:** tenants می توانند به طور کارآمدتر از منابع استفاده کنند، که به نوبه خود می تواند منجر به کاهش هزینه ها شود.
* **افزایش انعطاف پذیری:** tenants می توانند به طور مستقل از یکدیگر پیکربندی و مدیریت شوند، که به این معنی است که می توان آنها را به طور خاص برای نیازهای هر tenant سفارشی کرد.

در اینجا چند نمونه از tenants در مهندسی رایانه آورده شده است:

* **مستاجران SaaS:** در مدل SaaS، نرم افزار به عنوان یک سرویس از طریق اینترنت ارائه می شود. هر tenant می تواند به نسخه خود از نرم افزار دسترسی داشته باشد و بدون نیاز به نصب یا مدیریت نرم افزار، از آن استفاده کند.
* **مستاجران PaaS:** در مدل PaaS، پلتفرم به عنوان یک سرویس از طریق اینترنت ارائه می شود. هر tenant می تواند از پلتفرم برای توسعه و استقرار برنامه های خود استفاده کند.
* **مستاجران IaaS:** در مدل IaaS، زیرساخت به عنوان یک سرویس از طریق اینترنت ارائه می شود. هر tenant می تواند از سرورها، ذخیره سازی و شبکه خود در ابر استفاده کند.

استفاده از tenants در مهندسی رایانه در حال افزایش است، زیرا سازمان ها به دنبال راه هایی برای افزایش امنیت، مقیاس پذیری، انعطاف پذیری و کاهش هزینه های خود هستند.


--------------------
-------------
--------

# خلاصه‌ای از الگوهای طراحی ابری

## Ambassador

سرویس‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌‌های واسطه، سرویس‌های کمکی‌ای(helper services) هستند که به جای یک سرویس مصرف‌کننده یا برنامه، درخواست‌های شبکه را ارسال می‌کنند. این سرویس را می‌توان به عنوان یک پروکسی خارج از فرایند در نظر گرفت که در کنار سرویس گیرنده (کلاینت) قرار دارد.

این الگو برای برون‌سپاری کارهای مشترک ارتباط با کلاینت، مانند مانیتورینگ، لاگ‌گیری، مسیریابی، امنیت (مانند TLS) و الگوهای مقاومت به خطا به روشی مستقل از زبان برنامه‌نویسی، مناسب است. 

از این الگو اغلب برای برنامه‌های  قدیمی یا سایر برنامه‌هایی که تغییر آن‌ها دشوار است، به منظور گسترش قابلیت‌های شبکه‌ای آن‌ها استفاده می‌شود. همچنین می‌تواند به یک تیم متخصص امکان پیاده‌سازی این ویژگی‌ها را بدهد.

# Anti-corruption Layer

این سناریو نیازمند پیاده‌سازی یک لایه نمایشی (Facade) یا آداپتور بین دو زیرسامانه با معنای متفاوت است. این لایه واسط، درخواست‌های ارسالی از یک زیرسامانه به زیرسامانه دیگر را ترجمه می‌کند. با استفاده از این الگو، اطمینان حاصل می‌شود که طراحی یک برنامه توسط وابستگی به زیرسامانه‌های خارجی محدود نمی‌شود. این الگو برای اولین بار توسط اریک ایوانز در کتاب "طراحی هدایت شده توسط دامنه" (Domain-Driven Design) توصیف شده است.

## Backends for Frontend


الگوی سرویس‌های اختصاصی برای فرانت‌اند (BFF) به این معناست که برای هر برنامه یا رابط کاربری فرانت‌اند، یک سرویس بک‌اند جداگانه ایجاد شود. این الگو زمانی مفید است که نمی‌خواهید یک بک‌اند واحد را برای چندین رابط کاربری مختلف سفارشی کنید.

## CQRS


CQRS مخفف عبارت **Command Query Responsibility Segregation** (جداسازی مسئولیت فرمان و پرس و جو) است، الگویی که عملیات خواندن و به‌روزرسانی را برای یک ذخیره‌گاه داده جدا می‌کند. پیاده‌سازی CQRS در برنامه شما می‌تواند عملکرد، قابلیت مقیاس‌پذیری و امنیت آن را به طور قابل توجه‌ای افزایش دهد.


## تجميع منابع محاسباتی (Compute Resource Consolidation)

تجميع منابع محاسباتی به معنای **ادغام چندین کار یا عملیات در یک واحد محاسباتی واحد** است. این کار می‌تواند منجر به افزایش **بهره‌وری منابع محاسباتی** و در نتیجه **کاهش هزینه‌ها و سربار مدیریت** مرتبط با انجام پردازش‌های محاسباتی در برنامه‌های کاربردی میزبانی‌شده در ابر شود.


## ذخیره‌گاه پیکربندی خارجی (External Configuration Store)

ذخیره‌گاه پیکربندی خارجی به این معناست که **اطلاعات پیکربندی به جای جایگذاری در بسته استقرار برنامه، در یک مکان مرکزی نگهداری شود**. این کار مزایای متعددی را به همراه دارد:
## تجميع دروازه (Gateway Aggregation)

تجميع درخواست دروازه (Gateway Aggregation) الگویی است که در معماری میکروسرویس‌ها کاربرد دارد. در این الگو، یک **سرویس دروازه** به عنوان واسطه بین **سرویس گیرنده (کلاینت)** و **سرویس‌های پشتیبان (بک‌اند)** عمل می‌کند. دروازه وظیفه دارد چندین درخواست مجزا از سرویس گیرنده را به درخواست‌های واحد برای سرویس‌های بک‌اند مربوطه تبدیل کند و نتایج آن‌ها را جمع‌آوری و به کلاینت بازگرداند.

## مسیریابی دروازه (Gateway Routing)

مسیریابی دروازه به شما این امکان را می‌دهد تا درخواست‌ها را با استفاده از یک نقطه ورود (endpoint) واحد، به سرویس‌های مختلف یا نمونه‌های متعدد از یک سرویس هدایت کنید. این الگو در سناریوهای زیر کاربرد مفیدی دارد:

- **قرار دادن چندین سرویس پشت یک نقطه ورود واحد:** با این الگو می‌توانید چندین سرویس مجزا را در معرض دید قرار دهید، در حالی که تنها با یک آدرس و پورت واحد سروکار دارید. دروازه بر اساس اطلاعات موجود در درخواست (مانند مسیر URL، هدر درخواست و غیره) تصمیم می‌گیرد که درخواست را به کدام سرویس ارسال کند.
- **توزیع بار بین نمونه‌های سرویس:** اگر از چندین نمونه از یک سرویس برای افزایش قابلیت تحمل خطا (fault tolerance) و توزیع بار استفاده می‌کنید، مسیریابی دروازه می‌تواند درخواست‌ها را به صورت چرخشی (round-robin) یا بر اساس الگوریتم‌های دیگر بین این نمونه‌ها توزیع کند.
- **مدیریت نسخه‌های سرویس:** در صورتی که سرویس شما چندین نسخه با قابلیت‌های مختلف دارد، می‌توانید از مسیریابی دروازه برای مسیریابی درخواست‌ها به نسخه‌های مختلف بر اساس نیازهای کاربر استفاده کنید.

## Leader Election

در یک برنامه توزیع شده، با انتخاب یک نمونه به عنوان رهبر که مسئولیت مدیریت سایر نمونه‌ها را بر عهده می‌گیرد، اقدامات انجام شده توسط مجموعه ای از نمونه‌های همکاری کننده را هماهنگ و کنترل کنید. این کار می‌تواند به اطمینان از عدم تداخل نمونه‌ها با یکدیگر، ایجاد رقابت نمونه‌ها برای استفاده از منابع مشترک یا دخالت ناخواسته در کاری که سایر نمونه‌ها انجام می‌دهند، کمک کند.


## Sidecar


اجزای یک برنامه را برای ایزوله سازی و کپسوله سازی در یک فرایند یا کانتینر (container) جداگانه مستقر کنید. این الگو همچنین می‌تواند به برنامه‌ها اجازه دهد تا از اجزا و فناوری‌های ناهمگون تشکیل شوند.

این الگو به دلیل شباهت به سایدکار متصل به موتورسیکلت، سایدکار نامیده می‌شود. در این الگو، سایدکار به یک برنامه اصلی متصل است و ویژگی‌های حمایتی را برای برنامه فراهم می‌کند. سایدکار همچنین دارای چرخه عمر مشابه با برنامه اصلی است و در کنار برنامه اصلی ایجاد و حذف می‌شود. الگوی سایدکار گاهی اوقات به عنوان الگوی همکار (sidekick) شناخته می‌شود و یک الگوی از نوع تجزیه (decomposition) است. 

## Static Content Hosting

محتوای ایستا را در یک سرویس ذخیره سازی ابری مستقر کنید که بتواند آنها را به طور مستقیم به کاربر تحویل دهد. این کار می‌تواند نیاز به نمونه‌های محاسباتی بالقوه گران قیمت را کاهش دهد.


## Cache Aside

داده‌ها را پس از درخواست و واکشی از یک ذخیره‌گاه داده در یک حافظه کَش بارگذاری کنید. این کار می‌تواند کارایی برنامه را بهبود بخشد و همچنین به حفظ سازگاری (consistency) بین داده‌های موجود در حافظه کَش و داده‌های موجود در ذخیره‌گاه داده اصلی کمک کند. 

## Event Sourcing

به جای اینکه فقط وضعیت فعلی داده را در یک حوزه ذخیره کنید، از یک ذخیره‌گاه append-only برای ثبت همه‌ی سری اقداماتی که روی آن داده انجام شده استفاده کنید. این ذخیره‌گاه به عنوان سیستم ثبت عمل می‌کند و می‌تواند برای مادی سازی اشیاء دامنه (materialize the domain objects) استفاده شود. این کار می‌تواند با اجتناب از نیاز به همگام سازی مدل داده و حوزه تجاری، کارایی، قابلیت مقیاس پذیری و پاسخگویی را در حوزه‌های پیچیده ساده کند. همچنین می‌تواند سازگاری داده تراکنشی را فراهم کند و ردیابی و تاریخچه کاملی را برای انجام اقدامات جبرانی حفظ کند.

## Index Table

برای بهبود عملکرد کوئری، فیلدهای پرکاربرد در ذخیره‌گاه داده را ایندکس کنید. این الگو به برنامه‌ها این امکان را می‌دهد تا داده‌های مورد نظر را سریع‌تر از ذخیره‌گاه داده بازیابی کنند.

## Materialized View


داده‌های موجود در یک یا چند ذخیره‌گاه داده را به صورت پیش‌ساخته (prepopulated) نمایش دهید (view) به خصوص زمانی که این داده‌ها به طور بهینه برای عملیات پرس و جو (query) فرمت بندی نشده اند. این کار می‌تواند از کوئری کارآمد و استخراج داده پشتیبانی کند و عملکرد برنامه را بهبود بخشد. 

## Sharding


شاردینگ (Shardding) تکنیکی است که برای پارتیشن بندی افقی (تقسیم بندی بر اساس ردیف‌های جدول) یک مجموعه داده بزرگ در چندین سرور استفاده می‌شود تا عملکرد، مقیاس پذیری و در دسترس بودن یک سیستم را بهبود بخشد. این کار با شکستن مجموعه داده به بخش‌های کوچکتر به نام shard و توزیع آنها در چندین سرور انجام می‌شود. هر shard مستقل است و می‌توان آن را به طور مستقل از shardهای دیگر مدیریت و مقیاس بندی کرد. شاردینگ را می‌توان در سناریوهایی مانند مقیاس پذیری، در دسترس بودن و توزیع جغرافیایی (ذخیره داده در سرورهای مختلف مناطق جغرافیایی) استفاده کرد. شاردینگ را می‌توان با استفاده از الگوریتم‌های مختلفی مانند شاردینگ مبتنی بر محدوده (range-based sharding)، شاردینگ مبتنی بر hash و شاردینگ مبتنی بر دایرکتوری اجرا کرد.


## Valet Key


از یک توکن برای دسترسی مستقیم و محدود به یک منبع خاص برای کلاینت‌ها استفاده کنید تا انتقال داده از برنامه را کاهش دهد. این امر به خصوص در برنامه هایی که از سیستم‌های ذخیره سازی ابری یا صف‌های ابری استفاده می‌کنند مفید است و می‌تواند هزینه را به حداقل برساند و قابلیت مقیاس پذیری و کارایی را به حداکثر برساند. 


----------------------
--------
# الگوهای پیام‌رسانی

## Asynchronous Request-Reply


چندین الگو برای جدا کردن بک‌اند از host فرانت‌اند در سناریویی که پردازش بک‌اند ناهمزمان است اما فرانت‌اند همچنان به پاسخ مشخصی نیاز دارد، وجود دارد. 

## Claim Check


پیام‌های حجیم را به یک رسید (claim check) و محموله (payload) تقسیم کنید. رسید را به پلتفرم پیام رسانی ارسال کرده و محموله را در یک سرویس خارجی ذخیره کنید. این الگو به پردازش پیام‌های حجیم کمک می‌کند و در عین حال از کند شدن یا تحت فشار قرار گرفتن پلتفرم پیام رسانی و  پلتفرم کلاینت جلوگیری می‌کند. همچنین این الگو به کاهش هزینه‌ها کمک می‌کند، زیرا ذخیره سازی معمولا ارزان‌تر از واحدهای منابع استفاده شده توسط پلتفرم پیام رسانی است. 

## Choreography

بجای تکیه بر یک نقطه مرکزی کنترل، به جای هر جزء از سیستم در فرآیند تصمیم گیری در مورد گردش کار یک تراکنش تجاری مشارکت دهید.

## Competing Consumers

چندین مصرف کننده همزمان را فعال کنید تا پیام‌های دریافت شده در یک کانال پیام رسانی را پردازش کنند. با چندین مصرف کننده همزمان، یک سیستم می‌تواند چندین پیام را به طور همزمان پردازش کند تا توان عملیاتی را بهینه کند، مقیاس پذیری(scalability) و در دسترس بودن (availability) را بهبود بخشد و بار کاری را متعادل کند.

## Priority Queue


درخواست‌های ارسال‌شده به سرویس‌ها را می‌توان اولویت‌بندی کرد تا درخواست‌های با اولویت بالاتر سریع‌تر از درخواست‌های با اولویت پایین‌تر دریافت و پردازش شوند. این الگو در برنامه‌ها و اپلیکیشن‌هایی که ضمانت‌های سطح خدمات (SLA) مختلفی را به مشتریان ارائه می‌دهند، مفید است. 

## Publisher Subscriber

الگوی انتشار-اشتراک (Pub/Sub) این امکان را برای یک اپلیکیشن فراهم می‌کند تا رویدادها را به صورت غیرهمزمان برای چندین مصرف‌کننده علاقه‌مند اعلام کند، بدون اینکه فرستنده‌ها با گیرنده‌ها وابستگی داشته باشند.

## Queue Based Load Leveling

با استفاده از یک صف (queue) به عنوان بافر بین یک وظیفه (task) و سرویسی که فراخوانی می‌کند، می‌توان نوسانات بار سنگین را که می‌تواند باعث خرابی سرویس یا time out تسک(task) شود را تعدیل کرد. این کار به کم کردن تاثیر اوج‌های تقاضا روی هر دو موردِ در دسترس بودن (availability) و پاسخگویی (responsiveness) در حالت  تسک و  سرویس، کمک می‌کند.
## Scheduling Agent Supervisor

مجموعه‌ای از اقدامات توزیع‌شده را به‌عنوان یک عملیات واحد هماهنگ کنید. در صورت خرابی هر یک از اقدامات، سعی کنید خرابی‌ها را به طور شفاف مدیریت کنید، در غیر این صورت، کار انجام‌شده را لغو کنید تا کل عملیات به طور کلی موفق یا شکست بخورد. این کار با امکان بازیابی و تلاش مجدد برای اقداماتی که به دلیل استثنائات گذرا، خرابی‌های طولانی‌مدت و خرابی‌های فرآیند، می‌تواند به انعطاف‌پذیری یک سیستم توزیع‌شده بیافزاید. 

## Sequential Convoy

مجموعه دنباله‌دار (Sequential Convoy) الگویی است که اجرای مجموعه‌ای از تسک‌ها را به ترتیب خاصی اجازه می‌دهد. این الگو را می‌توان برای اطمینان از اجرای صحیح مجموعه‌ای از وظایف وابسته و رسیدگی به خطاها یا خرابی‌ها در طول اجرای وظایف به کار برد. این الگو در سناریوهایی مانند گردش کار(workflow) و تراکنش (transaction) قابل استفاده است و می‌توان آن را با استفاده از فناوری‌های مختلفی مانند ماشین‌های حالت (state machines)، گردش کار و تراکنش‌ها پیاده‌سازی کرد. 


-------------
--------------

# الگوهای deployment

## Deployment Stamps

الگوی deployment stamp شامل تهیه، مدیریت و نظارت بر گروهی ناهمگون از منابع برای میزبانی و اجرای چندین بار کاری (workloads) یا مستاجر(tenants) است. هر نسخه جداگانه تمایز، واحد سرویس، واحد مقیاس یا سلول نامیده می‌شود. در یک محیط چند مستاجری، هر تمایز یا واحد مقیاس می‌تواند به تعداد از پیش تعریف شده ای از مستاجران خدمات ارائه دهد. تمایزهای متعدد را می‌توان برای مقیاس بندی تقریباً خطی راه حل و خدمت رسانی به تعداد فزاینده ای از مستاجران مستقر کرد. این رویکرد می‌تواند مقیاس پذیری راه حل شما را بهبود بخشد، امکان استقرار نمونه‌ها را در چندین منطقه فراهم کند و داده‌های مشتری شما را جدا کند.

## Geodes

الگوی ژئود (Geode) شامل استقرار مجموعه ای از سرویس‌های backend در مجموعه ای از گره‌های جغرافیایی است که هر کدام می‌توانند به هر درخواست از هر مشتری در هر منطقه سرویس دهند. این الگو به سرویس دهی درخواست‌ها به صورت فعال-فعال (active-active) اجازه می‌دهد، تأخیر را بهبود می‌بخشد و با توزیع پردازش درخواست در سراسر جهان، در دسترس بودن را افزایش می‌دهد.



## Health Endpoint Monitoring

برای اطمینان از عملکرد صحیح برنامه‌ها و سرویس ها، می‌توانید بررسی‌های عملکردی آن را در برنامه‌های که به کمک ابزارهای خارجی می‌توانند از طریق نقاط انتهایی (endpoints) در معرض دید قرار داده و در فواصل منظم زمانی به آنها دسترسی داشته باشند را پیاده سازی کنید.

## Throttling


مصرف منابع مورد استفاده توسط یک نمونه از یک اپلیکیشن، یک مستاجر (tenant) مستقل یا یک سرویس کامل را کنترل کنید. این می‌تواند به سیستم اجازه دهد تا به عملکرد خود ادامه دهد و توافقات سطح خدمات (SLA) را برآورده کند، حتی زمانی که افزایش تقاضا بار شدیدی را بر منابع وارد می‌کند.


## Bulkhead



الگوی بخش‌بندی (Bulkhead) نوعی طراحی برای نرم‌افزار است که در برابر خرابی مقاوم است. در معماری بخش‌بندی، اجزای یک برنامه در گروه‌های مجزا قرار می‌گیرند تا در صورت خرابی یکی، بقیه به کار خود ادامه دهند. این الگو برگرفته از بخش‌های جداگانه (بخش‌بندی) بدنه کشتی است. اگر بدنه کشتی آسیب ببیند، تنها بخش آسیب دیده از آب پر می‌شود و این مانع از غرق شدن کشتی می‌شود.

## Circuit Breaker


هنگام اتصال به یک سرویس یا منبع ریموت، خطاهایی را که ممکن است زمان متغیری برای بازیابی آنها طول بکشد، مدیریت کنید. این می‌تواند پایداری و انعطاف پذیری یک برنامه را بهبود بخشد.

## Compensating Transaction


اگر یک یا چند مرحله با شکست مواجه شوند، کار انجام شده توسط یک سری مراحل را لغو کنید، که با هم یک عملیات در  eventually consistent  را تعریف می‌کنند. عملیات‌هایی که از مدل  eventually consistent  پیروی می‌کنند معمولاً در برنامه‌های میزبانی ابری یافت می‌شوند که فرآیندها و گردش‌های کاری پیچیده تجاری را پیاده‌سازی می‌کنند.

لغو کردن کار انجام شده توسط یک سری از مراحل، که در مجموع یک عملیات با  eventually consistent  را تعریف می‌کنند، در صورتی که یک یا چند مرحله با شکست مواجه شوند. عملیات‌هایی که از مدل  eventually consistent  پیروی می‌کنند، معمولاً در برنامه‌ها و اپلیکیشن‌های ابری که فرآیندهای تجاری و گردش کار پیچیده را اجرا می‌کنند، یافت می‌شوند.

## Leader Election


برای هماهنگ سازی فعالیت‌های مجموعه ای از نمونه‌های همکاری کننده در یک برنامه توزیع شده، می‌توان از الگوی رهبر-پیرو (Leader-Follower) استفاده کرد. در این الگو، یک نمونه به عنوان رهبر انتخاب می‌شود و مسئولیت مدیریت سایر نمونه‌ها (پیروها) را بر عهده می‌گیرد. این روش به جلوگیری از تداخل نمونه‌ها با یکدیگر، رقابت برای منابع مشترک و اختلال ناخواسته در کار سایر نمونه‌ها کمک می‌کند.

## Retry


الگوی تلاش مجدد (Retry Pattern) به برنامه این امکان را می‌دهد تا در صورت برقراری ارتباط با یک سرویس یا منبع شبکه با خرابی‌های گذرا برخورد کند، با تلاش مجدد شفاف برای عملیات با شکست، ثبات برنامه را بهبود بخشد.


## Federated Identity pattern



احراز هویت را به یک ارائه دهنده هویت خارجی واگذار کنید. این می‌تواند توسعه را ساده کند، نیاز به مدیریت کاربر را به حداقل برساند و تجربه کاربری برنامه را بهبود بخشد.

## Gatekeeper


برای محافظت از برنامه‌ها و سرویس‌ها می‌توانید از یک نمونه میزبان اختصاصی به عنوان واسطه بین مشتریان و برنامه یا سرویس استفاده کنید، درخواست‌ها را اعتبارسنجی و پاکسازی می‌کند و درخواست‌ها و داده‌ها را بین آنها منتقل می‌کند. این می‌تواند یک لایه امنیتی اضافی ایجاد کند و سطح حمله سیستم را محدود کند.


--------------
-------------

## ضدالگوهای عملکرد (Performance Antipatterns)

ضدالگوهای عملکرد در طراحی سیستم به اشتباهات رایج یا شیوه‌های غیراصولی اشاره دارند که می‌توانند منجر به عملکرد ضعیف در یک سیستم شوند. این الگوها می‌توانند در سطوح مختلف سیستم رخ دهند و ناشی از عوامل متعددی مانند طراحی ضعیف، نبود بهینه‌سازی یا درک ناکافی از حجم کاری باشند.

برخی از نمونه‌های ضدالگوهای عملکرد عبارتند از:

* **کوئر‌های N+1**: این اتفاق زمانی می‌افتد که یک سیستم برای بازیابی داده‌های مرتبط، چندین کوئری به پایگاه داده ارسال می‌کند، به جای اینکه از یک کوئری واحد برای بازیابی تمام داده‌های مورد نیاز استفاده کند.

* **رابط‌های پرحرف (Chatty interfaces)**: این اتفاق زمانی می‌افتد که یک سیستم درخواست‌های کوچک و مکرر زیادی به یک سرویس یا API خارجی ارسال کند، به جای اینکه درخواست‌های کمتر و بزرگ‌تر ارسال کند.

* **داده‌های نامحدود**: این اتفاق زمانی می‌افتد که یک سیستم، داده‌های بیشتری نسبت به آنچه برای کار مورد نظر لازم است، بازیابی یا پردازش کند که منجر به افزایش استفاده از منابع و کاهش عملکرد می‌شود.

* **الگوریتم‌های ناکارآمد**: این اتفاق زمانی می‌افتد که یک سیستم از الگوریتمی استفاده کند که برای کار مورد نظر مناسب نباشد و منجر به افزایش استفاده از منابع و کاهش عملکرد شود.

## پایگاه داده شلوغ (Busy Database)

پایگاه داده شلوغ در طراحی سیستم به پایگاه داده ای اطلاق می‌شود که حجم بالایی از درخواست‌ها یا تراکنش‌ها را مدیریت می‌کند، این می‌تواند زمانی اتفاق بیفتد که یک سیستم ترافیک بالایی را تجربه می‌کند یا زمانی که پایگاه داده به درستی برای حجم کاری که مدیریت می‌کند بهینه سازی نشده باشد. این می‌تواند منجر به کاهش عملکرد، افزایش استفاده از منابع، بن بست و مشاجره، ناسازگاری داده‌ها شود. برای پرداختن به یک پایگاه داده شلوغ، تعدادی از رویکردها مانند Scaling out، Optimizing the schema، Caching و Indexing را می‌توان در نظر گرفت.

## فرانت-اند شلوغ (Busy Frontend)

انجام کارهای ناهمزمان (asynchronous) روی تعداد زیادی از رشته‌های پس‌زمینه (background threads) می‌تواند منجر به کمبود منابع برای سایر وظایف همزمان در پیش‌زمینه (foreground tasks) شود و در نتیجه زمان پاسخگویی را به سطوح غیرقابل قبولی کاهش دهد.

وظایف با مصرف منابع بالا می‌توانند زمان پاسخگویی به درخواست‌های کاربر را افزایش دهند و باعث تأخیر زیاد (high latency) شوند. یک راه برای بهبود زمان پاسخگویی، انتقال یک کار با مصرف منابع بالا به یک رشته جداگانه است. این رویکرد به برنامه اجازه می‌دهد در حالی که پردازش در پس‌زمینه اتفاق می‌افتد، همچنان پاسخگو باقی بماند. با این حال، کارهایی که روی یک رشته پس‌زمینه اجرا می‌شوند همچنان منابع را مصرف می‌کنند. اگر تعداد آن‌ها زیاد باشد، می‌توانند باعث کمبود منابع برای رشته‌هایی شوند که درخواست‌ها را مدیریت می‌کنند.

این مشکل معمولاً زمانی رخ می‌دهد که یک برنامه به عنوان یک قطعه کد یکپارچه (monolithic) توسعه داده شود، به طوری که تمام منطق تجاری در یک لایه واحد با لایه نمایش (presentation layer) ترکیب شود.

## Chatty I/O


  تاثیر تجمعی تعداد زیادی از درخواست‌های I/O می‌تواند تأثیر قابل توجهی بر عملکرد و پاسخگویی سیستم داشته باشد.

   در مقایسه با وظایف محاسباتی، فراخوانی‌های شبکه و سایر عملیات I/O ذاتاً کند هستند. هر درخواست I/O معمولاً دارای سربار قابل توجهی است و اثر تجمعی چندین عملیات I/O می‌تواند سیستم را آهسته کند. در اینجا برخی از دلایل رایج I/O پرحرف آورده شده است:

  * خواندن و نوشتن تک تک رکوردها در پایگاه داده به عنوان درخواست‌های مجزا
  * اجرای یک عملیات منطقی واحد به عنوان یک سری درخواست‌های HTTP
  * خواندن و نوشتن روی یک فایل روی دیسک


## فراخوانی اضافی (Extraneous Fetching)

فراخوانی اضافی در طراحی سیستم به رویه دریافت داده بیشتر از مقدار مورد نیاز برای یک کار یا عملیات خاص اشاره دارد. این می‌تواند زمانی رخ دهد که یک سیستم برای حجم کاری خاص بهینه سازی نشده باشد یا زمانی که سیستم برای مدیریت نیازمندی‌های داده به درستی طراحی نشده باشد.

فراخوانی اضافی می‌تواند منجر به مشکلات زیر شود:

* **افت عملکرد (Performance degradation)**: افزایش زمان پاسخگویی به دلیل بازیابی و پردازش داده‌های غیرضروری
* **افزایش استفاده از منابع (Increased resource utilization)**: مصرف بالای منابع پردازشی و حافظه به دلیل پردازش داده‌های اضافی
* **افزایش ترافیک شبکه (Increased network traffic):** انتقال غیر ضروری حجم زیادی از داده‌ها روی شبکه
* **تجربه کاربری ضعیف (Poor user experience):‏** کندی و تأخیر در عملکرد برنامه به دلیل بار اضافی ناشی از فراخوانی‌های اضافی

## ایجاد غلط اشیاء (Improper Instantiation)

ایجاد غلط اشیاء در طراحی سیستم به رویه ایجاد نمونه‌های غیرضروری از یک شیء، کلاس یا سرویس اشاره دارد که می‌تواند منجر به مشکلات عملکرد و مقیاس‌پذیری شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی طراحی نشده باشد، کد به روش کارآمد نوشته نشده باشد، یا کد برای سناریوی استفاده خاص بهینه نشده باشد.


## پایگاه داده یکپارچه (Monolithic Persistence)

پایگاه داده یکپارچه به روشی گفته می‌شود که در آن از یک پایگاه داده‌ی تنها برای ذخیره‌ی کل اطلاعات یک برنامه یا سیستم استفاده می‌شود. این روش برای سیستم‌های ساده و کوچک کاربرد دارد، اما با رشد و پیچیدگی سیستم، این پایگاه داده می‌تواند به یک گلوگاه تبدیل شود و در نتیجه منجر به مقیاس‌پذیری ضعیف، انعطاف‌پذیری محدود و پیچیدگی بیشتر شود. برای رفع این محدودیت‌ها، می‌توان از روش‌هایی مانند میکروسرویس‌ها، پارتیشن‌بندی (Sharding) و پایگاه‌های داده‌ی NoSQL استفاده کرد.



## بدون کش (No Caching)

ضد الگوی بدون کش زمانی رخ می‌دهد که یک برنامه ابری که با حجم زیادی از درخواست‌های همزمان سروکار دارد، به طور مکرر داده‌های یکسانی را بازیابی می‌کند. این کار باعث کاهش عملکرد و مقیاس‌پذیری می‌شود.

زمانی که داده‌ها در حافظه کش ذخیره نمی‌شوند، می‌تواند منجر به رفتارهای نامطلوبی شود، از جمله:

* بازیابی مکرر اطلاعات یکسان از منبعی که دسترسی به آن پرهزینه است، مانند سربار I/O یا تأخیر بالا.
* ساختن مکرر اشیاء یا ساختارهای داده‌ای یکسان برای درخواست‌های متعدد.
* برقراری تماس‌های بیش از حد با یک سرویس از راه دور که دارای سهمیه‌ی سرویس‌دهی است و فراتر از محدودیت مشخص‌شده، سرعت سرویس‌دهی را کاهش می‌دهد (throttles clients).

این مشکلات در نهایت می‌توانند منجر به زمان پاسخگویی ضعیف، افزایش رقابت در پایگاه داده و مقیاس‌پذیری پایین شوند.




## همسایه پر سر و صدا (Noisy Neighbor)

همسایه پر سر و صدا به وضعیتی گفته می‌شود که در آن یکی از اجزای سیستم یا چندتای آنها، از منابع مشترک به میزان نامتناسبی استفاده می‌کنند و این باعث درگیری بر سر منابع و در نهایت کاهش عملکرد سایر اجزا می‌شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی برای مدیریت حجم کاری طراحی یا پیکربندی نشده باشد، یا زمانی که یک جزء رفتاری غیرمنتظره داشته باشد.

نمونه‌هایی از سناریوهای همسایه پر سر و صدا عبارتند از:

- یک کاربر روی یک سرور مشترک که از مقدار زیادی CPU یا حافظه استفاده می‌کند و در نتیجه عملکرد سایر کاربران روی همان سرور را کاهش می‌دهد.
- یک فرایند روی یک سرور مشترک که از مقدار زیادی I/O استفاده می‌کند و باعث می‌شود سایر فرایندها با I/O کند و تأخیر زیاد مواجه شوند.
- یک برنامه که مقدار زیادی از پهنای باند شبکه را مصرف می‌کند و در نتیجه باعث کاهش توان عملیاتی (throughput) سایر برنامه‌ها می‌شود.


## طوفان بازپخش (Retry Storm)

طوفان بازپخش به وضعیتی گفته می‌شود که در آن تعداد زیادی از تلاش‌های مجدد (retry) در مدت زمان کوتاهی راه‌اندازی می‌شوند و این امر منجر به افزایش قابل توجه ترافیک و استفاده از منابع می‌شود. این اتفاق می‌تواند زمانی رخ دهد که سیستم به درستی برای مدیریت خطاها طراحی نشده باشد یا زمانی که یک جزء رفتاری غیرمنتظره داشته باشد. این وضعیت می‌تواند منجر به موارد زیر شود:

- **افت عملکرد (Performance degradation):** کند شدن سیستم به دلیل حجم بالای درخواست‌های بازپخش
- **افزایش استفاده از منابع (Increased resource utilization):** مصرف بالای منابع پردازشی و حافظه به دلیل پردازش درخواست‌های تکراری
- **افزایش ترافیک شبکه (Increased network traffic):** ایجاد بار اضافی روی شبکه به علت ارسال مجدد حجم زیادی از درخواست‌ها
- **تجربه کاربری ضعیف (Poor user experience):‏** کندی و تأخیر در عملکرد برنامه به دلیل بار اضافی ناشی از طوفان بازپخش

برای مقابله با طوفان بازپخش، می‌توان از روش‌هایی مانند تأخیر تصاعدی (Exponential backoff)، قطع‌کنندگی مدار (Circuit breaking) و نظارت و هشدار (Monitoring and alerting) استفاده کرد.



##  Synchronous I/O  

در I/O همزمان، تا زمانی که عملیات ورودی/خروجی تکمیل شود، رشته (thread) فراخواننده بلاک می‌شود. این موضوع می‌تواند باعث کاهش عملکرد و تحت تاثیر قرار دادن مقیاس‌پذیری عمودی (vertical scalability) شود.

در یک عملیات I/O همزمان، رشته فراخواننده تا زمان تکمیل I/O، بلوکه شده و در وضعیت انتظار قرار می‌گیرد. در این مدت رشته نمی‌تواند کار مفیدی انجام دهد و در نتیجه منابع پردازشی هدر می‌رود. 

نمونه‌های رایج I/O عبارتند از:

* بازیابی یا ذخیره‌سازی داده‌ها در پایگاه داده یا هر نوع حافظه دائمی.
* ارسال درخواست به یک سرویس وب.
* ارسال یا دریافت پیام از صف پیام.
* نوشتن یا خواندن از یک فایل محلی.

این ضدالگو معمولا به دلایل زیر رخ می‌دهد:

* به نظر می‌رسد این روش، بدیهی‌ترین راه برای انجام یک عملیات است.
* برنامه نیازمند پاسخی برای درخواستی که ارسال کرده است.
* برنامه از کتابخانه‌ای استفاده می‌کند که فقط متدهای همزمان برای I/O ارائه می‌دهد.
* یک کتابخانه خارجی در درون خود عملیات I/O همزمان انجام می‌دهد. تنها یک فراخوانی I/O همزمان می‌تواند کل زنجیره فراخوانی را بلوکه کند.





