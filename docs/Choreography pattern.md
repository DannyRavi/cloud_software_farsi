
# Choreography pattern

از هر یک از اجزای سیستم بخواهید به جای تکیه بر یک نقطه کنترل مرکزی، در فرآیند تصمیم گیری (decision-making) در مورد گردش کار یک تراکنش تجاری شرکت کنند.

## **زمینه و مشکل**

در معماری میکروسرویس‌ها، اغلب این مورد اتفاق می‌افتد که یک application مبتنی بر معماری ابری به چندین سرویس کوچک‌تر تقسیم می‌شود که با همدیگر کار می‌کنند تا یک تراکنش تجاری را از ابتدا تا انتهای (business transaction end-to-end) آن پردازش کنند. برای کاهش اتصال بین سرویس‌ها، هر سرویس مسئول یک عملیات تجاری گونه به صورت واحد است. برخی از مزایای این روش عبارتند از:

توسعه سریعتر،کد بیس (code base) کوچکتر و مقیاس پذیری(scalability). با این حال، طراحی یک گردش کار(workflow) کارآمد و مقیاس پذیر(scalable) یک چالش بزرگ است و اغلب به ارتباطات بین سرویسی پیچیده نیاز دارد.  
  
سرویس‌ها با استفاده از API‌های تعریف شده با یکدیگر ارتباط برقرار می‌کنند. حتی یک عملیات تجاری منفرد می‌تواند منجر به چندین تماس نقطه به نقطه در بین همه سرویس‌ها شود. یک الگوی رایج برای ارتباط این سرویس‌ها، استفاده از یک سرویس متمرکز است که به عنوان ارکستر کننده (orchestrator) عمل می‌کند. تمام درخواست‌های دریافتی را تایید می‌کند و عملیات را به سرویس‌های مربوطه واگذار می‌کند. با انجام این کار، گردش کار کل تراکنش تجاری را نیز مدیریت می‌کند. هر سرویس فقط یک عملیات را تکمیل می‌کند و از گردش کار(workflow) کلی آگاه نیست.  
  
الگوی ارکستراتور(orchestrator) ارتباط نقطه به نقطه بین سرویس‌ها را کاهش می‌دهد، اما به دلیل اتصال تنگاتنگ (tight coupling) بین ارکستراتور(orchestrator) و سایر سرویس‌‌هایی که در پردازش تراکنش تجاری شرکت می‌کنند، دارای اشکالاتی است. برای اجرای کارها در یک توالی، ارکستراتور نیاز به اطلاعات  در مورد وظایف آن سرویس‌ها دارد. اگر می‌خواهید سرویس‌‌هایی را اضافه یا حذف کنید، منطق موجود دچار خطا می‌شود و باید بخش‌‌هایی از مسیر ارتباطی را دوباره بازسازی کنید. در حالی که می‌توانید گردش کار را پیکربندی (configure) کنید و سرویس‌ها را به راحتی با یک ارکستراتور که به خوبی طراحی شده است را اضافه یا حذف کنید، هر چند که چنین پیاده سازی پیچیده و نگهداری آن سخت است.

![orchestrator](../assets/messaging/orchestrator.png)


## راه حل

اجازه دهید هر سرویس تصمیم بگیرد که یک عملیات تجاری (business operation) چه زمانی و چگونه پردازش می‌شود، به جای اینکه به یک ارکستراتور مرکزی وابسته باشد.  
  
یکی از راه‌‌های پیاده‌سازی choreography، استفاده از الگوی [asynchronous messaging pattern](./Publisher-Subscriber%20pattern.md) برای هماهنگ کردن عملیات تجاری است.

![choreography-pattern](../assets/messaging/choreography-pattern.png)


