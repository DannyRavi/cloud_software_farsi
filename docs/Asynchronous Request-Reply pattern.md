پردازش باطن را از یک میزبان فرانت اند جدا کنید، جایی که پردازش باطن باید ناهمزمان باشد، اما فرانت اند هنوز به پاسخ واضح نیاز دارد.

## Context and problem

در توسعه برنامه‌های کاربردی مدرن، طبیعی است که برنامه‌های کلاینت - اغلب کدهایی که در یک سرویس گیرنده وب (مرورگر) اجرا می‌شوند - به APIهای راه دور برای ارائه منطق تجاری و نوشتن عملکرد وابسته باشند. این APIها ممکن است مستقیماً با برنامه مرتبط باشند یا ممکن است خدمات مشترک ارائه شده توسط شخص ثالث باشند. معمولاً این فراخوانی‌های API روی پروتکل HTTP(S) انجام می‌شوند و از معنای REST پیروی می‌کنند.

در بیشتر موارد، APIهای یک برنامه کلاینت به گونه ای طراحی شده اند که به سرعت، در حدود 100 میلی ثانیه یا کمتر پاسخ دهند. عوامل زیادی می توانند بر تأخیر پاسخ تأثیر بگذارند، از جمله:

پشته میزبانی یک برنامه.
اجزای امنیتی
موقعیت جغرافیایی نسبی تماس گیرنده و بخش پشتیبان.
زیرساخت شبکه
بار فعلی
اندازه محموله درخواستی
در حال پردازش طول صف
زمان پردازش درخواست توسط Backend
هر یک از این عوامل می تواند تأخیر را به پاسخ اضافه کند. برخی از آنها را می توان با کوچک کردن باطن کاهش داد. سایرین، مانند زیرساخت شبکه، تا حد زیادی خارج از کنترل توسعه دهنده برنامه هستند. اکثر APIها می توانند به اندازه کافی سریع پاسخ دهند تا پاسخ ها از طریق همان اتصال برسند. کد برنامه می‌تواند یک تماس API همزمان را به روشی غیرمسدود کننده برقرار کند و ظاهر پردازش ناهمزمان را ارائه دهد که برای عملیات I/O-bound توصیه می‌شود.

با این حال، در برخی از سناریوها، کار انجام شده توسط Backend ممکن است طولانی مدت، به ترتیب چند ثانیه باشد، یا ممکن است یک فرآیند پس‌زمینه باشد که در چند دقیقه یا حتی ساعت‌ها اجرا می‌شود. در آن صورت، انتظار برای تکمیل کار قبل از پاسخ به درخواست، امکان پذیر نیست. این وضعیت یک مشکل بالقوه برای هر الگوی درخواست-پاسخ همزمان است.

برخی از معماری ها با استفاده از یک واسطه پیام برای جداسازی مراحل درخواست و پاسخ، این مشکل را حل می کنند. این جداسازی اغلب با استفاده از الگوی تراز بار مبتنی بر صف حاصل می شود. این جداسازی می تواند به فرآیند مشتری و API باطن اجازه دهد تا به طور مستقل مقیاس شوند. اما این جداسازی همچنین پیچیدگی بیشتری را در زمانی که مشتری به اطلاع رسانی موفقیت نیاز دارد، به همراه دارد، زیرا این مرحله باید ناهمزمان شود.

بسیاری از ملاحظات مشابهی که برای برنامه‌های کلاینت مطرح شد، برای فراخوانی‌های REST API سرور به سرور در سیستم‌های توزیع‌شده نیز اعمال می‌شود - به عنوان مثال، در معماری میکروسرویس‌ها.

## Solution

یکی از راه حل های این مشکل استفاده از نظرسنجی HTTP است. نظرسنجی برای کد سمت سرویس گیرنده مفید است، زیرا ارائه نقاط پایانی برگشت تماس یا استفاده از اتصالات طولانی مدت می تواند دشوار باشد. حتی زمانی که امکان بازگشت به تماس وجود دارد، کتابخانه‌ها و خدمات اضافی مورد نیاز گاهی اوقات می‌توانند پیچیدگی بیشتری را اضافه کنند.

