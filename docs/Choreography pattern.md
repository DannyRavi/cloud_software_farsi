
از هر یک از اجزای سیستم بخواهید به جای تکیه بر یک نقطه کنترل مرکزی، در فرآیند تصمیم گیری(decision-making) در مورد گردش کار(workflow) یک تراکنش تجاری (business transaction) شرکت کنند.

## Context and problem

در معماری میکروسرویس‌ها، اغلب این مورد اتفاق می‌افتد که یک application مبتنی بر معماری ابری(cloud-based) به چندین سرویس کوچک‌تر تقسیم می‌شود که با همدیگر کار می‌کنند تا یک تراکنش تجاری (business transaction) را به صورت end-to-end پردازش کنند. برای کاهش اتصال بین سرویس‌ها، هر سرویس مسئول یک عملیات business گونه به صورت واحد است. برخی از مزایای این روش عبارتند از:
توسعه سریعتر،کد بیس (code base) کوچکتر و مقیاس پذیری(scalability). با این حال، طراحی یک گردش کار(workflow) کارآمد و مقیاس پذیر(scalable) یک چالش بزرگ است و اغلب به ارتباطات بین سرویسی پیچیده نیاز دارد.  
  
سرویس ها با استفاده از API های تعریف شده با یکدیگر ارتباط برقرار می کنند. حتی یک عملیات تجاری(business operation) منفرد می تواند منجر به چندین تماس نقطه به نقطه (point-to-point) در بین همه سرویس‌ها شود. یک الگوی رایج برای ارتباط این سرویس‌ها، استفاده از یک سرویس متمرکز است که به عنوان ارکستر کننده(orchestrator) عمل می کند. تمام درخواست های دریافتی را تایید می کند و عملیات را به سرویس های مربوطه واگذار می کند. با انجام این کار، گردش کار کل business transaction را نیز مدیریت می کند. هر سرویس فقط یک عملیات را تکمیل می کند و از گردش کار(workflow) کلی آگاه نیست.  
  
الگوی ارکستراتور(orchestrator) ارتباط نقطه به نقطه بین سرویس ها را کاهش می دهد، اما به دلیل اتصال تنگاتنگ(tight coupling) بین ارکستراتور(orchestrator) و سایر سرویس‌هایی که در پردازش business transaction شرکت می کنند، دارای اشکالاتی است. برای اجرای کارها در یک توالی، ارکستراتور نیاز به اطلاعات  در مورد وظایف آن سرویس‌ها دارد. اگر می‌خواهید سرویس‌هایی را اضافه یا حذف کنید، منطق موجود دچار خطا می‌شود و باید بخش‌هایی از مسیر ارتباطی را دوباره بازسازی کنید. در حالی که می توانید گردش کار را پیکربندی(configure) کنید و سرویس‌ها را به راحتی با یک ارکستراتور که به خوبی طراحی شده است را اضافه یا حذف کنید، هر چند که چنین پیاده سازی پیچیده و نگهداری آن سخت است.

![[Pasted image 20231203232640.png]]

## Solution

اجازه دهید هر سرویس تصمیم بگیرد که یک عملیات کسب و کار (business operation) چه زمانی و چگونه پردازش می شود، به جای اینکه به یک ارکستراتور مرکزی وابسته باشد.  
  
یکی از راه‌های پیاده‌سازی choreography، استفاده از الگوی [asynchronous messaging pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber) برای هماهنگ کردن business operation است.

![[Pasted image 20231203232705.png]]

درخواست client  پیام ها را در صف پیام منتشر می کند. با رسیدن پیام ها به مشترکین(subscribers) یا سرویس‌هایی که به آن پیام نیازمند هستند، هدایت می شوند. هر سرویس  از نوع subscribe، عملیات خود را همانطور که در پیام نشان داده شده است انجام می دهد و با موفقیت یا شکست شدن عملیات به message queue پاسخ می دهد. در صورت موفق بودن عملیات، سرویس می تواند پیامی را به همان صف یا message queue دیگری برگرداند تا در صورت نیاز سرویس دیگری بتواند گردش کار(workflow) را ادامه دهد. اگر عملیاتی با شکست مواجه شود، گذرگاه پیام(message bus) می تواند آن عملیات را دوباره تکرار کند.  
  
