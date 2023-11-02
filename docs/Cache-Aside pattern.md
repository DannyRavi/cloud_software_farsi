داده ها را در صورت نیاز  در حافظه پنهان از یک data store بارگیری کنید. این کار می تواند عملکرد سیستم را بهبود بخشد و  به حفظ ثبات بین داده های ذخیره شده در حافظه پنهان (cache) و داده های موجود در سایر data store ها کمک کند.
## Context and problem

برنامه ها از حافظه پنهان (cache) برای بهبود دسترسی متوالی به اطلاعات نگهداری شده در یک data store استفاده می کنند. با این حال این بسیار غیرعملی است که انتظار داشته باشیم داده‌های cache همیشه کاملاً با داده‌های data store (شامل دیسک و...) مطابقت داشته باشند. برنامه‌ها باید استراتژی‌ای را پیاده‌سازی کنند تا مطمئن باشند که داده‌های موجود در حافظه نهان تا حد امکان به‌روز هستند، همینطور ‌بتوانند موقعیت‌هایی را که زمانی که داده‌های موجود در cache قدیمی شده‌اند، شناسایی و مدیریت کنند.

## Solution

بسیاری از سیستم‌های ذخیره‌سازی تجاری(commercial caching systems) ، عملیات خواندن را به صورت read-through و عملیات نوشتن را  به صورت write-through/write-behind  را ارائه می‌کنند. در این سیستم ها، یک برنامه داده ها را با مراجعه به حافظه پنهان (cache) بدست می‌آورد. اگر داده‌ها در حافظه پنهان (cache) نباشد، از ذخیره‌گاه داده(data store) بازیابی شده و به حافظه پنهان اضافه می‌شود. هر گونه تغییر در داده های ذخیره شده در حافظه cache به طور خودکار به data store نیز بازگردانی و sync می شود.  
  
برای حافظه های نهان  (cache) که این قابلیت گفته شده در بالا را ارائه نمی دهند، این وظیفه بر روی برنامه هایی است که از  حافظه های نهان  (cache) برای نگهداری داده ها استفاده می کنند.
  
یک application  می‌تواند عملکرد ذخیره‌سازی خواندنی (read-through) را با پیاده‌سازی استراتژی Cache-Aside پیاده سازی کند. این استراتژی، داده ها را در صورت لزوم در حافظه پنهان بارگذاری می کند.
شکل زیر استفاده از الگوی Cache-Aside را برای ذخیره داده ها در حافظه پنهان نشان می دهد.

![[assets/Pasted image 20231102151446.png]]
اگر برنامه‌ای اطلاعات را به‌روزرسانی کند، می‌تواند با انجام اصلاحات در data store اصلی که میتوان دیسک یا موارد مشابه باشد  به همراه نامعتبر کردن  item مربوطه در حافظه پنهان، استراتژی write-through را دنبال کند.  
  
هنگامی که item بعدی مورد نیاز است، استفاده از ache-aside strategy در حافظه پنهان باعث می‌شود که داده‌های به‌روز شده از data store بازیابی شده و دوباره به حافظه پنهان اضافه شوند.



## Issues and considerations

هنگام تصمیم گیری در مورد نحوه اجرای این الگو به نکات زیر توجه کنید:  
  
**طول عمر داده های cache شده:** بسیاری از cache ها یک خط مشی یا policy  انقضا دار را اجرا می کنند که داده ها را بی‌اعتبار می کند و در صورت عدم دسترسی به آن داده برای یک دوره مشخص، آن ها را از حافظه پنهان حذف می کند. برای اینکه cache-aside موثر باشد، مطمئن شوید که خط مشی انقضا (expiration policy) با الگوی دسترسی برنامه هایی که از داده ها استفاده می کنند مطابقت دارد. همینطور در پیاده سازی‌ها مدت زمان انقضا را خیلی کوتاه نکنید زیرا این امر می تواند باعث شود برنامه ها به طور مداوم داده ها را از data store بازیابی کرده و به حافظه پنهان اضافه کنند. به طور مشابه، دوره انقضا را آنقدر طولانی نکنید که داده های ذخیره شده در حافظه پنهان احتمالاً قدیمی و بی فایده شوند. به یاد داشته باشید که حافظه پنهان برای داده های نسبتا ثابت یا داده هایی که مکررا خوانده می شوند مؤثرتر است.  
  