برنامه سرویس گیرنده یک تماس همزمان با API برقرار می کند و یک عملیات طولانی مدت در باطن را راه اندازی می کند.

API در سریع ترین زمان ممکن به صورت همزمان پاسخ می دهد. یک کد وضعیت HTTP 202 (پذیرفته شده) را برمی گرداند و تأیید می کند که درخواست برای پردازش دریافت شده است.

```
توجه داشته باشید

در واقع API باید هم درخواست و هم اقدامی را که باید قبل از شروع فرآیند طولانی انجام شود تأیید کند. اگر درخواست نامعتبر است، بلافاصله با یک کد خطا مانند HTTP 400 (درخواست بد) پاسخ دهید.
```


پاسخ دارای یک مرجع مکان است که به یک نقطه پایانی اشاره می کند که مشتری می تواند برای بررسی نتیجه عملیات طولانی مدت نظرسنجی کند.  
  
 و API پردازش را به مؤلفه دیگری مانند صف پیام بارگذاری می کند.  
  
برای هر تماس موفق با نقطه پایانی وضعیت، HTTP 200 را برمی گرداند. در حالی که کار هنوز در انتظار است، نقطه پایانی وضعیت منبعی را برمی گرداند که نشان می دهد کار هنوز در حال انجام است. هنگامی که کار کامل شد، نقطه پایانی وضعیت می‌تواند منبعی را که نشان‌دهنده اتمام است برگرداند یا به URL منبع دیگری هدایت شود. به عنوان مثال، اگر عملیات ناهمزمان یک منبع جدید ایجاد کند، نقطه پایانی وضعیت به URL مربوط به آن منبع هدایت می شود.  
  
نمودار زیر یک جریان معمولی را نشان می دهد:


پاسخ دارای یک مرجع مکان است که به یک نقطه پایانی اشاره می کند که مشتری می تواند برای بررسی نتیجه عملیات طولانی مدت نظرسنجی کند.  
  
API پردازش را به مؤلفه دیگری مانند صف پیام بارگذاری می کند.  
  
برای هر تماس موفق با نقطه پایانی وضعیت، HTTP 200 را برمی گرداند. در حالی که کار هنوز در انتظار است، نقطه پایانی وضعیت منبعی را برمی گرداند که نشان می دهد کار هنوز در حال انجام است. هنگامی که کار کامل شد، نقطه پایانی وضعیت می‌تواند منبعی را که نشان‌دهنده اتمام است برگرداند یا به URL منبع دیگری هدایت شود. به عنوان مثال، اگر عملیات ناهمزمان یک منبع جدید ایجاد کند، نقطه پایانی وضعیت به URL مربوط به آن منبع هدایت می شود.  
  
نمودار زیر یک جریان معمولی را نشان می دهد:

![[Pasted image 20231127163543.png]]

1 -مشتری یک درخواست ارسال می کند و یک پاسخ HTTP 202 (پذیرفته شده) دریافت می کند.  
2- مشتری یک درخواست HTTP GET را به نقطه پایانی وضعیت ارسال می کند. کار هنوز در انتظار است، بنابراین این تماس HTTP 200 را برمی گرداند.  
3- در برخی مواقع، کار کامل می شود و نقطه پایانی وضعیت 302 (یافت شده) را با هدایت مجدد به منبع برمی گرداند.  
4- مشتری منبع را در URL مشخص شده واکشی می کند.

## Issues and considerations

چندین راه ممکن برای پیاده سازی این الگو بر روی HTTP وجود دارد و همه سرویس های بالادستی معنایی یکسان ندارند. به عنوان مثال، بیشتر سرویس‌ها پاسخ HTTP 202 را از روش GET برنمی‌گردانند، زمانی که یک فرآیند راه دور تمام نشده باشد. به دنبال معنای REST خالص، آنها باید HTTP 404 (یافت نشد) را برگردانند. زمانی که فکر می‌کنید نتیجه تماس هنوز وجود ندارد، این پاسخ منطقی است.  
  
