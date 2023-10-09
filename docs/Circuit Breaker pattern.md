برطرف کردن خطاهای که ممکن است زمان نامشخصی برای بررسی و حل  آنها طول بکشد. در این شرایط این الگو می تواند ثبات و پایداری یک برنامه را بهبود بخشد.
## Context and problem

در یک محیط توزیع‌شده، تماس‌ها با منابع و سرویس‌های ریموت ممکن است به دلیل transient faultها، مانند connectionهای کند در شبکه، timeoutها، یا بیش از حد پاسخگو شدن به درخواست‌ها شدن یا در دسترس نبودن منابع از راه دور، با خطا مواجه شوند. این خطاها معمولاً پس از مدت کوتاهی خود را اصلاح می‌کنند و یک برنامه ابری قوی باید با استفاده از یک استراتژی مانند [Retry pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry) آماده شود تا آنها را مدیریت کند.

با این حال، ممکن است شرایطی نیز وجود داشته باشد که خطاها ناشی از رویدادهای غیرمنتظره باشد و رفع آن ممکن است بسیار بیشتر طول بکشد. شدت این خطاها از قطع جزئی اتصال تا خرابی کامل یک سرویس متفاوت است. در این شرایط ممکن است برای یک برنامه کاربردی بیهوده باشد که به طور مداوم عملیاتی را که بعید به نظر می رسد موفقیت آمیز باشد را بارها امتحان کند و در عوض برنامه باید سریعاً بپذیرد که عملیات شکست خورده است و بر این اساس این شکست را مدیریت کند.

علاوه بر این، اگر یک سرویس بسیار busy باشد، خرابی در یک قسمت از سیستم ممکن است منجر به خرابی های پشت سری هم و متوالی شود. به عنوان مثال، عملیاتی که یک سرویس را فراخوانی می کند، می تواند طوری config شود تا یک timeout را اجرا کند، و اگر سرویس در این مدت پاسخ ندهد، با یک پیام شکست پاسخ دهد. با این حال، این استراتژی می‌تواند باعث شود که بسیاری از درخواست‌های همزمان برای همان عملیات تا پایان دوره زمانی مسدود شوند. این درخواست های مسدود شده ممکن است منابع مهم سیستم مانند حافظه، thread ها، اتصالات پایگاه داده و غیره را در خود نگه دارند. در نتیجه، این منابع ممکن است تمام شود و باعث خرابی سایر بخش‌های احتمالاً نامرتبط سیستم شود که نیاز به استفاده از منابع مشابه دارند. در این مواقع، ترجیح داده می‌شود که عملیات فوراً با شکست مواجه شود و فقط در صورت احتمال موفقیت، سعی کنید سرویس را فراخوانی کنید. توجه داشته باشید که تنظیم یک بازه زمانی کوتاه‌تر ممکن است به حل این مشکل کمک کند، اما مهلت زمانی نباید آنقدر کوتاه باشد که عملیات در بیشتر مواقع با شکست مواجه شود، حتی اگر درخواست سرویس در نهایت با موفقیت انجام شود.

## Solution

