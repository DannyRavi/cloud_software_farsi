
# ‏Cache-Aside pattern
داده‌ها را در صورت نیاز  در حافظه cache از طریق یک  ذخیره‌گاه داده (data store) بارگیری کنید. با این کار می‌توان کارایی سیستم را بهبود بخشید و  به حفظ ثبات (consistency) بین داده‌های ذخیره شده در حافظه کَش (cache) و داده‌های موجود در سایر ذخیره‌گاه داده‌ها کمک کنید.

## موضوع و مشکل

برنامه‌ها از حافظه کَش (cache) برای بهبود دسترسی متوالی به اطلاعات نگهداری شده در یک ذخیره‌گاه داده‌ استفاده می‌کنند. با این حال این بسیار غیرعملی است که انتظار داشته باشیم داده‌‌های cache همیشه کاملاً با داده‌‌های data store (شامل دیسک و...) مطابقت داشته باشند. برنامه‌ها باید استراتژی‌ای را پیاده‌سازی کنند تا مطمئن باشند که داده‌‌های موجود در cache تا حد امکان به‌روز هستند و همینطور برنامه‌ها باید ‌بتوانند موقعیت‌‌های زمانی ‌ای که داده‌‌های موجود در cache قدیمی شده‌اند را شناسایی و مدیریت کنند.

## راه حل

بسیاری از سیستم‌‌های ذخیره‌سازی تجاری(commercial caching systems) ، عملیات خواندن را به صورت read-through و عملیات نوشتن را  به صورت write-through/write-behind  را ارائه می‌کنند. در این سیستم ها، یک برنامه، داده‌‌های مورد نظر خود را با مراجعه به حافظه cache بدست می‌آورد. اگر داده‌ها در حافظه cache نباشد آنگاه از ذخیره‌گاه داده(data store) بازیابی شده و به حافظه cache اضافه می‌شود. هر گونه تغییر در داده‌های ذخیره شده در حافظه cache به طور خودکار به data store نیز بازگردانی و sync می‌شود.  
  
به طور معمول حافظه  cache این قابلیت گفته شده در بالا را ارائه نمی‌دهند بنابراین این وظیفه بر روی برنامه‌‌هایی است که از  حافظه‌‌های cache  برای نگهداری داده‌ها استفاده می‌کنند.
  
یک application  می‌تواند عملکرد ذخیره‌سازی فقط خواندنی (read-through) را با پیاده‌سازی استراتژی Cache-Aside پیاده سازی کند. این استراتژی، داده‌ها را در صورت لزوم در حافظه cache بارگذاری می‌کند.
شکل زیر استفاده از الگوی Cache-Aside را برای ذخیره داده‌ها در حافظه کَش نشان می‌دهد.

![cache-aside-diagram](../assets/dataManagement/cache-aside-diagram.png)


اگر برنامه‌ای اطلاعات را به‌روزرسانی کند در ادامه می‌تواند با انجام اصلاحات در data store اصلی که احتمالا دیسک یا موارد مشابه باشد به همراه نامعتبر کردن  item مربوطه در حافظه cache، استراتژی write-through را دنبال کند.  
  
هنگامی که item بعدی مورد نیاز است، استفاده از استراتژی cache-aside  در حافظه cache باعث می‌شود که داده‌‌های به‌روز شده از data store بازیابی شده و دوباره به حافظه کَش اضافه شوند.



## مسائل و ملاحظات

هنگام تصمیم گیری در مورد نحوه اجرای این الگو به نکات زیر توجه کنید:  
  
**طول عمر داده‌‌های cache شده:** بسیاری از cacheها یک خط مشی یا policy  انقضا دار را اجرا می‌کنند که داده‌ها را بی‌اعتبار می‌کند و در صورت عدم دسترسی به آن داده برای یک دوره مشخص، آنها را از حافظه cache حذف می‌کند. برای اینکه cache-aside موثر باشد باید مطمئن شوید که خط مشی انقضا (expiration policy) با الگوی دسترسی برنامه‌هایی که از داده‌ها استفاده می‌کنند مطابقت دارد. همینطور در پیاده سازی‌ها، مدت زمان انقضا را خیلی کوتاه نکنید زیرا این امر می‌تواند باعث شود برنامه‌ها به طور مداوم داده‌ها را از data store بازیابی کرده و به حافظه کَش اضافه کنند. به طور مشابه، دوره انقضا را آنقدر طولانی نکنید که داده‌های ذخیره شده در حافظه کَش احتمالاً قدیمی و بی‌فایده شوند. به یاد داشته باشید که حافظه cache برای داده‌های نسبتا ثابت یا داده‌هایی که مکررا خوانده می‌شوند مفیدتر است.  
  