**استخراج داده ها:** اکثر حافظه‌های پنهان یا cache ها در مقایسه با data store ای که داده‌ها از آنجا منشأ می‌گیرند، اندازه محدودی دارند و در صورت لزوم، داده‌ها را برداشت و استخراج می‌کنند. اکثر حافظه‌های پنهان سیاستی را که least-recently-used نامیده می‌شود را برای انتخاب مواردی برای استخراج اتخاذ می‌کنند، که این مسئله تا حدی ممکن است قابل تنظیم کردن (customizable) باشد. در ادامه ویژگی انقضای global و سایر خصوصیات cache را تنظیم کنید و ویژگی انقضای هر آیتم cache شده را برای اطمینان از مقرون به صرفه بودن استفاده حافظه پنهان را بررسی کنید. به عنوان مثال، اگر بازیابی یک آیتم حافظه پنهان از data store بسیار پرهزینه است، نگه داشتن این آیتم در حافظه پنهان به قیمت مواردی که بیشتر در دسترس هستند اما هزینه کمتری دارند، می تواند مفید باشد.


**چکش کار (**Priming**) کش:** در بسیاری از روش‌ها، حافظه پنهان را با داده‌هایی که احتمالاً یک برنامه به عنوان بخشی از پردازش راه‌اندازی(startup) به آن نیاز دارد، پر می‌کنند. اگر برخی از این داده‌ها منقضی شوند یا استخراج  شوند در این حالت نیز الگوی Cache-Aside همچنان می‌تواند مفید باشد.  
  
**ثبات (Consistency):** پیاده سازی الگوی Cache-Aside ثبات و هماهنگی بین data store و cache را تضمین نمی کند. یک item در  data store داده را می‌توان در هر زمان با یک فرآیند خارجی تغییر کند و این تغییر ممکن است تا دفعه بعدی که item بارگیری شود در حافظه پنهان منعکس نشود. در سیستمی که replicates data  در  data storeها وجود دارد. اگر همگام‌سازی مکرر و چند مرتبه اتفاق بیفتد این مشکل می‌تواند بحرانی شود.  
  
**کشLocal (in-memory):** یک کش می تواند local برای یک نمونه برنامه باشد و در حافظه ذخیره شود. اگر یک برنامه مکرراً به داده های مشابه دسترسی داشته باشد، Cache-aside می تواند در این محیط مفید باشد. با این حال، یک حافظه پنهان local به صورت private است و بنابراین نمونه های مختلف برنامه می توانند هر کدام یک کپی از همان داده های حافظه پنهان داشته باشند. این داده‌ها می‌توانند به سرعت بین حافظه‌های پنهان دیگر ناسازگار شوند، بنابراین ممکن است لازم باشد داده‌هایی که در یک حافظه پنهان private نگهداری می‌شوند منقضی شوند و داده‌ها دفعات بیشتری refresh شوند. در این حالت، بررسی استفاده از مکانیسم ذخیره سازی caching مشترک یا توزیع شده را در نظر بگیرید.


## When to use this pattern

از این الگو زمانی استفاده کنید که:  
  
* کش عملیات read-through و write-throughبه صورت native را ارائه نمی دهد.   
* تقاضای منابع غیرقابل پیش بینی است. این الگو برنامه‌ها را قادر می‌سازد تا داده‌ها را براساس تقاضا بارگیری کنند. هیچ فرضی در مورد اینکه یک برنامه از قبل به کدام داده نیاز دارد را نمی کند. 

این الگو ممکن است مناسب نباشد:  
  