الگوی Circuit Breaker که توسط مایکل نایگارد در کتابش با عنوان [Release It!](https://pragprog.com/titles/mnee2/) رایج شده است، می‌تواند مانع از تلاش مکرر برنامه برای اجرای عملیاتی شود که احتمال شکست آن وجود دارد. اجازه دادن به ادامه کار بدون انتظار برای رفع عیب یا هدر دادن چرخه های CPU در حالی که مشخص می کند که عیب طولانی مدت است. الگوی Circuit Breaker همچنین یک برنامه کاربردی را قادر می سازد تا تشخیص دهد که آیا عیب برطرف شده است یا خیر. اگر به نظر می رسد مشکل برطرف شده است، برنامه می تواند سعی کند عملیات را فراخوانی کند.


هدف از الگوی Circuit Breaker با الگوی Retry متفاوت است. الگوی Retry یک برنامه را قادر می‌سازد تا یک عملیات را مجدداً امتحان کند، به این امید که موفق شود. الگوی Circuit Breaker مانع از انجام عملیاتی می شود که احتمال شکست آن وجود دارد. یک برنامه کاربردی می تواند این دو الگو را با استفاده از الگوی Retry برای فراخوانی عملیات از طریق  Circuit Breaker ترکیب کند. با این حال، منطق امتحان مجدد باید نسبت به exceptionهایی که توسط قطع کننده مدار بازگردانده می شود حساس باشد و اگر مدار شکن نشان می دهد که یک خطا گذرا نیست، از تلاش مجدد صرف نظر کند.

قطع کننده مدار به عنوان یک پروکسی برای عملیاتی عمل می کند که ممکن است خطا بخورد. پراکسی باید تعداد خرابی‌های اخیر رخ داده را کنترل کند و از این اطلاعات برای تصمیم‌گیری برای ادامه عملیات استفاده کند یا به سادگی یک exception را فوراً بازگرداند.  
  
پروکسی را می توان به عنوان یک state machine با حالت های زیر پیاده سازی کرد که عملکرد یک  Circuit Breaker را تقلید می کند:


**حالت Closed**: که request از application به عملیات پردازشی route می شود. پراکسی تعداد خرابی های اخیر را نگه می دارد و اگر فراخوانی عملیات ناموفق باشد، پراکسی این تعداد را افزایش می دهد. اگر تعداد خرابی‌های اخیر از یک آستانه مشخص در یک بازه زمانی معین بیشتر شود، پروکسی در حالت سوییچ  باز قرار می‌گیرد. در این مرحله، پراکسی یک تایمر زمان‌بندی را شروع می‌کند و زمانی که این تایمر منقضی شد، پراکسی در سوویچ را در  حالت نیمه باز قرار می‌دهد.

**حالت Open**: که request  برنامه بلافاصله با شکست مواجه می شود و یک exception به application باز می گردد.

**حالت Half-Open**: تعداد محدودی از request های برنامه مجاز به عبور و فراخوانی عملیات هستند. در صورت موفقیت آمیز بودن این درخواست ها، فرض بر این است که عیبی که قبلاً باعث خرابی شده بود برطرف شده است و کلید مدار به حالت بسته تغییر می کند (شمارگر خرابی تنظیم مجدد شده است). اگر هر درخواستی با شکست مواجه شود، مدار شکن فرض می‌کند که خطا همچنان وجود دارد، بنابراین به حالت باز برمی‌گردد و تایمر زمان‌بندی را مجدداً راه‌اندازی می‌کند تا به سیستم یک دوره زمانی بیشتر برای بازیابی از خرابی بدهد.

حالت**Half-Open** برای جلوگیری از نابود شدن ناگهانی request ها در یک سرویس recoverی مفید است. زمانی که یک سرویس بازیابی می‌شود، ممکن است بتواند حجم محدودی از درخواست‌ها را تا زمانی که بازیابی کامل شود پشتیبانی کند، اما در حالی که بازیابی در حال انجام است، حجم زیاد کار می‌تواند باعث شود که سرویس time out  بشود یا دوباره از کار بیفتد.

![[Pasted image 20230827191845.png]]

در شکل بالا، شمارشگر خرابی (failure counter) مورد استفاده در حالت بسته به صورت time based است و به طور خودکار در فواصل زمانی reset می شود. این به جلوگیری از ورود circuit breaker به حالت باز در صورت بروز خرابی های گاه به گاه کمک می کند. آستانه خرابی که مدار شکن را به حالت باز می‌رساند، تنها زمانی به دست می‌آید که تعداد معینی از خرابی در یک بازه زمانی مشخص رخ داده باشد. شمارنده مورد استفاده در حالت نیمه باز، تعداد تلاش های موفقیت آمیز برای فراخوانی عملیات را ثبت می کند. پس از موفقیت آمیز بودن تعداد معینی از فراخوانی های متوالی، کلید مدار به حالت بسته باز می گردد. اگر هر فراخوانی ناموفق باشد، قطع کننده مدار بلافاصله وارد حالت باز می شود و دفعه بعد که وارد حالت نیمه باز شد، شمارنده success دوباره reset می شود.


نکته: نحوه بازیابی سیستم به صورت خارجی انجام می شود، احتمالاً با بازیابی یا راه اندازی مجدد یک مؤلفه خراب یا تعمیر اتصال شبکه.


الگوی Circuit Breaker پایداری و ثباتی را فراهم می کند در حالی که سیستم پس از  fail  شدن به سرعت recover می‌شود و تأثیر آن بر performance را به حداقل می رساند. همینطور این الگو می‌تواند با رد کردن سریع request عملیاتی که احتمال شکست آن وجود دارد، به جای اینکه منتظر بمانید تا time out یا دیگر برنگردد، زمان پاسخ سیستم را حفظ کنید. اگر circuit breaker هر بار که تغییر حالت می‌دهد، رویدادی را مطرح می‌کند، این اطلاعات می‌تواند برای نظارت بر سلامت بخشی از سیستم که توسط circuit breaker محافظت می‌شود، یا برای هشدار دادن به administrator هنگامی که یک circuit breaker به حالت باز می‌رود، استفاده شود.

این الگو قابل تنظیم است و با توجه به نوع خرابی احتمالی قابل تنظیم است. به عنوان مثال، شما می توانید یک تایمر افزایش مدت زمان را برای قطع کننده مدار اعمال کنید. می توانید مدارشکن را در ابتدا برای چند ثانیه در حالت باز قرار دهید و سپس اگر خرابی برطرف نشد مدت زمان را به چند دقیقه افزایش دهید و به همین ترتیب. در برخی موارد، به جای بازگرداندن حالت باز شکست و ایجاد یک exception، بازگرداندن یک مقدار پیش‌فرض که برای برنامه معنادار است می‌تواند مفید باشد.

## Issues and considerations



هنگام تصمیم گیری در مورد نحوه اجرای این الگو باید نکات زیر را در نظر بگیرید:

**Exception Handling**. برنامه‌ای که عملیاتی را از طریق  circuit breaker فراخوانی می‌کند، باید برای رسیدگی به exceptionهای مطرح شده در صورت در دسترس نبودن عملیات آماده شود. نحوه رسیدگی به exceptionها مختص برنامه خواهد بود. به عنوان مثال، یک برنامه کاربردی می تواند به طور موقت عملکرد خود را کاهش دهد، یک عملیات جایگزین را برای انجام همان کار یا به دست آوردن همان داده ها فراخوانی کند، یا exception را به کاربر گزارش دهد و از او بخواهد که بعداً دوباره امتحان کند.

**Types of Exceptions**. یک درخواست ممکن است به دلایل زیادی با شکست مواجه شود، که برخی از آنها ممکن است نشان دهنده نوع شدیدتر شکست نسبت به سایرین باشد. به عنوان مثال، یک درخواست ممکن است به دلیل از کار افتادن یک سرویس ریموت حدود چند دقیقه طول بکشد تا recover شود یا به دلیل timeout به دلیل بارگیری موقت سرویس، با شکست مواجه شود. یک مدارشکن ممکن است بتواند انواع استثناهایی را که رخ می‌دهند بررسی کند و بسته به ماهیت این استثناها، استراتژی خود را تنظیم کند. به عنوان مثال، ممکن است به تعداد بیشتری از زمان‌های استثنا نیاز داشته باشد تا قطع‌کننده مدار در حالت باز قرار گیرد در مقایسه با تعداد خرابی‌های ناشی از در دسترس نبودن کامل سرویس.

**Logging**. یک قطع کننده مدار باید تمام درخواست های ناموفق (و احتمالاً درخواست های موفقیت آمیز) را ثبت کند تا مدیر بتواند بر سلامت عملیات نظارت کند.


**Recoverability**. حتما باید circuit breaker را طوری پیکربندی کنید که با الگوی recovery احتمالی عملیاتی که از آن محافظت می کند مطابقت داشته باشد. به عنوان مثال، اگر قطع کننده مدار برای مدت طولانی در حالت باز بماند، حتی اگر دلیل خرابی برطرف شده باشد، می تواند exception ایجاد کند. به طور مشابه، قطع کننده مدار می تواند در صورت تغییر سریع از حالت باز به حالت نیمه باز، نوسان داشته باشد و زمان پاسخگویی برنامه ها را کاهش دهد.

**Testing Failed Operations**. در حالت باز، به جای استفاده از تایمر برای تعیین زمان تغییر به حالت نیمه باز، یک قطع کننده مدار می تواند به طور دوره ای سرویس یا منبع ریموت را پینگ کند تا مشخص کند که آیا دوباره در دسترس است یا خیر. این پینگ می تواند به شکل تلاشی برای فراخوانی عملیاتی باشد که قبلاً ناموفق بوده است، یا می تواند از یک عملیات ویژه ارائه شده توسط سرویس راه دور به طور خاص برای آزمایش سلامت سرویس استفاده کند، همانطور که در الگوی [Health Endpoint Monitoring pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring) توضیح داده شده است.


**Manual Override**. در سیستمی که زمان بازیابی برای یک عملیات ناموفق بسیار متغیر است، ارائه یک گزینه تنظیم مجدد دستی که به administrator امکان می‌دهد قطع کننده مدار را ببندد (و شمارنده failure را reset کند) سودمند است. به طور مشابه، اگر عملیات محافظت شده توسط قطع کننده مدار موقتاً در دسترس نباشد، یک administrator می تواند یک circuit breaker را به حالت باز وادار کند (و تایمر زمان وقفه را مجدداً راه اندازی کند).

**Concurrency**. هر قطع کننده مدار می تواند توسط تعداد زیادی از نمونه(instances) های همزمان یک برنامه قابل دسترسی باشد. پیاده سازی نباید request های همزمان را مسدود کند یا به هر فراخوانی به یک عملیات سربار اضافی اضافه کند.

**Resource Differentiation**. اگر ممکن است چندین independent provider  وجود داشته باشد، هنگام استفاده از یک قطع کننده مدار برای یک resource بسیار محتاط باشید. به عنوان مثال، در یک data store که حاوی چندین قطعه است، ممکن است یک مورد کاملاً در دسترس باشد در حالی که دیگری مشکلی موقتی را تجربه می‌کند. اگر پاسخ‌های خطا در این سناریوها ادغام شوند، یک برنامه ممکن است سعی کند به برخی از موارد دسترسی داشته باشد، حتی زمانی که احتمال شکست بسیار زیاد است، در حالی که دسترسی به موارد دیگر ممکن است مسدود شود، حتی اگر احتمال موفقیت آن وجود داشته باشد.


**Accelerated Circuit Breaking**. گاهی اوقات یکfailure response می تواند حاوی اطلاعات کافی باشد تا کلید مدار فوراً خاموش شود و برای  زمان کوتاهی خاموش بماند. به عنوان مثال، پاسخ خطا از یک shared resource که overloaded شده است می تواند نشان دهد که retry کردن به هیچ وجه توصیه نمی شود و در عوض برنامه باید چند دقیقه دیگر دوباره try کند.


نکته:

یک سرویس می‌تواند HTTP 429 (Requests‌های خیلی زیاد) را در صورتی که client را throttling می‌کند، یا HTTP 503 (سرویس در دسترس نیست) را اگر سرویس در حال حاضر در دسترس نباشد، برگرداند. پاسخ می تواند شامل اطلاعات اضافی، مانند مدت زمان پیش بینی شده تاخیر باشد.


**Replaying Failed Requests**.  یک  circuit breaker در حالت سوییچ باز  به جای اینکه به سرعت از کار بیفتد می تواند جزئیات هر درخواست را در یک journal ثبت کند و ترتیبی دهد که این درخواست ها زمانی که resource یا سرویس ریموت در دسترس قرار می گیرد، دوباره اجرا شوند.

**Inappropriate Timeouts on External Services**. قطع کننده مدار ممکن است نتواند به طور کامل از برنامه ها در برابر عملیاتی که در سرویس های خارجی که با یک دوره زمانی طولانی پیکربندی شده اند و با شکست مواجه می شوند، محافظت کند. اگر بازه زمانی بیش از حد طولانی باشد، ممکن است thread‌ای که از یک قطع کننده مدار استفاده می‌کند، برای مدت طولانی مسدود شود، قبل از اینکه قطع کننده مدار نشان دهد که عملکرد شکست خورده است. در این زمان، بسیاری از نمونه های کاربردی دیگر نیز ممکن است سعی کنند سرویس را از طریق قطع کننده مدار فراخوانی کنند و تعداد قابل توجهی از رشته ها را قبل از اینکه همه آنها شکست بخورند، ببندند.

## When to use this pattern

از این الگو استفاده کنید:

برای جلوگیری از trying یک application برای فراخوانی یک سرویس remote یا دسترسی به shared resource در صورت احتمال شکست این عملیات.

این الگو توصیه نمی شود:

برای مدیریت دسترسی به local private resources در یک برنامه کاربردی، مانندin-memory data structure. در این محیط، استفاده از قطع کننده مدار باعث افزایش سربار(overhead) به سیستم شما می شود.  

به عنوان جایگزینی برای رسیدگی به استثناها در منطق تجاری برنامه های شما.

## Example

در یک application وب، چندین صفحه با داده های بازیابی شده از یک سرویس خارجی پر می شوند. اگر سیستم حداقل caching را پیاده سازی کند، بیشتر بازدیدها به این صفحات باعث یک رفت و برگشت(round trip) به سرویس می شود. اتصالات از برنامه وب به سرویس را می توان با یک بازه زمانی (معمولاً 60 ثانیه) پیکربندی کرد و اگر سرویس در این زمان پاسخ ندهد، logic هر صفحه وب فرض می کند که سرویس در دسترس نیست و یک exception ایجاد می کند.

با این حال، اگر سرویس از کار بیفتد و سیستم بسیار busy باشد، کاربران باید حدود تا 60 ثانیه منتظر بمانند تا یک exception رخ دهد. در نهایت منابعی مانند حافظه، connections و thread ها ممکن است تمام شود و از اتصال سایر کاربران به سیستم جلوگیری کند، حتی اگر به صفحاتی که داده ها را از سرویس بازیابی می کنند دسترسی نداشته باشند.

مقیاس‌بندی(Scaling) سیستم با افزودن وب سرورهای بیشتر و اجرای load balancing ممکن است تا زمانی که منابع تمام می‌شوند به تأخیر بیفتد، اما این مشکل را حل نمی‌کند زیرا درخواست‌های کاربر همچنان پاسخگو نیستند و همه سرورهای وب ممکن است در نهایت منابع خود را تمام کنند.

کلاس CircuitBreaker اطلاعات وضعیت را در مورد circuit breaker در یک object که interface ICircuitBreakerStateStore نشان داده شده در کد زیر را پیاده سازی می کند، حفظ می کند.

```csharp
interface ICircuitBreakerStateStore
{
  CircuitBreakerStateEnum State { get; }

  Exception LastException { get; }

  DateTime LastStateChangedDateUtc { get; }

  void Trip(Exception ex);

  void Reset();

  void HalfOpen();

  bool IsClosed { get; }
}
```

مشخصه State وضعیت فعلی circuit breaker را نشان می دهد و همانطور که توسط شمارش CircuitBreakerStateEnum تعریف شده است، Open، HalfOpen یا Closed خواهد بود. اگر کلید مدار بسته باشد، ویژگی IsClosed باید درست باشد، اما اگر باز یا نیمه باز باشد، باید نادرست باشد. روش Trip وضعیت circuit breaker را به حالت باز تغییر می دهد و استثنایی را که باعث تغییر حالت شده است به همراه تاریخ و ساعت وقوع استثنا ثبت می کند. ویژگی های LastException و LastStateChangedDateUtc این اطلاعات را برمی گرداند. روش Reset نیز circuit breaker را می بندد و روش HalfOpen نیر  circuit breaker را روی نیمه باز قرار می دهد.

کلاس InMemoryCircuitBreakerStateStore در این مثال شامل پیاده سازی interface ICircuitBreakerStateStore است. کلاس CircuitBreaker نمونه ای از این کلاس را ایجاد می کند تا وضعیت CircuitBreaker را حفظ کند.

متد ExecuteAction در کلاس CircuitBreaker عملیاتی را که به عنوان یک Action delegate مشخص شده است، بسته بندی (wraps) می کند. اگر قطع کننده مدار بسته باشد، ExecuteAction نماینده Action را فراخوانی می کند. اگر عملیات ناموفق باشد، یک کنترل کننده خطا TrackException را فراخوانی می کند، که وضعیت circuit breaker را برای باز کردن تنظیم می کند. مثال کد زیر این جریان را برجسته می کند.

```csharp
public class CircuitBreaker
{
  private readonly ICircuitBreakerStateStore stateStore =
    CircuitBreakerStateStoreFactory.GetCircuitBreakerStateStore();

  private readonly object halfOpenSyncObject = new object ();
  ...
  public bool IsClosed { get { return stateStore.IsClosed; } }

  public bool IsOpen { get { return !IsClosed; } }

  public void ExecuteAction(Action action)
  {
    ...
    if (IsOpen)
    {
      // The circuit breaker is Open.
      ... (see code sample below for details)
    }

    // The circuit breaker is Closed, execute the action.
    try
    {
      action();
    }
    catch (Exception ex)
    {
      // If an exception still occurs here, simply
      // retrip the breaker immediately.
      this.TrackException(ex);

      // Throw the exception so that the caller can tell
      // the type of exception that was thrown.
      throw;
    }
  }

  private void TrackException(Exception ex)
  {
    // For simplicity in this example, open the circuit breaker on the first exception.
    // In reality this would be more complex. A certain type of exception, such as one
    // that indicates a service is offline, might trip the circuit breaker immediately.
    // Alternatively it might count exceptions locally or across multiple instances and
    // use this value over time, or the exception/success ratio based on the exception
    // types, to open the circuit breaker.
    this.stateStore.Trip(ex);
  }
}
```

مثال زیر کدی را نشان می دهد (که از مثال قبلی حذف شده است) که در صورت بسته نبودن circuit breaker اجرا می شود. ابتدا بررسی می کند که آیا مدار شکن برای مدتی بیشتر از زمان مشخص شده توسط فیلد محلی OpenToHalfOpenWaitTime در کلاس CircuitBreaker باز بوده است یا خیر. اگر اینطور باشد، متد ExecuteAction نیز circuit breaker را روی نیمه باز تنظیم می کند، سپس سعی می کند عملیات مشخص شده توسط delegate Action را انجام دهد.

در صورت موفقیت آمیز بودن عملیات، circuit breaker به حالت بسته بازنشانی می شود. اگر عملیات با شکست مواجه شود، به حالت باز برمی‌گردد و زمان وقوع exception به‌روزرسانی می‌شود تا circuit breaker قبل از تلاش مجدد برای انجام عملیات، مدت بیشتری منتظر بماند.

علاوه بر این، از یک lock برای جلوگیری از تلاش ق circuit breaker برای برقراری تماس همزمان با عملیات در حالی که نیمه باز است، استفاده می کند. تلاش همزمان برای فراخوانی عملیات به گونه‌ای انجام می‌شود که گویی circuit breaker باز است و با استثنایی که بعداً توضیح داده شد با شکست مواجه می‌شود.


```csharp
...
    if (IsOpen)
    {
      // The circuit breaker is Open. Check if the Open timeout has expired.
      // If it has, set the state to HalfOpen. Another approach might be to
      // check for the HalfOpen state that had be set by some other operation.
      if (stateStore.LastStateChangedDateUtc + OpenToHalfOpenWaitTime < DateTime.UtcNow)
      {
        // The Open timeout has expired. Allow one operation to execute. Note that, in
        // this example, the circuit breaker is set to HalfOpen after being
        // in the Open state for some period of time. An alternative would be to set
        // this using some other approach such as a timer, test method, manually, and
        // so on, and check the state here to determine how to handle execution
        // of the action.
        // Limit the number of threads to be executed when the breaker is HalfOpen.
        // An alternative would be to use a more complex approach to determine which
        // threads or how many are allowed to execute, or to execute a simple test
        // method instead.
        bool lockTaken = false;
        try
        {
          Monitor.TryEnter(halfOpenSyncObject, ref lockTaken);
          if (lockTaken)
          {
            // Set the circuit breaker state to HalfOpen.
            stateStore.HalfOpen();

            // Attempt the operation.
            action();

            // If this action succeeds, reset the state and allow other operations.
            // In reality, instead of immediately returning to the Closed state, a counter
            // here would record the number of successful operations and return the
            // circuit breaker to the Closed state only after a specified number succeed.
            this.stateStore.Reset();
            return;
          }
        }
        catch (Exception ex)
        {
          // If there's still an exception, trip the breaker again immediately.
          this.stateStore.Trip(ex);

          // Throw the exception so that the caller knows which exception occurred.
          throw;
        }
        finally
        {
          if (lockTaken)
          {
            Monitor.Exit(halfOpenSyncObject);
          }
        }
      }
      // The Open timeout hasn't yet expired. Throw a CircuitBreakerOpen exception to
      // inform the caller that the call was not actually attempted,
      // and return the most recent exception received.
      throw new CircuitBreakerOpenException(stateStore.LastException);
    }
    ...


```

برای استفاده از یک object CircuitBreaker برای محافظت از یک عملیات، یک برنامه یک نمونه (instance) از کلاس CircuitBreaker ایجاد می کند و متد ExecuteAction را فراخوانی می کند و عملیاتی را که باید به عنوان پارامتر انجام شود را مشخص می کند. اگر عملیات به دلیل باز بودن CircuitBreaker شکست خورد، برنامه باید آماده باشد تا exception CircuitBreakerOpenException را بگیرد. کد زیر یک مثال را نشان می دهد:

```csharp
var breaker = new CircuitBreaker();

try
{
  breaker.ExecuteAction(() =>
  {
    // Operation protected by the circuit breaker.
    ...
  });
}
catch (CircuitBreakerOpenException ex)
{
  // Perform some different action when the breaker is open.
  // Last exception details are in the inner exception.
  ...
}
catch (Exception ex)
{
  ...
}
```