درخواست کلاینت  پیام‌ها را در صف پیام (message queue) منتشر می‌کند. با رسیدن پیام‌ها به مشترکین (subscribers) یا سرویس‌‌هایی که به آن پیام نیازمند هستند، هدایت و ارسال می‌شوند. هر سرویس  از نوع subscribe، عملیات خود را همانطور که در پیام نشان داده شده است انجام می‌دهد و با موفقیت یا شکست شدن عملیات به صف پیام یا message queue پاسخ می‌دهد. در صورت موفق بودن عملیات، سرویس می‌تواند پیامی را به همان صف یا message queue دیگری برگرداند تا در صورت نیاز سرویس دیگری بتواند گردش کار(workflow) را ادامه دهد. اگر عملیاتی با شکست مواجه شود، گذرگاه پیام(message bus) می‌تواند آن عملیات را دوباره تکرار کند.  
  
به این ترتیب، سرویس‌ها بدون وابستگی به ارکستراتور یا ارتباط مستقیم بین آنها، workflow را بین خود طراحی می‌کنند.  
  
از آنجایی که ارتباط نقطه به نقطه وجود ندارد، این الگو به کاهش اتصال بین سرویس‌ها کمک می‌کند. همچنین، می‌تواند گلوگاه عملکرد ایجاد شده توسط ارکستراتور را در زمانی که باید با تمام تراکنش‌ها سر و کار داشته باشد را برطرف کند.


## **چه زمانی از این الگو استفاده کنیم؟**

اگر انتظار دارید سرویس‌ها را مرتباً به‌روزرسانی یا جایگزین کنید و در نهایت برخی از سرویس‌ها را اضافه یا حذف کنید، از الگوی choreography استفاده کنید. کل برنامه را می‌توان با تلاش کمتر و حداقل اختلال در سرویس‌‌های موجود تغییر داد.  
  
اگر با گلوگاه(bottleneck) اجرایی در ارکستراتور مرکزی مواجه شدید، این الگو را در نظر بگیرید.  
  