* هنگامی که مجموعه داده های ذخیره شده ثابت(static) است. اگر داده‌ها در فضای کش موجود جا می‌شوند، اول حافظه پنهان را با داده‌ها در هنگام راه‌اندازی پر کنید و سیاستی را اعمال کنید که از منقضی شدن داده‌ها جلوگیری می‌کند.  
* برای حالت caching session باید اطلاعات  در یکweb application host موجود باشد . در این محیط، باید از معرفی وابستگی های مبتنی بر پیوستگی client-server اجتناب کنید.


## Example

در Microsoft Azure می توانید از Azure Cache برای Redis برای ایجاد یک کش توزیع شده استفاده کنید که می تواند توسط چندین نمونه از یک برنامه به اشتراک گذاشته شود.  
  
این مثال کد زیر از سرویس گیرنده[StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) استفاده می کند که یک کتابخانه کلاینت Redis است که برای دات نت نوشته شده است. برای اتصال به Azure Cache برای مثال Redis، متد `ConnectionMultiplexer.Connect` ثابت(static) را فراخوانی کرده و connection string را ارسال کنید. این روش یک `ConnectionMultiplexer` را برمی گرداند که نشان دهنده برقراری اتصال است. یکی از روش‌های اشتراک‌گذاری یک نمونه `ConnectionMultiplexer` در برنامه شما، داشتن یک ویژگی ثابت است که یک نمونه متصل شده را برمی‌گرداند، مشابه مثال زیر. این رویکرد یک روش ایمن و thread-safe برای مقداردهی اولیه تنها یک نمونه متصل فراهم می کند.

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


متد GetMyEntityAsync در مثال کد زیر اجرای الگوی Cache-Aside را نشان می دهد. این روش با استفاده از رویکرد خواندنی(read-through)، یک شی را از حافظه پنهان بازیابی می کند.

یک شی با استفاده از یک شناسه عدد صحیح به عنوان کلید شناسایی می شود. متد GetMyEntityAsync سعی می کند یک آیتم را با این کلید از حافظه پنهان بازیابی کند. اگر یک مورد منطبق پیدا شد درنتیجه برگردانده می شود. اگر هیچ تطبیقی در حافظه پنهان وجود نداشته باشد، متد GetMyEntityAsync شی را از یک data store بازیابی می‌کند و آن را به حافظه پنهان اضافه می‌کند و سپس آن را برمی‌گرداند. کدی که در واقع داده ها را از data store می خواند در اینجا نشان داده نمی شود، زیرا به ذخیره داده بستگی دارد. توجه داشته باشید که مورد ذخیره شده در حافظه پنهان به گونه ای پیکربندی شده است که منقضی شود تا در صورت به روز رسانی در جای دیگری از فرسوده شدن آن جلوگیری شود.

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


> نمونه ها از Azure Cache برای Redis برای دسترسی به data store و بازیابی اطلاعات از کش استفاده می کنند. برای اطلاعات بیشتر، به استفاده از کش Azure برای Redis و نحوه ایجاد یک برنامه وب با [Azure Cache for Redis](https://learn.microsoft.com/en-us/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache) و [How to create a Web App with Azure Cache for Redis](https://learn.microsoft.com/en-us/azure/redis-cache/cache-web-app-howto)مراجعه کنید .

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


> توجه داشته باشید 

> ترتیب مراحل مهم است. قبل از حذف آیتم از حافظه نهان، data store را به روز کنید. اگر ابتدا مورد ذخیره شده را حذف کنید، یک پنجره زمانی کوچک وجود دارد که کلاینت ممکن است قبل از به‌روزرسانی data store آن را مورد را واکشی کند. این منجر به از دست رفتن حافظه پنهان (به دلیل حذف آیتم از حافظه پنهان) می شود، و باعث می شود نسخه قبلی مورد از فروشگاه داده واکشی شده و دوباره به حافظه پنهان اضافه شود در  نتیجه، داده های کش قدیمی خواهد بود.