**استخراج داده ها:** اکثر حافظه‌‌های cache در مقایسه با data store ‌ای که داده‌ها از آنجا منشأ می‌گیرند، اندازه محدودی دارند و در صورت لزوم، داده‌ها خاصی را برداشت و استخراج می‌کنند. اکثر حافظه‌‌های cache سیاستی را که least-recently-used نامیده می‌شود را برای انتخاب مواردی برای استخراج اتخاذ می‌کنند که این مسئله تا حدی ممکن است قابل تنظیم کردن (customizable) باشد. در نهایت باید ویژگی انقضای global و سایر خصوصیات cache را تنظیم کنید و ویژگی انقضای هر آیتم cache شده را برای اطمینان از مقرون به صرفه بودن استفاده حافظه cache را بررسی کنید. به عنوان مثال، اگر بازیابی یک آیتم حافظه کَش از data store بسیار پرهزینه است، نگه داشتن این آیتم در حافظه cache به قیمت مواردی که بیشتر در دسترس هستند اما هزینه کمتری دارند، می‌تواند مفیدتر باشد.


**پرایمینگ/Priming (در لغت به معنی بتونی کاری است) یا   کردن یک cache**: در بسیاری از روش‌ها، حافظه cache را با داده‌‌هایی که احتمالاً یک برنامه به عنوان بخشی از رویه راه‌اندازی(startup) خود به آن نیاز دارد، پر می‌کنند. اگر برخی از این داده‌ها منقضی شوند یا حذف  شوند در این حالت نیز الگوی Cache-Aside همچنان می‌تواند مفید باشد. 
  
**پایداری (Consistency):** پیاده سازی الگوی Cache-Aside ثبات و هماهنگی بین data store و cache را تضمین نمی‌کند. یک آیتم در  data store  را می‌تواند در هر زمانی با یک فرآیند خارجی تغییر کند و این تغییر ممکن است تا دفعه بعدی که آیتم مورد نظر بارگیری شود در حافظه cache منعکس نشود. در سیستمی که replicates data ( این مورد به معنی تکثیر پایگاه داده شناخته می‌شود، روشی برای کپی کردن داده‌ها است تا اطمینان حاصل شود که همه اطلاعات در زمان واقعی بین همه منابع داده یکسان می‌مانند.)  در  data storeها وجود دارد. اگر همگام‌سازی مکرر و چند مرتبه اتفاق بیفتد این مشکل می‌تواند بحرانی شود.  
  
**‏Local (in-memory) caching.** یک cache می‌تواند به صورت محلی (local) برای یک نمونه application باشد و در حافظه (in-memory)  ذخیره شود. اگر یک برنامه در دفعات زیادی به داده‌‌های مشابه دسترسی داشته باشد، Cache-aside می‌تواند در این محیط مناسب باشد. با این حال، این حافظه cache که به صورت local  و  private است پس نمونه‌‌های مختلف application می‌توانند هر کدام یک کپی از همان داده‌های حافظه cache داشته باشند. این داده‌ها می‌توانند به سرعت بین حافظه‌‌های cache دیگر ناسازگار و ناپایدار شوند، پس احتمالا لازم باشد داده‌‌هایی که در یک حافظه cache به صورت private نگهداری می‌شوند منقضی شوند و داده‌ها دفعات بیشتری refresh/تازه‌سازی شوند. در این حالت، بررسی استفاده از مکانیسم ذخیره سازی caching مشترک یا توزیع شده را در نظر بگیرید.


## چه زمانی از این الگو استفاده کنیم

از این الگو زمانی استفاده کنید که:  
  