به این ترتیب، سرویس ها بدون وابستگی به ارکستراتور یا ارتباط مستقیم بین آنها، workflow را بین خود طراحی می کنند.  
  
از آنجایی که ارتباط نقطه به نقطه وجود ندارد، این الگو به کاهش اتصال بین سرویس‌ها کمک می کند. همچنین، می تواند گلوگاه عملکرد ایجاد شده توسط ارکستراتور را در زمانی که باید با تمام تراکنش ها سر و کار داشته باشد، برطرف کند.


## When to use this pattern

اگر انتظار دارید سرویس‌ها را مرتباً به‌روزرسانی یا جایگزین کنید، و در نهایت برخی از سرویس‌ها را اضافه یا حذف کنید، از الگوی choreography استفاده کنید. کل برنامه را می توان با تلاش کمتر و حداقل اختلال در سرویس‌های موجود تغییر داد.  
  
اگر با گلوگاه(bottleneck) اجرایی در ارکستراتور مرکزی مواجه شدید، این الگو را در نظر بگیرید.  
  
این الگو یک مدل طبیعی برای معماری بدون سرور(serverless) است که در آن همه سرویس‌ها می‌توانند کوتاه مدت(short lived) یا رویداد محور(event driven) باشند. سرویس‌ها می‌توانند به‌ دلیل یک رویداد فعال شوند، وظیفه خود را انجام دهند و پس از پایان کار حذف شوند.

## Issues and considerations

غیرمتمرکز کردن ارکستراتور می تواند باعث ایجاد مشکلاتی در هنگام مدیریت گردش کار شود.  
  