این الگو یک مدل طبیعی برای معماری بدون سرور([serverless](https://en.wikipedia.org/wiki/Serverless_computing)) است که در آن همه سرویس‌ها می‌توانند کوتاه مدت (short lived) یا رویداد محور (event driven) باشند. سرویس‌ها می‌توانند به‌ دلیل یک رویداد فعال شوند، وظیفه خود را انجام دهند و پس از پایان کار حذف شوند.

## مسائل و ملاحظات:

غیرمتمرکز کردن ارکستراتور می‌تواند باعث ایجاد مشکلاتی در هنگام مدیریت گردش کار شود.  
  
اگر یک سرویس نتواند یک عملیات تجاری را تکمیل کند، بازیابی آن شکست می‌تواند دشوار باشد. یکی از راه‌ها این است که سرویس با فعال سازی یک رخداد، حالت شکست را نشان دهد. سرویس دیگری که در آن رویداد‌های ناموفق مشترک(subscribes) می‌شود، اقدامات لازم را انجام می‌دهد، مانند اعمال تراکنش‌های جبرانی( [compensating transactions](./Compensating%20Transaction%20pattern.md)) برای بازگشت به شرایط قبل عملیات موفق در یک request. سرویس ناموفق ممکن است رویدادی را به دلیل خرابی فعال کند. در آن صورت، استفاده از مکانیسم تلاش مجدد(retry) و یا time out ، آن را برای تشخیص آن که عملیات مورد نظر به عنوان یک شکست برداشت می‌شود را در نظر بگیرید. برای مثال به بخش مثال در این مبحث مراجعه کنید.  
  
هنگامی که می‌خواهید عملیات تجاری مستقل را به صورت موازی پردازش کنید، پیاده سازی یک workflow کار ساده‌ای است. می‌توانید از یک message bus استفاده کنید. با این حال، زمانی که طراحی الگوی choreography باید در یک توالی اتفاق بیفتد پس workflow می‌تواند پیچیده‌تر شود. به عنوان مثال، سرویس C تنها پس از اینکه سرویس A و سرویس B عملیات خود را با موفقیت کامل کردند، می‌تواند عملیات خود را آغاز کند. یک رویکرد این است که message bus پیام داشته باشید که پیامها را به ترتیب مورد نیاز دریافت می‌کنند. برای اطلاعات بیشتر به بخش مثال در این مبحث مراجعه کنید.  
  
اگر تعداد سرویس‌ها به سرعت رشد کند، الگوی choreography به یک چالش تبدیل می‌شود. با توجه به تعداد بالای قطعات متحرک مستقل (independent moving parts)  که در نتیجه باعث workflow پیچیده‌‌تر بین سرویس‌ها  می‌شود. همچنین ردیابی خطاها را مشکل‌تر  می‌کند.  
  
ارکستراتور به طور متمرکز انعطاف پذیری workflow را مدیریت می‌کند و می‌تواند به یک نقطه شکست تبدیل شود. از سوی دیگر، در الگوی choreography، وظایف بین همه سرویس‌ها توزیع می‌شود و انعطاف پذیری توسعه نرم‌افزار افزایش می‌یابد.  
  
هر سرویس نه تنها مسئول انعطاف پذیری (resiliency) عملیات خود است، بلکه همچنین وظیفه workflow را نیز بر عهده دارد. این وظیفه می‌تواند برای سرویس‌ها تا حدی سنگین باشد و استفاده از آن را سخت کند. هر سرویس باید حالت‌‌های ناگذرا، گذرا و time-out را چندین بار امتحان کند تا request‌های آن به‌ خوبی خاتمه یابد. همچنین سرویس‌ها  باید نسبت به اطلاع رسانی موفقیت یا عدم موفقیت عملیات توانایی داشته باشند تا بقیه سرویسها بر این اساس به درستی کار کنند و وظایف خود رو ادامه دهند.


## مثال
توجه این مثال در مستندات مایکروسافت عوض شده است.

این مثال الگوی choreography را با برنامه تحویل هواپیما‌های بدون سرنشین( [Drone Delivery app](https://github.com/mspnp/microservices-reference-implementation)) نشان می‌دهد. وقتی کلاینت درخواست مورد نظر را خود را انتخاب می‌کند ، برنامه یک هواپیمای بدون سرنشین را به اون اختصاص می‌دهد و به کلاینت اطلاع می‌دهد.  
  
نمونه ‌ای از این الگوی در [GitHub](https://github.com/mspnp/cloud-design-patterns/tree/master/choreography) موجود است.

![choreography-example](../assets/messaging/choreography-example.png)


یک تراکنش تجاری از  سمت یک کلاینت به 3 نوع عملیات تجاری متمایز نیاز دارد: 
- ایجاد یا به‌روزرسانی یک بسته 
- اختصاص یک هواپیمای بدون سرنشین برای تحویل بسته  
- بررسی وضعیت تحویل.

این عملیات توسط سه میکروسرویس انجام می‌شود: بسته، زمان‌بندی هواپیمای بدون سرنشین و خدمات تحویل بسته. به جای ارکستراتور مرکزی، سرویسها از پیام برای همکاری و هماهنگ کردن درخواست بین خود استفاده می‌کنند.

### طراحی

تراکنش تجاری به صورت متوالی از طریق hop‌های متعدد پردازش می‌شود. هر hop دارای یک message bus و business service مربوطه است.  
  
هنگامی که یک کلاینت درخواست تحویل(delivery request) را از طریق یک  HTTP endpoint ارسال می‌کند، سرویس مورد نظر آن را دریافت می‌کند و یک رویداد عملیاتی را فعال می‌کند و آن را به یک message bus ارسال می‌کند. این bus در واقع مشترک تجاری را فراخوانی می‌کند و رویداد را در یک request از نوع POST ارسال می‌کند. با دریافت رویداد، سرویس تجاری می‌تواند عملیات را با موفقیت یا شکست انجام دهد و اگر برنامه دچار time out شود، عملیات به پایان می‌رسد. در صورت موفقیت آمیز بودن عملیات، سرویس با Ok status code به bus پاسخ می‌دهد، در ادامه رویداد (event) عملیات جدیدی را فعال می‌کند و آن را به message bus مربوط به hop بعدی ارسال می‌کند. در صورت وقوع خرابی یا وقفه در time-out، سرویس با ارسال کد BadRequest به message bus که درخواست اصلی POST را ارسال کرده است، مشکل را گزارش می‌کند. message bus عملیات را بر اساس روش retry دوباره انجام می‌دهد. پس از انقضای  timeout در این عملیات، message bus، عملیات ناموفق را علامت گذاری می‌کند و پردازش باقی request متوقف می‌شود.  
  
این workflow تا زمانی که همه requestها پردازش شود ادامه می‌یابد.  
  
این طرح از چندین message bus برای پردازش کل business transaction استفاده می‌کند. همین طور [Microsoft Azure Event Grid](https://learn.microsoft.com/en-us/azure/event-grid/)  سرویس پیام رسانی را ارائه می‌دهد. این برنامه در یک  [Azure Kubernetes Service (AKS)](https://learn.microsoft.com/en-us/azure/aks/) cluster با [two containers in the same pod](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/#creating-a-pod-that-runs-two-containers) مستقر شده است. یک کانتینر [ambassador](https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador) را اجرا می‌کند که با Event Grid تعامل دارد در حالی که دیگری یک business service را اجرا می‌کند. رویکرد با دو [کانتینر](https://www.docker.com/resources/what-container/) در یک [pod](https://kubernetes.io/docs/concepts/workloads/pods/) قابلیت‌های  اجرایی و مقیاس پذیری را بهبود می‌بخشد. ambassador و business service معمولا  شبکه یکسانی را  با هم به اشتراک می‌گذارند که امکان تأخیر کم و توان عملیاتی بالا را فراهم می‌کند.  
  
برای جلوگیری از تکرار عملیات یا retry متعدد، فقط Event Grid به‌جای سرویس تجاری، یک عملیات را دوباره تکرار می‌کند. با ارسال یک پیام به یک  [dead letter queue (DLQ)](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dead-letter-queues) یک درخواست ناموفق را علامت گذاری می‌کند.  
  
سرویس تجاری نمی‌تواند متوجه این موضوع شود که retry مختلف از یک منبع مشخص تکراری است یا نه.  به عنوان مثال، سرویس Package از عملیات upsert برای افزودن داده‌ها به  ذخیره‌گاه داده استفاده می‌کند.  
  
این مثال یک راه حل دستکاری شده را برای ارتباط بین تماس‌ها در همه سرویس‌ها و پرش‌‌های Event Grid پیاده سازی می‌کند.  
  
در اینجا یک نمونه کد وجود دارد که الگوی choreography بین تمام سرویس‌‌های تجاری را نشان می‌دهد. این کد گردش کار  Drone Delivery app transactions را نشان می‌دهد. کد‌های مربوط به مدیریت کردن خطاها  و لاگ خطاها برای اختصار حذف شده است.

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

## منابع مرتبط

این الگوها را در طراحی خود برای choreography در نظر بگیرید.  
  
*‏ با استفاده از  [ambassador design pattern](./Ambassador%20pattern.md)،  می‌توانید  business service را ماژول بندی کنید.  
  
*‏ الگوی  [queue-based load leveling pattern](./Queue-Based%20Load%20Leveling%20pattern.md) را برای مدیریت کردن  نوسانات workload به کار ببرید.  
  
*‏ از پیام‌‌های توزیع شده ناهمزمان از طریق الگوی  [publisher-subscriber pattern](./Publisher-Subscriber%20pattern.md) استفاده کنید.  
  
*‏ از  [compensating transactions](./Compensating%20Transaction%20pattern.md) برای خنثی سازی یک سری عملیات موفق در صورت شکست یک یا چند عملیات مرتبط استفاده کنید.  
  
*‏ برای اطلاعات در مورد استفاده از message broker در زیرساخت پیام رسانی، به گزینه‌های [Asynchronous messaging options in Azure](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/messaging) مراجعه کنید.