*‏ استفاده از cache معمولا اجرای عملیات‌های read-through و write-through به صورت native را ارائه نمی‌دهد.   

*‏ تقاضای منابع مصرفی مورد نیاز تا حد زیادی غیرقابل پیش بینی است. این الگو برنامه‌ها را قادر می‌سازد تا داده‌ها را براساس تقاضا و نیازمندی‌ها بارگیری کنند و هیچ فرضی در مورد اینکه یک برنامه از قبل به کدام یک از داده‌ها نیاز دارد را متصور نیست. 


این الگو ممکن است مناسب نباشد:  
  
*‏ هنگامی که مجموعه داده‌‌های ذخیره شده ثابت یا استاتیک است. اگر داده‌ها در فضای cache موجود جا می‌شوند پس در مرحله اول باید حافظه cache را با داده‌‌هایی که در هنگام راه‌اندازی (startup) برنامه مورد نیاز است را پر کنید و سیاستی را اعمال کنید که از منقضی شدن داده‌ها جلوگیری می‌کند.  

*‏ برای حالت caching session باید اطلاعات  در یک میزبان وب اپلیکیشن موجود باشد . در این محیط، باید از معرفی وابستگی‌های مبتنی بر  client-server اجتناب کنید.


## مثال

در Microsoft Azure می‌توانید از Azure Cache برای پایگاه داده (https://redis.io)[Redis] جهت ایجاد یک cache توزیع شده استفاده کنید که می‌تواند توسط چندین نمونه از یک برنامه به اشتراک گذاشته شود.  
  
در این مثال؛ کد زیر از سرویس گیرنده [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) استفاده می‌کند که یک کتابخانه کلاینت Redis است که برای دات نت نوشته شده است. برای اتصال به Azure Cache برای مثال Redis، متد `ConnectionMultiplexer.Connect` ثابت (static) را فراخوانی کرده و connection string را ارسال کنید. این روش یک `ConnectionMultiplexer` را برمی‌گرداند که نشان دهنده برقراری اتصال است. یکی از روش‌‌های اشتراک‌گذاری یک نمونه `ConnectionMultiplexer` در برنامه شما، داشتن یک ویژگی استاتیک یا ثابت است که یک نمونه متصل شده را برمی‌گرداند، مشابه مثال زیر. این رویکرد یک روش ایمن و thread-safe برای مقداردهی اولیه تنها یک نمونه متصل فراهم می‌کند.

```csharp
private static ConnectionMultiplexer Connection;

// Redis connection string information
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
    return ConnectionMultiplexer.Connect(cacheConnection);
});

public static ConnectionMultiplexer Connection => lazyConnection.Value;
```


متد `GetMyEntityAsync` در مثال کد زیر، اجرای الگوی Cache-Aside را نشان می‌دهد. این روش با استفاده از رویکرد خواندنی(read-through)، یک شی را از حافظه کَش بازیابی می‌کند.

یک شی با استفاده از یک شناسه عدد صحیح به عنوان کلید شناسایی می‌شود. متد `GetMyEntityAsync` سعی می‌کند یک آیتم را با این کلید از حافظه کَش بازیابی کند. اگر یک مورد منطبق پیدا شد  پس نتیجه برگردانده می‌شود. اگر هیچ تطبیقی در حافظه کَش وجود نداشته باشد، متد `GetMyEntityAsync` شی را از یک ذخیره گاه داده یا data store بازیابی می‌کند و آن را به حافظه کَش اضافه می‌کند و سپس آن را برمی‌گرداند. کدی که در واقع داده‌ها را از data store می‌خواند در اینجا نشان داده نمی‌شود، زیرا به ذخیره داده بستگی دارد. توجه داشته باشید که مورد ذخیره شده در حافظه کَش به گونه ‌ای پیکربندی شده است که منقضی شود تا در صورت به روز رسانی در جای دیگری از فرسوده شدن آن جلوگیری شود.

```csharp
// Set five minute expiration as a default
private const double DefaultExpirationTimeInMinutes = 5.0;

public async Task<MyEntity> GetMyEntityAsync(int id)
{
  // Define a unique key for this method and its parameters.
  var key = $"MyEntity:{id}";
  var cache = Connection.GetDatabase();

  // Try to get the entity from the cache.
  var json = await cache.StringGetAsync(key).ConfigureAwait(false);
  var value = string.IsNullOrWhiteSpace(json)
                ? default(MyEntity)
                : JsonConvert.DeserializeObject<MyEntity>(json);

  if (value == null) // Cache miss
  {
    // If there's a cache miss, get the entity from the original store and cache it.
    // Code has been omitted because it is data store dependent.
    value = ...;

    // Avoid caching a null value.
    if (value != null)
    {
      // Put the item in the cache with a custom expiration time that
      // depends on how critical it is to have stale data.
      await cache.StringSetAsync(key, JsonConvert.SerializeObject(value)).ConfigureAwait(false);
      await cache.KeyExpireAsync(key, TimeSpan.FromMinutes(DefaultExpirationTimeInMinutes)).ConfigureAwait(false);
    }
  }

  return value;
}
```


> نمونه‌ها از Azure Cache برای Redis برای دسترسی به data store و بازیابی اطلاعات از cache استفاده می‌کنند. برای اطلاعات بیشتر، به استفاده از کش Azure برای Redis و نحوه ایجاد یک برنامه وب با [Azure Cache for Redis](https://learn.microsoft.com/en-us/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache) و [How to create a Web App with Azure Cache for Redis](https://learn.microsoft.com/en-us/azure/redis-cache/cache-web-app-howto)مراجعه کنید .

```csharp
public async Task UpdateEntityAsync(MyEntity entity)
{
    // Update the object in the original data store.
    await this.store.UpdateEntityAsync(entity).ConfigureAwait(false);

    // Invalidate the current cache object.
    var cache = Connection.GetDatabase();
    var id = entity.Id;
    var key = $"MyEntity:{id}"; // The key for the cached object.
    await cache.KeyDeleteAsync(key).ConfigureAwait(false); // Delete this key from the cache.
}
```




>> توجه داشته باشید 
ترتیب مراحل مهم است. قبل از حذف آیتم از حافظه کَش باید data store را به روزرسانی کنید. اگر ابتدا مورد ذخیره شده را حذف کنید، یک پنجره زمانی کوچک وجود دارد که کلاینت ممکن است قبل از به‌روزرسانی data store آن را مورد را واکشی کند. این منجر به از دست رفتن حافظه کَش (به دلیل حذف آیتم از حافظه کَش) می‌شود، و باعث می‌شود نسخه قبلی مورد از فروشگاه داده واکشی شده و دوباره به حافظه کَش اضافه شود در  نتیجه، داده‌های کش قدیمی خواهد بود.


### موضوعات مرتبط

اطلاعات زیر ممکن است هنگام اجرای این الگو مهم باشد:

-‏ الگوی [Reliable web app](https://learn.microsoft.com/en-us/azure/architecture/web-apps/guides/reliable-web-app/overview). این الگو به شما نشان می‌دهد که چگونه الگوی cache-aside را برای وب اپلیکیشن  مبتنی بر محیط ابری هستند را اعمال کنید.

-‏ [Caching Guidance](https://learn.microsoft.com/en-us/azure/architecture/best-practices/caching). اطلاعات بیشتری در مورد نحوه ذخیره سازی داده‌ها در یک راه حل ابری و مسائلی که باید هنگام پیاده سازی cache در نظر بگیرید را ارائه می‌دهد.

-‏ [Data Consistency Primer](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/dn589800(v=pandp.10)).  اپلیکیشن‌‌های مبتنی بر محیط ابری معمولاً از داده‌‌هایی استفاده می‌کنند که در  data storeها پخش شده‌اند. مدیریت و حفظ پایداری داده‌ها در این محیط یک جنبه حیاتی از سیستم است به ویژه مسائل همزمانی (concurrency)  و در دسترس بودن (availability) که ممکن است ایجاد شود. این آغازگر مسائل مربوط به سازگاری (data consistency) در میان داده‌‌های توزیع شده را توصیف و خلاصه می‌کند که چگونه یک اپلیکیشن می‌تواند پایداری احتمالی(eventual consistency) را برای حفظ در دسترس بودن داده‌ها پیاده سازی کند.