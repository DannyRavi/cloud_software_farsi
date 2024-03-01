

هنگامی‌ که یک برنامه سعی می‌ کند به یک سرویس یا شبکه متصل شود و خرابی های گذرا را مدیریت کند به کمک الگوی تلاش مجدد (Retry) روی یک عملیات ناموفق که در نتیجه می‌ تواند ثبات برنامه را بهبود بخشد.

### **طرح صورت مسئله:**

برنامه‌ای که با عناصر در حال اجرا در محیط ابری ارتباط برقرار می‌‌کند باید نسبت به خطاهای گذرا که می‌‌تواند در این محیط رخ دهد حساس باشد. خطاها شامل از دست دادن لحظه ای اتصال شبکه و سرویس‌ها یا در دسترس نبودن موقت یک سرویس یا دچار timeout شدن آن است که زمانی که یک سرویس شلوغ (busy) است رخ می‌ دهد.

این خطاها معمولاً خود به خود تصحیح می‌‌شوند و اگر عملی که باعث ایجاد خطا شده است پس از تأخیر مناسب تکرار شود، احتمالاً موفقیت‌آمی‌ز خواهد بود. به عنوان مثال، یک سرویس پایگاه داده که تعداد زیادی درخواست همزمان را پردازش می‌‌کند، می‌‌تواند یک [throttling strategy](https://learn.microsoft.com/en-us/azure/architecture/patterns/throttling) را پیاده‌سازی کند که به طور موقت هر درخواست دیگری را رد می‌‌کند تا زمانی که حجم کاری آن کاهش یابد. برنامه‌ای که سعی می‌‌کند به پایگاه داده دسترسی پیدا کند ممکن است متصل نشود، اما اگر بعد از تأخیر دوباره امتحان کند ممکن است موفق شود.

## راه حل


در فضای ابری، خطاهای گذرا غیر معمول نیستند و باید برنامه‌ به گونه‌ای طراحی شود تا به زیبایی و شفافیت آن‌ها را مدیریت کند. این کار تأثیراتی را که خطاها می‌ توانند بر business tasks که برنامه انجام می‌ دهد را به حداقل می‌رساند.

اگر یک برنامه در زمانی که می‌‌خواهد درخواستی را به یک سرویس ریموت ارسال کند، خطایی را تشخیص دهد، می‌‌تواند با استفاده از استراتژی‌های زیر این خرابی را برطرف کند:

*  **Cancel**:  اگر خطا نشان می‌ دهد که خرابی گذرا نیست یا تکرار آن بعید است موفقیت آمیز باشد، برنامه باید عملیات را لغو کند و یک استثنا (exception) را گزارش کند. به عنوان مثال، authentication failure ناشی از ارائه اعتبارنامه‌های نامعتبر(invalid credentials) به هیچ وجه مهم نیست که چند بار try شده است.

* **Retry**: اگر خطای خاص گزارش شده غیرعادی یا به ندرت باشد، ممکن است به دلیل شرایط غیرعادی مانند خراب شدن یک packet شبکه در حین انتقال ایجاد شده باشد. در این حالت، برنامه می‌‌تواند بلافاصله درخواست ناموفق را دوباره امتحان کند، زیرا بعید است که همان شکست تکرار شود و احتمالاً درخواست موفقیت‌آمیز خواهد بود.

*  **Retry after delay**: اگر خطا ناشی از یکی از  اتصالات ساده یا busy failures باشد، شبکه یا سرویس ممکن است به مدت کوتاهی نیاز داشته باشد تا مشکلات شبکه اصلاح شود یا کارهای عقب افتاده پاک شود. application قبل از retry request باید برای زمان مناسب منتظر بماند.

برای خرابی‌های گذرا(transient) رایج‌تر، باید تناوبی بین تلاش‌های مجدد  انتخاب شود تا request‌ها از چند نمونه برنامه تا حد امکان به طور یکنواخت پخش شوند. این احتمال overloaded بیش از حد یک سرویس busy را کاهش می‌ دهد. اگر بسیاری از نمونه های یک برنامه به طور مداوم سرویسی را با  retry requests درگیر سرشلوغی(overwhelming) می‌‌کنند احتمالا بازیابی شدن سرویس بیشتر طول می‌‌کشد.

اگر request همچنان ناموفق بود، application می‌‌تواند منتظر بماند و تلاش دیگری انجام دهد. در صورت لزوم این فرآیند را می‌ توان با افزایش تاخیر بین retry های مجدد تکرار کرد تا زمانی که حداکثر تعداد درخواست‌ها انجام شود. بسته به نوع خرابی و احتمال تصحیح آن در این مدت، تأخیر را می‌ توان به صورت تدریجی یا تصاعدی افزایش داد.

نمودار زیر فراخوانی عملیات در یک سرویس می‌زبانی شده با استفاده از این الگو را نشان می‌ دهد. اگر درخواست پس از تعدادی تلاش از پیش تعیین شده ناموفق باشد، برنامه باید خطا را به عنوان یک استثنا تلقی کرده و آن را مطابق با آن رسیدگی کند.

نمودار زیر فراخوانی عملیات در یک hosted service  با استفاده از این الگو را نشان می‌ دهد. اگر request پس از تعدادی تلاش از پیش تعیین شده ناموفق باشد، برنامه باید خطا را به عنوان یک exception تلقی کرده و آن را مطابق با آن شرایط را handle کند.

![[Pasted image 20240229135902.png]]

برنامه باید تمام  اقدامات برای دسترسی به یک remote service را در کدی درهم‌سازی کند که یک خط مشی retry مطابق  با یکی از استراتژی‌های فهرست شده‌ای که در بالا را معرفی شده است را داشته باشد. درخواست‌هایی که به سرویس‌های مختلف ارسال می‌‌شوند می‌‌توانند تابع سیاست‌های(policies) متفاوتی باشند. برخی از provideها کتابخانه‌هایی را ارائه می‌‌کنند که خط مشی retry خاصی را اجرا می‌‌کنند، جایی که برنامه می‌‌تواند حداکثر تعداد retryها و زمان بین retry و سایر پارامترها را مشخص کند.

یک application باید جزئیات خطاها و عملیات های ناموفق را ثبت کند. این اطلاعات برای اپراتورهای برنامه مفید است. همانطور که گفته شد، برای جلوگیری از تعداد زیاد alertها برای اپراتورها در مورد عملیاتی که در آن retryها موفق بودند، بهتر است خرابی‌های اولیه را به‌عنوان ورودی‌های اطلاعاتی ثبت کنید و فقط failure آخرین retry را به عنوان یک error واقعی ثبت کنید. در اینجا نمونه ای از آن با عنوان  [example of how this logging model would look like](https://docs.particular.net/nservicebus/recoverability/#retry-logging) آورده شده است.


اگر سرویسی که اغلب در دسترس نیست یا busy است، معمولا علت آن این است که سرویس منابع خود را تمام کرده است. شما می‌ توانید با کوچک کردن سرویس، تعداد این خطاها را کاهش دهید. به عنوان مثال، اگر یک سرویس پایگاه داده به طور مداوم بیش از حد سربار گذاری یا overloaded می‌ شود، ممکن است پارتیشن بندی پایگاه داده و پخش load آن در چندین سرور مفید باشد.

همینطور Microsoft Entity Framework امکاناتی را برای آزمایش مجدد عملیات پایگاه داده فراهم می‌ کند. همچنین، اکثر سرویس‌های Azure و SDK مشتری دارای مکانیزم retry هستند. برای اطلاعات بیشتر، به راهنمای  [Retry guidance for specific services](https://learn.microsoft.com/en-us/azure/architecture/best-practices/retry-service-specific)  مراجعه کنید.



### مسائل و ملاحظات:


هنگام تصمیم گیری در مورد نحوه اجرای این الگو باید نکات زیر را در نظر بگیرید.

در واقع خط مشی retry  باید طوری تنظیم شود که با الزامات تجاری برنامه و ماهیت شکست‌های آن مطابقت داشته باشد. برای برخی از عملیات غیر بحرانی بهتر است به جای اینکه چندین بار  retry کنید که بر توان عملیاتی برنامه تأثیر بگذارید، بهتر است سریع‌تر دچار شکست شوید. به عنوان مثال، در یک برنامه وب تعاملی که به یک سرویس ریموت دسترسی دارد، بهتر است پس از تعداد کمتری از retryها و تنها با تأخیر کوتاهی بین retry، برنامه fail شود و یک پیام مناسب برای کاربر نمایش داده شود (مثلاً «لطفاً بعداً دوباره امتحان کنید» ). برای یک برنامه گروهی (batch application)، ممکن است مناسب‌تر باشد که تعداد تلاش‌های مجدد را با تأخیر فزاینده بین retry‌ها افزایش دهیم.

یک خط مشی  retry  تهاجمی‌ با حداقل تأخیر بین retry‌ها و تعداد زیادی از retryها، می‌‌تواند سرویس busy را که نزدیک به ظرفیت نهایی اش کار می‌‌کند، تضعیف کند. این retry policy همچنین می‌ تواند بر پاسخگویی برنامه تأثیر بگذارد به خصوص زمانی که به طور مداوم در حالت retry برای انجام عملیات ناموفق باشد.

اگر یک request پس از تعداد قابل توجهی از retryها همچنان با شکست مواجه شد، بهتر است برنامه از ارسال request‌های بیشتر به همان منبع جلوگیری کند و به سادگی یک failure را هر چه سریع‌تر گزارش کند. هنگامی‌ که این دوره منقضی می‌ شود، برنامه به طور آزمایشی می‌ تواند به یک یا چند request اجازه دهد تا ببیند آیا آنها موفق هستند یا خیر. برای جزئیات بیشتر این استراتژی، [Circuit Breaker pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) را ببینید.


در نظر بگیرید که آیا این کار دارای توان عملیاتی  است یا خیر. اگر چنین است، ذاتاً retry بی خطر است. در غیر این صورت، retry می‌ تواند باعث شود که عملیات بیش از یک بار اجرا شود و عوارض جانبی ناخواسته داشته باشد. به عنوان مثال، یک سرویس ممکن است request را دریافت کند و request را با موفقیت پردازش کند، اما پاسخی ارسال نکند. در آن نقطه، منطق retry ممکن است request را مجددا ارسال کند، با این فرض که اولین درخواست دریافت نشده است.

در نظر بگیرید که چگونه retry عملیاتی که بخشی از یک تراکنش است بر ثبات کلی تراکنش تأثیر می‌ گذارد. برای به حداکثر رساندن شانس موفقیت و کاهش نیاز به لغو تمام مراحل تراکنش، خط مشی  retry  را برای عملیات مورد نیاز هر تراکنش باید به خوبی تنظیم کنید.

اطمینان حاصل کنید که همه کدهای retry به طور کامل در برابر انواع شرایط خرابی آزمایش شده است. بررسی کنید که به  بر کارایی یا قابلیت اطمینان برنامه تأثیر منفی نمی‌گذارد و باعث بار بیش از حد بر روی سرویس‌ها و منابع یا ایجاد وضعیت رقابتی (race conditions) یا گلوگاه (bottleneck) نمی‌شود.


منطق retry را فقط در جایی اجرا کنید که مفهوم کامل یک عملیات ناموفق(failing operation) درک شده باشد. به عنوان مثال، اگر task ای که شامل retry است، تسک دیگری را فراخوانی می‌ کند که آن هم شامل retry  است، این لایه اضافی از retryها می‌ تواند تاخیرهای طولانی را به پردازش اضافه کند. شاید بهتر باشد task سطح پایین را طوری پیکربندی کنید که سریع fail شود و دلیل شکست را به task ای که آن را فراخوانی کرده است گزارش دهید. سپس این task سطح بالاتر می‌ تواند بر اساس خط مشی خود این شکست را مدیریت کند.

ثبت داده‌ها و log از همه خرابی‌های ارتباطی (connectivity failures) که باعث retry می‌‌شوند بسیار مهم است تا بتوان مشکلات اساسی برنامه، سرویس‌ها یا منابع را شناسایی کرد.


همیشه باید خطاهایی را که به احتمال زیاد برای یک سرویس یا یک resource رخ می‌ دهد بررسی کنید تا متوجه شوید که آیا آنها  طولانی مدت یا همیشگی هستند و در صورت وقوع این حالت بهتر است به عنوان یک استثنا یا exception به آن حالت خطا رسیدگی شود. برنامه می‌‌تواند exception را گزارش یا ثبت یا log کند و سپس سعی کند با فراخوانی یک سرویس جایگزین (در صورت موجود بودن) یا به صورت ادامه دادن با عملکرد ضعیف وظیفه خود را تکمیل کند. برای اطلاعات بیشتر در مورد نحوه تشخیص و handle به عیوب طولانی مدت، به الگوی [Circuit Breaker pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) مراجعه کنید.

## **چه زمانی از این الگو استفاده کنیم؟**

از این الگو زمانی استفاده کنید که یک برنامه ممکن است هنگام تعامل یا دسترسی با یک سرویس ریموت، خطاهای گذرا را تجربه کند. انتظار می‌ رود این خطاها کوتاه مدت باشند و تکرار request که قبلا با خطا روبه‌رو شده است می‌تواند در تلاش بعدی موفق شود.

این الگو ممکن است مفید نباشد:

* هنگامی‌ که یک خطا احتمالاً طولانی مدت است، زیرا این مورد می‌تواند بر کیفیت پاسخگویی یک برنامه تأثیر بگذارد. برنامه ممکن است در تلاش برای تکرار درخواستی که احتمال شکست آن وجود دارد، زمان و منابع را تلف کند.  
* برای رسیدگی به خرابی‌هایی که به دلیل خطاهای گذرا نیستند، مانند exceptionهای داخلی ناشی از خطاهای منطق تجاری و پیاده سازی یک برنامه.  
* به عنوان جایگزینی برای پرداختن به مسائل مقیاس پذیری در یک سیستم. اگر برنامه‌ای با خطاهای busy به صورت مکرر مواجه می‌‌شود، اغلب نشانه آن است که سرویس یا منبعی که به آن دسترسی دارید باید بزرگ‌تر شود.

## مثال


این مثال در سی شارپ پیاده سازی الگوی Retry را نشان می‌ دهد. متد 'OperationWithBasicRetryAsync' که در زیر نشان داده شده است، یک سرویس خارجی را به صورت ناهمزمان از طریق متد 'TransientOperationAsync' فراخوانی می‌ کند. جزئیات متد 'TransientOperationAsync' مختص سرویس خواهد بود و از کد نمونه حذف شده است.

```csharp
private int retryCount = 3;
private readonly TimeSpan delay = TimeSpan.FromSeconds(5);

public async Task OperationWithBasicRetryAsync()
{
  int currentRetry = 0;

  for (;;)
  {
    try
    {
      // Call external service.
      await TransientOperationAsync();

      // Return or break.
      break;
    }
    catch (Exception ex)
    {
      Trace.TraceError("Operation Exception");

      currentRetry++;

      // Check if the exception thrown was a transient exception
      // based on the logic in the error detection strategy.
      // Determine whether to retry the operation, as well as how
      // long to wait, based on the retry strategy.
      if (currentRetry > this.retryCount || !IsTransient(ex))
      {
        // If this isn't a transient error or we shouldn't retry,
        // rethrow the exception.
        throw;
      }
    }

    // Wait to retry the operation.
    // Consider calculating an exponential delay here and
    // using a strategy best suited for the operation and fault.
    await Task.Delay(delay);
  }
}

// Async method that wraps a call to a remote service (details not shown).
private async Task TransientOperationAsync()
{
  ...
}
```

عبارتی که این روش را فراخوانی می‌ کند در یک بلوک try/catch ترکیب شده در یک حلقه for قرار دارد. اگر فراخوانی متد 'TransientOperationAsync' بدون ایجاد استثناء موفق شود، حلقه for خارج می‌ شود. اگر متد 'TransientOperationAsync' با شکست مواجه شود، بلوک catch دلیل شکست را بررسی می‌ کند. اگر تصور می‌ شود که یک خطای گذرا باشد، کد قبل از امتحان مجدد عملیات برای یک تاخیر کوتاه منتظر می‌ ماند.

حلقه for همچنین تعداد دفعاتی که این عملیات انجام شده است را ردیابی می‌ کند و اگر کد سه بار شکست بخورد، exception طولانی‌تر فرض می‌ شود. اگر استثنا گذرا نباشد یا طولانی‌مدت باشد، نگه‌دارنده یک exception می‌‌اندازد. این exception از حلقه for خارج می‌ شود و باید توسط کدی که متد 'OperationWithBasicRetryAsync' را فراخوانی می‌ کند، انجام شود.


روش 'IsTransient'  که در زیر نشان داده شده است، مجموعه خاصی از exceptionها را بررسی می‌‌کند که مربوط به محیطی است که کد در آن اجرا می‌‌شود. تعریف استثنای گذرا با توجه به منابعی که در دسترس هستند و محیطی که عملیات در آن انجام می‌‌شود، متفاوت خواهد بود.


```csharp
private bool IsTransient(Exception ex)
{
  // Determine if the exception is transient.
  // In some cases this is as simple as checking the exception type, in other
  // cases it might be necessary to inspect other properties of the exception.
  if (ex is OperationTransientException)
    return true;

  var webException = ex as WebException;
  if (webException != null)
  {
    // If the web exception contains one of the following status values
    // it might be transient.
    return new[] {WebExceptionStatus.ConnectionClosed,
                  WebExceptionStatus.Timeout,
                  WebExceptionStatus.RequestCanceled }.
            Contains(webException.Status);
  }

  // Additional exception checking logic goes here.
  return false;
}
```

## قدم بعدی

* قبل از نوشتن منطق retry برای برنامه، از یک فریم‌ورک کلی مانند  [Polly](https://github.com/App-vNext/Polly) برای دات نت یا [Resilience4j](https://github.com/resilience4j/resilience4j) برای جاوا استفاده کنید.

* هنگام پردازش فرمان‌هایی که داده‌های کسب‌وکار را تغییر می‌‌دهند، توجه داشته باشید که retry می‌‌تواند منجر به دوبار انجام این عمل شود، که اگر آن عمل چیزی شبیه شارژ کردن کارت اعتباری مشتری باشد، می‌‌تواند بسیار مشکل‌ساز باشد. استفاده از الگوی Idempotence شرح داده شده در این [پست](https://particular.net/blog/what-does-idempotent-mean) وبلاگ می‌ تواند به مقابله با این موقعیت ها کمک کند.

## منابع مرتبط

* الگوی [Reliable web app](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/reliable-web-app/overview) به شما نشان می دهد که چگونه الگوی retry را برای برنامه های تحت وب که در فضای ابری همگرا هستند اعمال کنید.
* برای اکثر سرویس های Azure  SDK،  مشتری شامل منطق retry داخلی است. برای اطلاعات بیشتر، به [راهنمای retry برای سرویس‌های Azure](https://learn.microsoft.com/en-us/azure/architecture/best-practices/retry-service-specific) مراجعه کنید.
* الگوی [Circuit Breaker](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker). اگر انتظار می رود که خرابی طولانی تر باشد، ممکن است اجرای الگوی [Circuit Breaker](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) مناسب تر باشد. ترکیب الگوهای Retry و Circuit Breaker یک رویکرد جامع برای رسیدگی به عیوب ارائه می دهد.