یک پاسخ HTTP 202 باید مکان و فرکانسی را که مشتری باید برای پاسخ نظرسنجی کند، نشان دهد. باید هدرهای اضافی زیر را داشته باشد:

|Header|Description|Notes|
|---|---|---|
|Location|A URL the client should poll for a response status.|This URL could be a SAS token with the [Valet Key Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/valet-key) being appropriate if this location needs access control. The valet key pattern is also valid when response polling needs offloading to another backend|
|Retry-After|An estimate of when processing will complete|This header is designed to prevent polling clients from overwhelming the back-end with retries.|

* ممکن است لازم باشد از یک پراکسی پردازش یا نما برای دستکاری هدرهای پاسخ یا محموله بسته به خدمات اساسی استفاده شده استفاده کنید.

* اگر نقطه پایانی وضعیت پس از تکمیل تغییر مسیر دهد، بسته به معنای دقیقی که پشتیبانی می کنید، کدهای بازگشتی مناسب HTTP 302 یا HTTP 303 هستند.

* پس از پردازش موفقیت آمیز، منبع مشخص شده توسط هدر مکان باید کد پاسخ HTTP مناسب مانند 200 (OK)، 201 (ایجاد شده)، یا 204 (بدون محتوا) را برگرداند.

* اگر در حین پردازش خطایی رخ داد، خطا را در URL منبع توضیح داده شده در سرصفحه Location ادامه دهید و در حالت ایده آل یک کد پاسخ مناسب را از آن منبع به مشتری برگردانید (کد 4xx).

* همه راه حل ها این الگو را به یک شکل پیاده سازی نمی کنند و برخی از سرویس ها شامل سرصفحه های اضافی یا جایگزین می شوند. به عنوان مثال، Azure Resource Manager از یک نوع تغییر یافته از این الگو استفاده می کند. برای اطلاعات بیشتر، Azure Resource Manager Async Operations را ببینید.

* مشتریان قدیمی ممکن است از این الگو پشتیبانی نکنند. در این صورت، ممکن است لازم باشد یک نما روی API ناهمزمان قرار دهید تا پردازش ناهمزمان را از مشتری اصلی پنهان کنید. به عنوان مثال، Azure Logic Apps که به صورت بومی از این الگو پشتیبانی می‌کند، می‌تواند به عنوان یک لایه یکپارچه‌سازی بین یک API ناهمزمان و یک کلاینت که تماس‌های همزمان برقرار می‌کند استفاده شود. به انجام کارهای طولانی مدت با الگوی اقدام webhook مراجعه کنید.

* در برخی از سناریوها، ممکن است بخواهید راهی برای لغو درخواست طولانی مدت برای مشتریان فراهم کنید. در آن صورت، سرویس باطن باید از نوعی دستورالعمل لغو پشتیبانی کند.
## When to use this pattern


از این الگو برای موارد زیر استفاده کنید:  
  
کدهای سمت کلاینت، مانند برنامه های مرورگر، که در آن ارائه نقاط پایانی تماس برگشتی دشوار است، یا استفاده از اتصالات طولانی مدت، پیچیدگی بیشتری را اضافه می کند.  
  
تماس‌های سرویسی که فقط پروتکل HTTP در دسترس است و سرویس برگشت نمی‌تواند به دلیل محدودیت‌های دیوار آتش در سمت کلاینت، تماس‌های برگشتی را برقرار کند.  
  
تماس‌های خدماتی که نیاز به ادغام با معماری‌های قدیمی دارند که از فن‌آوری‌های پاسخ به تماس مدرن مانند WebSockets یا webhooks پشتیبانی نمی‌کنند.  
  
این الگو ممکن است زمانی مناسب نباشد:  
  
به جای آن می توانید از سرویسی استفاده کنید که برای اعلان های ناهمزمان ساخته شده است، مانند Azure Event Grid.  
پاسخ ها باید در زمان واقعی برای مشتری ارسال شوند.  
مشتری باید نتایج زیادی را جمع آوری کند و تأخیر دریافتی آن نتایج مهم است. به جای آن یک الگوی اتوبوس خدماتی را در نظر بگیرید.  
می توانید از اتصالات شبکه پایدار سمت سرور مانند WebSockets یا SignalR استفاده کنید. از این خدمات می توان برای اطلاع تماس گیرنده از نتیجه استفاده کرد.  
طراحی شبکه به شما این امکان را می‌دهد که پورت‌هایی را برای دریافت تماس‌های ناهمزمان یا وبک‌هوک باز کنید.