اگر یک سرویس نتواند یک business operation را تکمیل کند، بازیابی آن شکست می تواند دشوار باشد. یکی از راه‌ها این است که سرویس با فعال سازی یک رخداد، حالت شکست را نشان دهد. سرویس دیگری که در آن رویدادهای ناموفق مشترک(subscribes) می شود، اقدامات لازم را انجام می دهد، مانند اعمال تراکنش های جبرانی( [compensating transactions](https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction)) برای بازگشت به شرایط قبل عملیات موفق در یک request. سرویس ناموفق ممکن است رویدادی را به دلیل خرابی فعال کند. در آن صورت، استفاده از مکانیسم تلاش مجدد(retry) و یا time out ، آن را برای تشخیص آن که عملیات مورد نظر به عنوان یک شکست برداشت می‌شود را در نظر بگیرید. برای مثال به بخش [Example](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography#example) مراجعه کنید.  
  
هنگامی که می خواهید business operation مستقل را به صورت موازی پردازش کنید، پیاده سازی یک workflow کار ساده‌ای است. می توانید از یک message bus استفاده کنید. با این حال، زمانی که طراحی الگوی choreography باید در یک توالی اتفاق بیفتد پس workflow می تواند پیچیده‌تر شود. به عنوان مثال، سرویس C تنها پس از اینکه سرویس A و سرویس B عملیات خود را با موفقیت کامل کردند، می تواند عملیات خود را آغاز کند. یک رویکرد این است که message bus پیام داشته باشید که پیام ها را به ترتیب مورد نیاز دریافت می کنند. برای اطلاعات بیشتر به بخش [Example](https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography#example) مراجعه کنید.  
  
اگر تعداد سرویس‌ها به سرعت رشد کند، الگوی choreography به یک چالش تبدیل می شود. با توجه به تعداد بالای قطعات متحرک مستقل(independent moving parts)  ک در نتیجه باعث workflow پیچیده‌ تر بین سرویس‌ها  می‌شود. همچنین ردیابی خطاها  را  مشکل تر  می کند.  
  
ارکستراتور به طور متمرکز انعطاف پذیری workflow را مدیریت می کند و می تواند به یک نقطه شکست تبدیل شود. از سوی دیگر، در الگوی choreography، وظایف بین همه سرویس‌ها توزیع می شود و انعطاف پذیری توسعه نرم‌افزار افزایش می یابد.  
  
هر سرویس نه تنها مسئول انعطاف پذیری(resiliency) عملیات خود است، بلکه همچنین وظیفه workflow را نیز بر عهده دارد. این وظیفه می تواند برای سرویس‌ها تا حدی سنگین باشد و استفاده از آن را سخت کند. هر سرویس باید حالت‌های ناگذرا، گذرا و time-out را چندین بار امتحان کند تا requestهای آن به‌ خوبی خاتمه یابد. همچنین سرویس‌ها  باید نسبت به اطلاع رسانی موفقیت یا عدم موفقیت عملیات توانایی داشته باشند تا بقیه سرویس ها بر این اساس به درستی کار کنند و وظایف خود رو ادامه دهند.


## Example

این مثال الگوی choreography را با برنامه تحویل هواپیماهای بدون سرنشین( [Drone Delivery app](https://github.com/mspnp/microservices-reference-implementation)) نشان می دهد. وقتی کلاینت درخواست مورد نظر را خود را انتخاب می کند ، برنامه یک هواپیمای بدون سرنشین را به اون اختصاص می دهد و به کلاینت اطلاع می دهد.  
  
نمونه ای از این الگوی در [GitHub](https://github.com/mspnp/cloud-design-patterns/tree/master/choreography) موجود است.

![[Pasted image 20231203232837.png]]

یک تراکنش تجاری(business transaction)  از  سمت یک کلاینت به 3 نوع business operation متمایز نیاز دارد: 
- ایجاد یا به‌روزرسانی یک بسته 
- اختصاص یک هواپیمای بدون سرنشین برای تحویل بسته  
- بررسی وضعیت تحویل.

این عملیات توسط سه میکروسرویس انجام می‌شود: بسته، زمان‌بندی هواپیمای بدون سرنشین و خدمات تحویل بسته. به جای ارکستراتور مرکزی، سرویس ها از پیام برای همکاری و هماهنگ کردن درخواست بین خود استفاده می کنند.

### Design

تراکنش تجاری(business transaction) به صورت متوالی از طریق hop های متعدد پردازش می شود. هر hop دارای یک message bus و business service مربوطه است.  
  
هنگامی که یک کلاینت درخواست تحویل(delivery request) را از طریق یک  HTTP endpoint ارسال می کند، سرویس Ingestion آن را دریافت می کند، یک رویداد عملیاتی را فعال می کند و آن را به یک message bus ارسال می کند. این bus در واقع subscribed business شده را فراخوانی می کند و رویداد را در یک request از نوع POST ارسال می کند. با دریافت رویداد، business service می تواند عملیات را با موفقیت یا شکست انجام دهد و اگر برنامه دچار time out شود، عملیات به پایان می رسد. در صورت موفقیت آمیز بودن عملیات، سرویس با Ok status code به bus پاسخ می دهد، در ادامه رویداد(event) عملیات جدیدی را فعال می کند و آن را به message bus مربوط به hop بعدی ارسال می کند. در صورت وقوع خرابی یا وقفه در time-out، سرویس با ارسال کد BadRequest به message bus که درخواست اصلی POST را ارسال کرده است، مشکل را گزارش می کند. message bus عملیات را بر اساس روش retry دوباره انجام می دهد. پس از انقضای  timeout در این عملیات، message bus، عملیات ناموفق را علامت گذاری می‌کند و پردازش باقی request متوقف می‌شود.  
  
این workflow تا زمانی که همه request ها پردازش شود ادامه می یابد.  
  
این طرح از چندین message bus برای پردازش کل business transaction استفاده می کند. همین طور [Microsoft Azure Event Grid](https://learn.microsoft.com/en-us/azure/event-grid/)  سرویس پیام رسانی را ارائه می دهد. این برنامه در یک  [Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/) cluster با [two containers in the same pod](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/#creating-a-pod-that-runs-two-containers) مستقر شده است. یک کانتینر [ambassador](https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador) را اجرا می کند که با Event Grid تعامل دارد در حالی که دیگری یک business service را اجرا می کند. رویکرد با دو کانتینر در یک pod قابلیت های  اجرایی و مقیاس پذیری را بهبود می بخشد. ambassador و business service معمولا  network یکسانی را  با هم به اشتراک می گذارند که امکان تأخیر کم و توان عملیاتی بالا را فراهم می کند.  
  
برای جلوگیری از retry متعدد، فقط Event Grid به‌جای سرویس business، یک عملیات را دوباره تکرار می‌کند. با ارسال یک پیام به یک  [dead letter queue (DLQ)](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dead-letter-queues)یک درخواست ناموفق را علامت گذاری می کند.  
  
سرویس business نمی تواند متوجه این موضوع شود که retry مختلف از یک منبع مشخص تکراری است یا نه.  به عنوان مثال، سرویس Package از عملیات upsert برای افزودن داده ها به data store استفاده می کند.  
  
این مثال یک راه حل custom شده را برای ارتباط بین تماس ها در همه سرویس ها و پرش های Event Grid پیاده سازی می کند.  
  
در اینجا یک نمونه کد وجود دارد که الگوی choreography بین تمام business serviceها را نشان می دهد. این کد گردش کار  Drone Delivery app transactions را نشان می دهد. کدهای مربوط به هندل کردن و لاگ خطاها برای اختصار حذف شده است.

```csharp
[HttpPost]
[Route("/api/[controller]/operation")]
[ProducesResponseType(typeof(void), 200)]
[ProducesResponseType(typeof(void), 400)]
[ProducesResponseType(typeof(void), 500)]

public async Task<IActionResult> Post([FromBody] EventGridEvent[] events)
{

   if (events == null)
   {
       return BadRequest("No Event for Choreography");
   }

   foreach(var e in events)
   {

        List<EventGridEvent> listEvents = new List<EventGridEvent>();
        e.Topic = eventRepository.GetTopic();
        e.EventTime = DateTime.Now;
        switch (e.EventType)
        {
            case Operations.ChoreographyOperation.ScheduleDelivery:
            {
                var packageGen = await packageServiceCaller.UpsertPackageAsync(delivery.PackageInfo).ConfigureAwait(false);
                if (packageGen is null)
                {
                    //BadRequest allows the event to be reprocessed by Event Grid
                    return BadRequest("Package creation failed.");
                }

                //we set the event type to the next choreography step
                e.EventType = Operations.ChoreographyOperation.CreatePackage;
                listEvents.Add(e);
                await eventRepository.SendEventAsync(listEvents);
                return Ok("Created Package Completed");
            }
            case Operations.ChoreographyOperation.CreatePackage:
            {
                var droneId = await droneSchedulerServiceCaller.GetDroneIdAsync(delivery).ConfigureAwait(false);
                if (droneId is null)
                {
                    //BadRequest allows the event to be reprocessed by Event Grid
                    return BadRequest("could not get a drone id");
                }
                e.Subject = droneId;
                e.EventType = Operations.ChoreographyOperation.GetDrone;
                listEvents.Add(e);
                await eventRepository.SendEventAsync(listEvents);
                return Ok("Drone Completed");
            }
            case Operations.ChoreographyOperation.GetDrone:
            {
                var deliverySchedule = await deliveryServiceCaller.ScheduleDeliveryAsync(delivery, e.Subject);
                return Ok("Delivery Completed");
            }
            return BadRequest();
    }
}
```

## Related resources

این الگوها را در طراحی خود برای choreography در نظر بگیرید.  
  
* با استفاده از e [ambassador design pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador)،  می‌توانید  business service را Modularize کنید.  
  
* الگوی  [queue-based load leveling pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling) را برای handle کردن به نوسانات workload به کار ببرید.  
  
* از پیام‌های توزیع شده ناهمزمان از طریق الگوی  [publisher-subscriber pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber) استفاده کنید.  
  
* از  [compensating transactions](https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction) برای خنثی سازی یک سری عملیات موفق در صورت شکست یک یا چند عملیات مرتبط استفاده کنید.  
  
* برای اطلاعات در مورد استفاده از message broker در زیرساخت پیام رسانی، به گزینه های [Asynchronous messaging options in Azure](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/messaging) مراجعه کنید.