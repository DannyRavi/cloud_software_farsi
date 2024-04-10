
----------------------------------
helper services
در علوم رایانه، "سرویس‌های کمکی" یا "helper services" به خدمات و سرویس‌هایی اشاره دارد که برنامه‌ها یا سیستم‌های رایانه‌ای را در انجام وظایف و عملیات مختلفی کمک می‌کنند. این خدمات معمولاً به عنوان قسمتی از یک برنامه یا سیستم بزرگتر ارائه می‌شوند و معمولاً به صورت پشتیبانی و کمک فنی برای عملکرد بهتر برنامه یا سیستم استفاده می‌شوند. به عنوان مثال، خدمات کمکی می‌توانند شامل سیستم‌های پشتیبانی، پردازش پس‌زمینه، خدمات رمزگذاری و رمزنگاری، مدیریت حافظه، خدمات شبکه، و غیره باشند.

------------
[daemon](https://en.wikipedia.org/wiki/Daemon_(computing))

در [سیستم‌عامل‌های](https://fa.wikipedia.org/wiki/%D8%B3%DB%8C%D8%B3%D8%AA%D9%85%E2%80%8C%D8%B9%D8%A7%D9%85%D9%84 "سیستم‌عامل") با قابلیت [چندوظیفگی](https://fa.wikipedia.org/wiki/%DA%86%D9%86%D8%AF%DA%A9%D8%A7%D8%B1%DA%AF%DB%8C "چندکارگی")، یک **دیمِن** (به [انگلیسی](https://fa.wikipedia.org/wiki/%D8%B2%D8%A8%D8%A7%D9%86_%D8%A7%D9%86%DA%AF%D9%84%DB%8C%D8%B3%DB%8C "زبان انگلیسی"): Daemon) یک [برنامه](https://fa.wikipedia.org/wiki/%D8%A8%D8%B1%D9%86%D8%A7%D9%85%D9%87_(%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "برنامه (رایانه)") است که به جای اینکه تحت کنترل مستقیم یک کاربر تعاملی باشد، در [پس‌زمینه](https://fa.wikipedia.org/w/index.php?title=%D9%BE%D8%B3%E2%80%8C%D8%B2%D9%85%DB%8C%D9%86%D9%87_(%D8%A8%D8%B1%D9%86%D8%A7%D9%85%D9%87_%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87)&action=edit&redlink=1 "پس‌زمینه (برنامه رایانه) (صفحه وجود ندارد)") اجرا می‌شود. به‌طور سنتی نام دیمن‌ها با حرف d خاتمه می‌یابد. به عنوان مثال، [syslogd](https://fa.wikipedia.org/w/index.php?title=Syslogd&action=edit&redlink=1 "Syslogd (صفحه وجود ندارد)") دیمنی است که قابلیت ثبت رخداد در [سیستم‌عامل‌های](https://fa.wikipedia.org/wiki/%D8%B3%DB%8C%D8%B3%D8%AA%D9%85%E2%80%8C%D8%B9%D8%A7%D9%85%D9%84 "سیستم‌عامل") [شبه یونیکس](https://fa.wikipedia.org/wiki/%D8%B4%D8%A8%D9%87_%DB%8C%D9%88%D9%86%DB%8C%DA%A9%D8%B3 "شبه یونیکس") را پیاده‌سازی می‌کند و برنامه‌های کاربردی به کمک این دیمن اطلاعاتی را در فایل‌های ثبت رخداد خود می‌نویسند. یا همچنین **[اوپن‌اس‌اس‌اچ](https://fa.wikipedia.org/wiki/%D8%A7%D9%88%D9%BE%D9%86%E2%80%8C%D8%A7%D8%B3%E2%80%8C%D8%A7%D8%B3%E2%80%8C%D8%A7%DA%86 "اوپن‌اس‌اس‌اچ")** دیمنی است که در پس‌زمینه سیستم منتظر اتصالات ورودی [اس‌اس‌اچ](https://fa.wikipedia.org/wiki/%D8%A7%D8%B3%E2%80%8C%D8%A7%D8%B3%E2%80%8C%D8%A7%DA%86 "اس‌اس‌اچ") می‌ماند و آن‌ها را اجابت می‌کند. در [سیستم‌عامل‌های](https://fa.wikipedia.org/wiki/%D8%B3%DB%8C%D8%B3%D8%AA%D9%85%E2%80%8C%D8%B9%D8%A7%D9%85%D9%84 "سیستم‌عامل") [یونیکس](https://fa.wikipedia.org/wiki/%DB%8C%D9%88%D9%86%DB%8C%DA%A9%D8%B3 "یونیکس") و [شبه یونیکس](https://fa.wikipedia.org/wiki/%D8%B4%D8%A8%D9%87_%DB%8C%D9%88%D9%86%DB%8C%DA%A9%D8%B3 "شبه یونیکس")، [فرایند والد](https://fa.wikipedia.org/wiki/%D9%81%D8%B1%D8%A7%DB%8C%D9%86%D8%AF_%D9%88%D8%A7%D9%84%D8%AF "فرایند والد") یک دیمن، معمولاً، اما نه همیشه، فرایندی به نام [اینیت](https://fa.wikipedia.org/wiki/%D8%A7%DB%8C%D9%86%DB%8C%D8%AA "اینیت") است. یک دیمن معمولاً به این صورت ایجاد می‌شود که یک [فرایند](https://fa.wikipedia.org/wiki/%D9%81%D8%B1%D8%A7%DB%8C%D9%86%D8%AF_(%D8%B9%D9%84%D9%88%D9%85_%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "فرایند (علوم رایانه)")، فرایند فرزندی را [منشعب](https://fa.wikipedia.org/wiki/%D8%A7%D9%86%D8%B4%D8%B9%D8%A7%D8%A8_(%D8%B3%DB%8C%D8%B3%D8%AA%D9%85%E2%80%8C%D8%B9%D8%A7%D9%85%D9%84) "انشعاب (سیستم‌عامل)") کرده و سپس بلافاصله خارج می‌شود تا باعث شود اینیت فرایند فرزند تولید شده را مال خود کند. به علاوه، دیمن یا سیستم‌عامل باید کارهای دیگری را هم انجام دهد، مثلاً باید دیمن مورد نظر از کنترل هر ترمینالی خارج شود و به هیچ ترمینالی وابسته نباشد. چرا که دیمن قرار است در پس‌زمینه به اجرا درآید و قرار نیست با کاربر به صورت تعاملی ارتباط برقرار کند. به منظور انجام دادن راحت‌تر این کارها، بیشتر سیستم‌عامل‌های یونیکس توابع و رویه‌هایی مانند daemon(3) را پیاده‌سازی کرده‌اند که عملیات فوق را خیلی آسانتر می‌کنند. در اکثر سیستم‌ها، دیمن‌ها اغلب در هنگام بوت شدن سیستم آغاز به کار می‌کنند و خدماتی نظیر پاسخگویی به درخواست‌های شبکه، فعالیت‌های سخت‌افزاری و … را ارائه می‌دهند.

منبع:https://fa.wikipedia.org/wiki/دیمن

-----------------
Computer cluster
**خوشهٔ کامپیوتر** (به [انگلیسی](https://fa.wikipedia.org/wiki/%D8%B2%D8%A8%D8%A7%D9%86_%D8%A7%D9%86%DA%AF%D9%84%DB%8C%D8%B3%DB%8C "زبان انگلیسی"): Computer cluster) نوعی از سیستم‌های [پردازش موازی](https://fa.wikipedia.org/wiki/%D9%BE%D8%B1%D8%AF%D8%A7%D8%B2%D8%B4_%D9%85%D9%88%D8%A7%D8%B2%DB%8C "پردازش موازی") و توزیع شده‌است، که متشکل از مجموعه‌ای از رایانه‌های مستقل می‌باشد که تمام این گره‌ها در ارتباط تنگاتنگ با کل سیستم هستند و به عنوان یک منبع یکپارچه کار می‌کنند.

https://fa.wikipedia.org/wiki/خوشه_کامپیوتر

--------------------

Scaling out؟؟؟

"Scaling out" یا "مقیاس‌پذیری به سمت بیرون" به فرآیند افزایش ظرفیت یا عملکرد یک برنامه یا سیستم با افزودن منابع سخت‌افزاری یا نرم‌افزاری به آن اشاره دارد. این روش از "مقیاس‌پذیری به سمت بالا" یا "Scaling up" متفاوت است که در آن منابع سخت‌افزاری یا نرم‌افزاری موجود افزایش می‌یابد.

هنگامی که یک برنامه یا سیستم به مقیاس‌پذیری به سمت بیرون تکیه می‌کند، این بدان معناست که به جای افزایش قدرت محاسباتی یا منابع سخت‌افزاری بر روی یک سرور موجود، می‌توان سرور‌های جدید را به شبکه اضافه کرد و بار کاری را بین آن‌ها توزیع کرد. این افزایش ظرفیت معمولاً باعث افزایش مقیاس‌پذیری، انعطاف‌پذیری، و قابلیت اطمینان بیشتر برنامه یا سیستم می‌شود.

به عنوان مثال، در محیط‌های وب، مقیاس‌پذیری به سمت بیرون معمولاً با افزودن سرور‌های جدید به لایه‌های بارزنی (load balancing) و توزیع بار کاری بین آن‌ها صورت می‌گیرد تا بتوان تعداد کاربران بیشتری را پذیرفت و خدمات بهبود یافته‌ای ارائه داد.

----------------
Hosting stack؟؟؟

"Hosting stack" به مجموعه‌ای از فناوری‌ها، نرم‌افزارها، و سرویس‌های مورد استفاده برای ارائه و مدیریت سایت‌ها و برنامه‌های وب روی یک سرور اشاره دارد. این "پشته" (stack) معمولاً از چندین لایه یا سطح مختلف تشکیل شده است که هر کدام وظایف مشخصی را بر عهده دارند. به عنوان مثال، یک Hosting Stack ممکن است شامل موارد زیر باشد:

1. سیستم عامل سرور: مانند لینوکس، ویندوز سرور، یا برنامه‌های مدیریت سرور مانند cPanel یا Plesk.
2. نرم‌افزار سرور وب: مانند Apache، Nginx، یا Microsoft IIS که برای ارسال وب‌صفحات به مرورگرها استفاده می‌شوند.
3. زبان برنامه‌نویسی وب: مانند PHP، Python، Ruby، یا Node.js برای تولید دینامیک صفحات وب.
4. پایگاه داده: مانند MySQL، PostgreSQL، MongoDB، یا Microsoft SQL Server برای ذخیره و بازیابی داده‌ها.
5. فریم‌ورک‌ها و ابزار توسعه: مانند Laravel، Django، Flask، یا ASP.NET برای تسهیل فرایند توسعه وب‌سایت‌ها و برنامه‌ها.
6. سرویس‌های ابری: مانند Amazon Web Services (AWS)، Microsoft Azure، یا Google Cloud Platform (GCP) که امکانات مختلفی از جمله ذخیره‌سازی، محاسبات، و ارائه سرویس‌های شبکه را فراهم می‌کنند.

این موارد تنها یک نمونه از عناصری هستند که ممکن است در یک Hosting Stack مورد استفاده قرار گیرند، و بسته به نیازها و اولویت‌های خاص هر پروژه، ممکن است این لایه‌ها متفاوت باشند.

-------------
الگوی نما -  Facade

**الگوی نما** (فساد): یک الگوی ساختاری در [الگوهای طراحی نرم‌افزار](https://fa.wikipedia.org/wiki/%D8%A7%D9%84%DA%AF%D9%88%DB%8C_%D8%B7%D8%B1%D8%A7%D8%AD%DB%8C_(%D8%AF%D8%A7%D9%86%D8%B4_%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "الگوی طراحی (دانش رایانه)") می‌باشد که معمولاً در [برنامه‌نویسی شی گرا](https://fa.wikipedia.org/wiki/%D8%A8%D8%B1%D9%86%D8%A7%D9%85%D9%87%E2%80%8C%D9%86%D9%88%DB%8C%D8%B3%DB%8C_%D8%B4%DB%8C%D8%A1%DA%AF%D8%B1%D8%A7 "برنامه‌نویسی شیءگرا") از آن استفاده می‌شود. نام آن برگرفته از شباهت آن به مشابه آن در معماری ساختمان نما (ساختمان) می‌باشد.

نما، شیءای است که یک رابط راحت برای دسترسی به قسمت بزرگ و پیچیده‌ای از کد می‌باشد. مثل [کتابخانه کلاس‌ها](https://fa.wikipedia.org/wiki/%DA%A9%D8%AA%D8%A7%D8%A8%D8%AE%D8%A7%D9%86%D9%87_(%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "کتابخانه (رایانه)"). فساد می‌تواند:

- استفاده از یک [کتابخانه نرم‌افزاری](https://fa.wikipedia.org/wiki/%DA%A9%D8%AA%D8%A7%D8%A8%D8%AE%D8%A7%D9%86%D9%87_(%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "کتابخانه (رایانه)") را آسان‌تر کند، بفهمد و آن را تست کند. چرا که توابع راحتی برای عملیات‌های عادی دارد.
- به دلایل مشابهی، خواندن از کتابخانه را راحت‌تر و امکان‌پذیرتر می‌کند.
- کاهش [وابستگی](https://fa.wikipedia.org/wiki/%D8%AC%D9%81%D8%AA%DA%AF%D8%B1%DB%8C_(%D8%AF%D8%A7%D9%86%D8%B4_%D8%B1%D8%A7%DB%8C%D8%A7%D9%86%D9%87) "جفتگری (دانش رایانه)") به کدهای خارجی مهم‌ترین وظیفه یک کتابخانه داخلی است، چرا که بیشتر قسمت‌های کد از نما استفاده می‌کنند، باعث تغییرپذیری بیشتر در طراحی سیستم می‌شود.
- بسته مجموعه‌ای از [رابط‌های برنامه کاربری](https://fa.wikipedia.org/wiki/%D8%B1%D8%A7%D8%A8%D8%B7_%D8%A8%D8%B1%D9%86%D8%A7%D9%85%D9%87%E2%80%8C%D9%86%D9%88%DB%8C%D8%B3%DB%8C_%DA%A9%D8%A7%D8%B1%D8%A8%D8%B1%D8%AF%DB%8C "رابط برنامه‌نویسی کاربردی") با طراحی ضعیف، تنها با یک رابط برنامه کاربری ([API](https://fa.wikipedia.org/wiki/API "API")) که از طراحی خوبی برخوردار است.

معمولاً وقتی از [الگوی طراحی](https://fa.wikipedia.org/wiki/%D8%A7%D9%84%DA%AF%D9%88%DB%8C_%D8%B7%D8%B1%D8%A7%D8%AD%DB%8C "الگوی طراحی") نما استفاده می‌شود که سامانه از پیچیدگی زیادی برخوردار است یا فهمیدن آن دشوار است، به خاطر آن که تعداد زیادی از کلاس‌های دارای وابستگی داخلی یا کلاس‌هایی که کد آنها در دسترس نباشد وجود داشته باشند. این الگو پیچیدگی یک سامانه بزرگ را مخفی کرده و یک رابط ساده برای مشتری فراهم می‌کند. معمولاً دارای یک کلاس پوشه‌بندی ساده است که مجموعه‌ای از عضوهای مورد نیاز مشتری در آن وجود دارند. این اعضا به جای مشتری نما، به سامانه دسترسی دارند و نحوه پیاده‌سازی را مخفی می‌کنند.


-----------------
 Connection pool
In [software engineering](https://en.wikipedia.org/wiki/Software_engineering "Software engineering"), a **connection pool** is a [cache](https://en.wikipedia.org/wiki/Database_cache "Database cache") of [database connections](https://en.wikipedia.org/wiki/Database_connection "Database connection") maintained so that the connections can be reused when future requests to the database are required.[[1]](https://en.wikipedia.org/wiki/Connection_pool#cite_note-Pugh-1) Connection pools are used to enhance the performance of executing commands on a database. Opening and maintaining a database connection for each user, especially requests made to a dynamic database-driven [website](https://en.wikipedia.org/wiki/Website "Website") application, is costly and wastes resources. In connection pooling, after a connection is created, it is placed in the pool and it is used again so that a new connection does not have to be established. If all the connections are being used, a new connection is made and is added to the pool. Connection pooling also cuts down on the amount of time a user must wait to establish a connection to the database.

-------------------------

 Thread pool
 
In [computer programming](https://en.wikipedia.org/wiki/Computer_programming "Computer programming"), a **thread pool** is a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern "Software design pattern") for achieving [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science) "Concurrency (computer science)") of execution in a computer program. Often also called a **replicated workers** or **worker-crew model**,[[1]](https://en.wikipedia.org/wiki/Thread_pool#cite_note-1) a thread pool maintains multiple [threads](https://en.wikipedia.org/wiki/Thread_(computer_science) "Thread (computer science)") waiting for [tasks](https://en.wikipedia.org/wiki/Task_(computers) "Task (computers)") to be allocated for [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing "Concurrent computing") execution by the supervising program. By maintaining a pool of threads, the model increases performance and avoids latency in execution due to frequent creation and destruction of threads for short-lived tasks.[[2]](https://en.wikipedia.org/wiki/Thread_pool#cite_note-2) The number of available threads is tuned to the computing resources available to the program, such as a parallel task queue after completion of execution.

https://en.wikipedia.org/wiki/Thread_pool

![[Pasted image 20240409133239.png]]

----------------
 Serverless computing
 
**Serverless computing** is a [cloud computing](https://en.wikipedia.org/wiki/Cloud_computing "Cloud computing") [execution model](https://en.wikipedia.org/wiki/Execution_model "Execution model") in which the cloud provider allocates machine resources on demand, taking care of the [servers](https://en.wikipedia.org/wiki/Server_(computing) "Server (computing)") on behalf of their customers. "Serverless" is a [misnomer](https://en.wikipedia.org/wiki/Misnomer "Misnomer") in the sense that servers are still used by cloud service providers to execute code for developers. However, developers of serverless applications are not concerned with [capacity planning](https://en.wikipedia.org/wiki/Capacity_planning "Capacity planning"), configuration, management, maintenance, [fault tolerance](https://en.wikipedia.org/wiki/Fault_tolerance "Fault tolerance"), or scaling of containers, [VMs](https://en.wikipedia.org/wiki/Virtual_machine "Virtual machine"), or physical servers. Serverless computing does not hold resources in [volatile memory](https://en.wikipedia.org/wiki/Volatile_memory "Volatile memory"); computing is rather done in short bursts with the results persisted to storage. When an app is not in use, there are no computing resources allocated to the app. Pricing is based on the actual amount of resources consumed by an application.[[1]](https://en.wikipedia.org/wiki/Serverless_computing#cite_note-techcrunch-lambda-1) It can be a form of [utility computing](https://en.wikipedia.org/wiki/Utility_computing "Utility computing").

Serverless computing can simplify the process of [deploying code](https://en.wikipedia.org/wiki/Software_deployment "Software deployment") into production. Serverless code can be used in conjunction with code deployed in traditional styles, such as [microservices](https://en.wikipedia.org/wiki/Microservices "Microservices") or [monoliths](https://en.wikipedia.org/wiki/Monolithic_application "Monolithic application"). Alternatively, applications can be written to be purely serverless and use no provisioned servers at all.[[2]](https://en.wikipedia.org/wiki/Serverless_computing#cite_note-lambda-api-gateway-2) This should not be confused with computing or networking models that do not require an actual server to function, such as [peer-to-peer](https://en.wikipedia.org/wiki/Peer-to-peer "Peer-to-peer") (P2P).

https://en.wikipedia.org/wiki/Serverless_computing

-----------------
blob storage - object storage

**Object storage** (also known as **object-based storage**[[1]](https://en.wikipedia.org/wiki/Object_storage#cite_note-1) or **blob storage**) is a [computer data storage](https://en.wikipedia.org/wiki/Computer_data_storage "Computer data storage") approach that manages data as "blobs" or "objects", as opposed to other storage architectures like [file systems](https://en.wikipedia.org/wiki/File_systems "File systems") which manages data as a file hierarchy, and [block storage](https://en.wikipedia.org/wiki/Block_storage "Block storage") which manages data as blocks within sectors and tracks.[[2]](https://en.wikipedia.org/wiki/Object_storage#cite_note-2) Each object is typically associated with a variable amount of [metadata](https://en.wikipedia.org/wiki/Metadata "Metadata"), and a [globally unique identifier](https://en.wikipedia.org/wiki/Globally_unique_identifier "Globally unique identifier"). Object storage can be implemented at multiple levels, including the device level (object-storage device), the system level, and the interface level. In each case, object storage seeks to enable capabilities not addressed by other storage architectures, like interfaces that are directly programmable by the application, a namespace that can span multiple instances of physical hardware, and data-management functions like [data replication](https://en.wikipedia.org/wiki/Data_replication "Data replication") and data distribution at object-level granularity.


-----------------


 data store 
 
نکته: یک ذخیره ساز یا ذخیره‌گاه یا data store یک مخزن دیجیتالی است که اطلاعات را در سیستم‌های کامپیوتری ذخیره و حفظ می‌کند. یک data store می‌تواند یک ذخیره‌سازی متصل به شبکه، یک ذخیره‌سازی ابری توزیع‌شده، یک درایو سخت فیزیکی یا یک ذخیره‌سازی مجازی باشد. این گزینه می‌تواند هم اطلاعات ساختاریافته مانند جداول اطلاعات و هم اطلاعات غیرساختاریافته مانند ایمیل‌ها، تصاویر و ویدیوها را ذخیره کند. [سازمان‌ها از data store برای نگهداری، به اشتراک گذاری و مدیریت اطلاعات در سطح واحدهای کسب و کار استفاده می‌کنند](https://en.wikipedia.org/wiki/Data_store)

--------------


-----------------------------
on-premises:

نرم‌افزار داخلی (به اختصار on-prem، یا به عنوان on-premise نامیده می‌شود) روی رایانه‌ها در محل شخصی یا سازمانی که از نرم‌افزار استفاده می‌کند، نصب و اجرا می‌شود، نه در یک مرکز ریموت مانند یک سرور. [server farm](https://en.wikipedia.org/wiki/Server_farm "Server farm") یا محیط ابری نرم‌افزار داخلی گاهی اوقات به عنوان نرم‌افزار «[shrinkwrap](https://en.wikipedia.org/wiki/Shrink_wrap_contract "Shrink wrap contract")» نامیده می‌شود و نرم‌افزار خارج از محل معمولاً «نرم‌افزار به‌عنوان سرویس» یا [software as a service](https://en.wikipedia.org/wiki/Software_as_a_service "Software as a service") («SaaS») یا «محاسبات ابری» ([cloud computing](https://en.wikipedia.org/wiki/Cloud_computing)) نامیده می‌شود.  

نرم افزار متشکل از پایگاه داده و ماژول هایی است که به طور خاص به نیازهای منحصر به فردی یا سازمانهای بزرگ در مورد اتوماسیون سیستم تجاری در سطح شرکت و کارایی آن‌ها پاسخ می‌دهند.

-----------------

داده‌ها، داده‌ها در همه جا - وقتی از مقیاس‌پذیری صحبت می‌کنیم، درباره آن صحبت می‌کنیم  
مقیاس پذیری در رایانش ابری توانایی افزایش یا کاهش سریع و آسان اندازه یا قدرت راه حل یا منبع فناوری اطلاعات است. در حالی که اصطلاح مقیاس‌پذیری می‌تواند به توانایی هر سیستمی برای رسیدگی به حجم فزاینده‌ای از کار اشاره کند، زمانی که ما در مورد اینکه آیا مقیاس‌پذیری در مقابل مقیاس‌پذیری را افزایش دهیم، صحبت می‌کنیم، اغلب به پایگاه‌های داده و داده‌ها و بسیاری از آن‌ها اشاره می‌کنیم.  
  
مقیاس‌پذیری پایگاه داده برای توسعه‌دهندگان برنامه‌های مدرن مورد توجه است. فرض کنید یک برنامه جدید شروع به کار کند—و تقاضا برای آن از تعداد انگشت شماری کاربر به میلیون ها کاربر در سراسر جهان افزایش می یابد. یکی از مهم‌ترین توانایی‌هایی که به توسعه‌دهندگان اپلیکیشن کمک می‌کند تا با تقاضا همگام شوند و زمان خرابی را به حداقل برسانند، توانایی مقیاس‌پذیری کارآمد است.  
  
این مکالمه در مورد کوچک‌سازی در مقابل افزایش مقیاس بر روی روش‌هایی تمرکز می‌کند که مقیاس‌پذیری به ما کمک می‌کند تا حجم بسیار زیاد و مجموعه وسیعی از داده‌ها، تغییر حجم داده‌ها و تغییر الگوهای بار کاری را تطبیق داده و مدیریت کنیم - همه از ابر، تلفن همراه، رسانه‌های اجتماعی، و اطلاعات بزرگ.