## Example

کد زیر گزیده هایی از برنامه ای را نشان می دهد که از توابع Azure برای پیاده سازی این الگو استفاده می کند. سه عملکرد در راه حل وجود دارد:

* نقطه پایانی API ناهمزمان.
* نقطه پایان وضعیت
* یک تابع Backend که آیتم های کاری در صف را می گیرد و آنها را اجرا می کند.

![[Pasted image 20231127165352.png]]
این نمونه در GitHub موجود است.

### AsyncProcessingWorkAcceptor function

تابع AsyncProcessingWorkAcceptor یک نقطه پایانی را پیاده سازی می کند که کار یک برنامه مشتری را می پذیرد و آن را در یک صف برای پردازش قرار می دهد.

* تابع یک ID درخواست تولید می کند و آن را به عنوان ابرداده به پیام صف اضافه می کند.
* پاسخ HTTP شامل یک سرصفحه مکان است که به یک نقطه پایانی وضعیت اشاره می کند. شناسه درخواست بخشی از مسیر URL است.

```csharp
public static class AsyncProcessingWorkAcceptor
{
    [FunctionName("AsyncProcessingWorkAcceptor")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "post", Route = null)] CustomerPOCO customer,
        [ServiceBus("outqueue", Connection = "ServiceBusConnectionAppSetting")] IAsyncCollector<ServiceBusMessage> OutMessages,
        ILogger log)
    {
        if (String.IsNullOrEmpty(customer.id) || string.IsNullOrEmpty(customer.customername))
        {
            return new BadRequestResult();
        }

        string reqid = Guid.NewGuid().ToString();

        string rqs = $"http://{Environment.GetEnvironmentVariable("WEBSITE_HOSTNAME")}/api/RequestStatus/{reqid}";

        var messagePayload = JsonConvert.SerializeObject(customer);
        var message = new ServiceBusMessage(messagePayload);
        message.ApplicationProperties.Add("RequestGUID", reqid);
        message.ApplicationProperties.Add("RequestSubmittedAt", DateTime.Now);
        message.ApplicationProperties.Add("RequestStatusURL", rqs);

        await OutMessages.AddAsync(message);

        return new AcceptedResult(rqs, $"Request Accepted for Processing{Environment.NewLine}ProxyStatus: {rqs}");
    }
}
```

### AsyncProcessingBackgroundWorker function

تابع AsyncProcessingBackgroundWorker عملیات را از صف دریافت می کند، برخی کارها را بر اساس بار پیام انجام می دهد و نتیجه را در یک حساب ذخیره سازی می نویسد.

```csharp
public static class AsyncProcessingBackgroundWorker
{
    [FunctionName("AsyncProcessingBackgroundWorker")]
    public static async Task RunAsync(
        [ServiceBusTrigger("outqueue", Connection = "ServiceBusConnectionAppSetting")] BinaryData customer,
        IDictionary<string, object> applicationProperties,
        [Blob("data", FileAccess.ReadWrite, Connection = "StorageConnectionAppSetting")] BlobContainerClient inputContainer,
        ILogger log)
    {
        // Perform an actual action against the blob data source for the async readers to be able to check against.
        // This is where your actual service worker processing will be performed

        var id = applicationProperties["RequestGUID"] as string;

        BlobClient blob = inputContainer.GetBlobClient($"{id}.blobdata");

        // Now write the results to blob storage.
        await blob.UploadAsync(customer);
    }
}
```


### AsyncOperationStatusChecker function

تابع AsyncOperationStatusChecker نقطه پایانی وضعیت را پیاده سازی می کند. این تابع ابتدا بررسی می کند که آیا درخواست تکمیل شده است یا خیر  
  
* اگر درخواست تکمیل شد، تابع یا یک کلید نوبت به پاسخ برمی‌گرداند، یا تماس را فوراً به URL-کلید هدایت می‌کند.  
* اگر درخواست هنوز معلق است، باید یک کد 200 شامل وضعیت فعلی را برگردانیم.



```csharp
public static class AsyncOperationStatusChecker
{
    [FunctionName("AsyncOperationStatusChecker")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "RequestStatus/{thisGUID}")] HttpRequest req,
        [Blob("data/{thisGuid}.blobdata", FileAccess.Read, Connection = "StorageConnectionAppSetting")] BlockBlobClient inputBlob, string thisGUID,
        ILogger log)
    {

        OnCompleteEnum OnComplete = Enum.Parse<OnCompleteEnum>(req.Query["OnComplete"].FirstOrDefault() ?? "Redirect");
        OnPendingEnum OnPending = Enum.Parse<OnPendingEnum>(req.Query["OnPending"].FirstOrDefault() ?? "OK");

        log.LogInformation($"C# HTTP trigger function processed a request for status on {thisGUID} - OnComplete {OnComplete} - OnPending {OnPending}");

        // Check to see if the blob is present
        if (await inputBlob.ExistsAsync())
        {
            // If it's present, depending on the value of the optional "OnComplete" parameter choose what to do.
            return await OnCompleted(OnComplete, inputBlob, thisGUID);
        }
        else
        {
            // If it's NOT present, then we need to back off. Depending on the value of the optional "OnPending" parameter, choose what to do.
            string rqs = $"http://{Environment.GetEnvironmentVariable("WEBSITE_HOSTNAME")}/api/RequestStatus/{thisGUID}";

            switch (OnPending)
            {
                case OnPendingEnum.OK:
                    {
                        // Return an HTTP 200 status code.
                        return new OkObjectResult(new { status = "In progress", Location = rqs });
                    }

                case OnPendingEnum.Synchronous:
                    {
                        // Back off and retry. Time out if the backoff period hits one minute.
                        int backoff = 250;

                        while (!await inputBlob.ExistsAsync() && backoff < 64000)
                        {
                            log.LogInformation($"Synchronous mode {thisGUID}.blob - retrying in {backoff} ms");
                            backoff = backoff * 2;
                            await Task.Delay(backoff);
                        }

                        if (await inputBlob.ExistsAsync())
                        {
                            log.LogInformation($"Synchronous Redirect mode {thisGUID}.blob - completed after {backoff} ms");
                            return await OnCompleted(OnComplete, inputBlob, thisGUID);
                        }
                        else
                        {
                            log.LogInformation($"Synchronous mode {thisGUID}.blob - NOT FOUND after timeout {backoff} ms");
                            return new NotFoundResult();
                        }
                    }

                default:
                    {
                        throw new InvalidOperationException($"Unexpected value: {OnPending}");
                    }
            }
        }
    }

    private static async Task<IActionResult> OnCompleted(OnCompleteEnum OnComplete, BlockBlobClient inputBlob, string thisGUID)
    {
        switch (OnComplete)
        {
            case OnCompleteEnum.Redirect:
                {
                    // Redirect to the SAS URI to blob storage

                    return new RedirectResult(inputBlob.GenerateSASURI());
                }

            case OnCompleteEnum.Stream:
                {
                    // Download the file and return it directly to the caller.
                    // For larger files, use a stream to minimize RAM usage.
                    return new OkObjectResult(await inputBlob.DownloadContentAsync());
                }

            default:
                {
                    throw new InvalidOperationException($"Unexpected value: {OnComplete}");
                }
        }
    }
}

public enum OnCompleteEnum
{

    Redirect,
    Stream
}

public enum OnPendingEnum
{

    OK,
    Synchronous
}
```

## Next steps

اطلاعات زیر ممکن است هنگام اجرای این الگو مرتبط باشد:  
  
گزینه Azure Logic Apps - وظایف طولانی مدت را با الگوی عمل نظرسنجی انجام دهید.  
برای بهترین روش‌های کلی در هنگام طراحی وب API، به طراحی Web API مراجعه کنید.

##   
Related resources

- [Backends for Frontends pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends)