به کمک رای گیری/انتخاب یک نمونه از بین نمونه هایی که در یک برنامه distributed به هم وابستگی  
و collaborating دارند به عنوان مدیر و leader بقیه نمونه ها، هدف اصلی این روش است . این ایده باعث می  
شود که دیگر نمونه ها با یک دیگر conflict نداشته باشند، conflict ای که باعث ایجاد اختلاف برای مصرف منابع مشترک(shared resources) می‌شود، یا به طور ناخواسته در کاری که نمونه‌های دیگر انجام می‌دهند تداخل(interfere) ایجاد می‌کنند.

### **طرح صورت مسئله:**

یک cloud application معمولی وظایف زیادی دارد که به صورت هماهنگ عمل می کنند. این وظایف همگی می‌توانند نمونه‌هایی باشند که کد یکسانی را اجرا می‌کنند و نیاز به دسترسی به منابع یکسانی دارند، یا ممکن است به طور موازی با هم کار کنند تا بخش‌های جداگانه یک محاسبه پیچیده را انجام دهند.

نمونه‌های هر task ممکن است برای بیشتر اوقات به طور جداگانه اجرا شوند، اما ممکن است لازم باشد اقدامات هر نمونه با نمونه دیگر نیازمند هماهنگی باشد تا اطمینان حاصل شود که آنها دچار conflict نیستند یا باعث ایجاد اختلاف برای منابع مشترک نمی‌شوند، یا به طور تصادفی در اجرای task خود دچار تداخلی با سایر نمونه‌ها ایجاد نمی‌شوند.

### مثلا:

- در یک سیستم cloud-based که [horizontal scaling](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.cloudzero.com%2Fblog%2Fhorizontal-vs-vertical-scaling&si=mqo0rbtvo9ma&st=post&k=XYhwZBMzXZQ40C9oGdTFv5R8Y1RE8O%2B70veEyHkj%2BbY%3D) را پیاده‌سازی می‌کند، چندین نمونه از یک task می‌توانند همزمان با هر instance serving با کاربرهای متفاوتی اجرا شوند. اگر این نمونه‌ها در یک shared resource قابلیت ذخیره سازی و write داشته باشند، لازم است اقدامات آنها هماهنگ(coordinate) شود تا از بازنویسی یا ذخیره مجدد تغییرات ایجاد شده توسط سایر نمونه‌ها جلوگیری شود.
- اگر taskها عناصر تکی یک محاسبات پیچیده را به صورت موازی انجام می دهند، نتایج باید پس از تکمیل همه آنها جمع شوند (aggregator).
- نمونه‌های هر task منحصرد به فرد هستند، بنابراین یک leader وجود ندارد که بتواند به عنوان هماهنگ‌کننده(coordinator) یا جمع‌کننده(aggregator) عمل کند.

### راه حل:

فقط یک نمونه task باید انتخاب شود تا به عنوان leader عمل کند، و این نمونه باید اقدامات سایر موارد زیرمجموعه را هماهنگ(coordinate) کند. اگر همه taskها یک کد را اجرا کنند، هر کدام می‌توانند به عنوان leader عمل کنند. بنابراین، فرآیند انتخابات(election) باید با دقت مدیریت شود تا از تصاحب همزمان دو یا چند task به عنوان leader جلوگیری شود.

سیستم باید مکانیزمی قوی برای انتخاب leader فراهم کند. این روش باید با رویدادهایی مانند قطع شبکه یا process failures مقابله کند. در بسیاری از راه‌حل‌ها، task instanceهای فرعی؛ leader را از طریق نوعی روش heartbeat method یا با polling نظارت(monitor) می‌کنند. اگر leader تعیین‌شده به‌طور غیرمنتظره‌ای خاتمه یابد، یا خرابی شبکه، leader را برایsubordinate task instanceها از دسترس خارج کند، لازم است که leader جدیدی انتخاب کنند.

چندین استراتژی برای انتخاب یک leader از میان مجموعه ای از وظایف در یک distributed environment شده وجود دارد، از جمله:

- انتخاب task instance با کمترینinstance rank یاprocess ID.
- مسابقه برای به دست آوردن یک [mutex](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fstackoverflow.com%2Fa%2F34556&si=mqo0rbtvo9ma&st=post&k=pBQF4PMPdIDOPWEQF2O2%2BBKWvO0S32Zwsm1WSFtwTSI%3D) مشترک و توزیع شده. اولین نمونه وظیفه ای که mutex را بدست می آورد leader است. با این حال، سیستم باید اطمینان حاصل کند که در صورت پایان یا قطع ارتباط leader با بقیه سیستم، [mutex](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fstackoverflow.com%2Fa%2F34556&si=mqo0rbtvo9ma&st=post&k=pBQF4PMPdIDOPWEQF2O2%2BBKWvO0S32Zwsm1WSFtwTSI%3D) آزاد می شود تا به task instance دیگری اجازه دهد تا leader شود.
- پیاده سازی یکی از الگوریتم های رایج انتخاب رهبر مانند [Bully Algorithm](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.cs.colostate.edu%2F~cs551%2FCourseNotes%2FSynchronization%2FBullyExample.html&si=mqo0rbtvo9ma&st=post&k=38lsn%2BDxRYKpvsKm4orgMCh5%2Bkvk19vepv440AVk098%3D) یا [Ring Algorithm](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.cs.colostate.edu%2F~cs551%2FCourseNotes%2FSynchronization%2FRingElectExample.html&si=mqo0rbtvo9ma&st=post&k=FPhX7OfJuR80mXY6T8v%2FSy9r%2B6%2FrlY9Ga3FW7xY3JBM%3D). این الگوریتم‌ها فرض می‌کنند که هر نامزد در انتخابات یک شناسه منحصربه‌فرد(unique ID) دارد و می‌تواند به طور قابل اعتماد با سایر نامزدها ارتباط برقرار کند.

### مسائل و ملاحظات:

هنگام تصمیم گیری در مورد نحوه اجرای این الگو به نکات زیر توجه کنید:

- فرآیند انتخاب leader باید در برابر شکست های گذرا و مداوم (transient and persistent failures) مقاوم باشد.
- تشخیص اینکه چه زمانی leader دچار failed شده است یا unavailable شده (مثلاً به دلیل نقص ارتباطات) باید امکان پذیر باشد. سرعت تشخیص مورد نیاز به سیستم بستگی دارد. برخی از سیستم ها ممکن است بتوانند برای مدت کوتاهی بدون leader کار کنند، که در طی آن ممکن است یک خطای گذرا برطرف شود. در موارد دیگر، ممکن است لازم باشد فوراً leader failure را تشخیص داده و یک انتخابات/election جدید راه اندازی شود.
- در سیستمی که horizontal autoscaling را پیاده‌سازی می‌کند، اگر سیستم کوچک(scales back) شود و برخی از منابع محاسباتی را خاموش کند، leader می‌تواند terminated شود.
- استفاده از یک mutex مشترک و توزیع شده یک وابستگی به سرویس خارجی ارائه دهنده mutex ایجاد می کند. سرویس یک نقطه شکست واحد را تشکیل می دهد. اگر به هر دلیلی از دسترس خارج شود، سیستم قادر به انتخاب leader نخواهد بود.
- استفاده از یک فرآیند اختصاصی واحد (single dedicated process) به عنوان leader یک رویکرد خیلی ساده بوده و با این حال، اگر process با fail شود، ممکن است تاخیر قابل توجهی در هنگام restartedوجود داشته باشد. latency حاصل می تواند بر عملکرد و زمان پاسخ سایر فرآیندها تأثیر بگذارد، اگر آنها منتظر leader برای هماهنگی (coordinate) عملیات باشند.
- پیاده‌سازی یکی از leader election algorithm ها به صورت دستی، بیشترین انعطاف‌پذیری را برای تنظیم و بهینه‌سازی کد فراهم می‌کند.

**چه زمانی از این الگو استفاده کنیم؟**

از این الگو زمانی استفاده کنید که taskها در یک distributed application، مانند راه‌حل cloud-hosted، نیاز به هماهنگی دقیق دارند و هیچ leader دقیقی وجود ندارد.

> از تبدیل شدن leader به گلوگاه(bottleneck) در سیستم اجتناب کنید. هدف leader هماهنگ کردن subordinate taskهاست و لزوماً لازم نیست که خود در این کار شرکت کند - اگرچه اگر task به عنوان leader انتخاب نشود باید بتواند این کار را انجام دهد.

**چه زمانی نباید از این الگو استفاده کنیم؟**

- یک leader یا process اختصاصی وجود دارد که همیشه می تواند به عنوان leader عمل کند. برای مثال، ممکن است بتوان یک singleton process را پیاده سازی کرد که task instanceها را هماهنگ می کند. اگر این process به نوعی fail یا unhealthy شود، سیستم می تواند آن را خاموش کرده و دوباره راه اندازی کند.
- هماهنگی بین taskها را می توان با استفاده از روشی سبک تر به دست آورد. به عنوان مثال، اگر چندین task instance به سادگی نیاز به دسترسی هماهنگ به یک shared resource دارند، راه حل بهتر استفاده از [optimistic or pessimistic locking](https://virgool.io/@bulwark/%D8%AA%D9%81%D8%A7%D9%88%D8%AA-optimistic-%D9%88-pessimistic-locking-dis3nymz2yp1) برای کنترل دسترسی است.
- معمولا راه حل‌های third-party مناسب تر است. به عنوان مثال، سرویس Microsoft Azure HDInsight (بر اساس Apache Hadoop) از سرویس ارائه شده توسط Apache Zookeeper برای هماهنگ کردن نقشه (coordinate the map) و reduce task که باعث جمع آوری و خلاصه کردن داده ها می‌شود، استفاده می‌کند.

### مثال:

پروژه DistributedMutex در راه‌حل LeaderElection (نمونه‌ای که نشان می‌دهد این الگو در [GitHub](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fgithub.com%2Fmspnp%2Fcloud-design-patterns%2Ftree%2Fmaster%2Fleader-election&si=mqo0rbtvo9ma&st=post&k=iJohi4w9KP4tRuyeKJmtHxLP3SeFaSPZRYNKzL83njM%3D) موجود است) نشان می‌دهد که چگونه می‌توان با استفاده از یک Azure Storage blob برای ارائه مکانیزمی برای اجرای یک mutex مشترک و توزیع‌شده استفاده کرد. این mutex می تواند برای انتخاب یک leader در میان گروهی از role instanceها در Azure cloud service استفاده شود. اولین [role instance](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fcli%2Fazure%2Fcloud-service%2Frole-instance%3Fview%3Dazure-cli-latest&si=mqo0rbtvo9ma&st=post&k=eZfSuDDggvL4MZ3datLNIbRWsQKqJCnGXTRxaAu3c3k%3D) که [lease](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Frest%2Fapi%2Fstorageservices%2Flease-blob&si=mqo0rbtvo9ma&st=post&k=F%2FpJWTA49iBNsQt0b80qYcihd77fb8mAi8a570iP5dw%3D) را به دست می آورد، به عنوان leader انتخاب می شود و تا زمانی که lease را آزاد نکند یا نتواند مجوز را تمدید کند، leader باقی می‌ماند. در صورتی که leader دیگر در دسترس نباشد، سایر role instanceها می‌توانند به monitor کردن [blob lease](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Frest%2Fapi%2Fstorageservices%2Flease-blob&si=mqo0rbtvo9ma&st=post&k=F%2FpJWTA49iBNsQt0b80qYcihd77fb8mAi8a570iP5dw%3D) ادامه دهند.

  

> یک blob lease یک قفل نوشتن انحصاری روی یک blob است. یک blob می تواند تنها موضوع یک lease در هر نقطه از زمان باشد. یک role instance می تواند یک lease را روی یک blob مشخص درخواست کند، و اگر هیچ role instance دیگری lease را روی همان blob lease نداشته باشد، به آن lease داده می شود. در غیر این صورت درخواست یک استثنا ایجاد می کند.

> برای جلوگیری از یکrole instance معیوب که lease را به طور نامحدود حفظ می کند، محدودیت مادام العمر را برای lease تعیین کنید. وقتی این مدت منقضی شد، lease در دسترس می شود. با این حال، در حالی که یک role instance یک lease را نگه می دارد، می تواند درخواست تمدید lease را داشته باشد وleaseبرای مدت زمان بیشتری به آن اعطا می شود. اگر role instance بخواهدlease را حفظ کند، می تواند به طور مداوم این فرآیند را تکرار کند. برای اطلاعات بیشتر در مورد نحوه lease a blob مقاله؛ [Lease Blob (REST API)](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Frest%2Fapi%2Fstorageservices%2FLease-Blob&si=mqo0rbtvo9ma&st=post&k=6WkQD5tgQhiRzsWn3rMnPGBKeR%2BaQRmKZxGzYLCDhE8%3D) را ببینید.

کلاس BlobDistributedMutex در مثال سی شارپ زیر حاوی متد RunTaskWhenMutexAcquired است که یک role instance را قادر می‌سازد تا یک lease را روی یک blob مشخص به دست آورد. هنگامی که شی BlobDistributedMutex ایجاد می شود، جزئیات blob (نام، container و حساب ذخیره سازی) به سازنده در یک شی BlobSettings منتقل می شود (این شی یک ساختار ساده است که در کد نمونه گنجانده شده است). سازنده همچنین Task را می پذیرد که به کدی ارجاع می دهد کهrole instance باید اجرا کند در صورتی که lease را با موفقیت در blob به دست آورد و leader انتخاب شد. توجه داشته باشید که کدی که جزئیات سطح پایین کسب lease را مدیریت می کند در یک کلاس کمکی جداگانه به نام BlobLeaseManager پیاده سازی شده است.

```csharp
public class BlobDistributedMutex

{

...

private readonly BlobSettings blobSettings;

private readonly Func<CancellationToken, Task> taskToRunWhenLeaseAcquired;

...

  

public BlobDistributedMutex(BlobSettings blobSettings,

Func<CancellationToken, Task> taskToRunWhenLeaseAcquired)

{

this.blobSettings = blobSettings;

this.taskToRunWhenLeaseAcquired = taskToRunWhenLeaseAcquired;

}

  

public async Task RunTaskWhenMutexAcquired(CancellationToken token)

{

var leaseManager = new BlobLeaseManager(blobSettings);

await this.RunTaskWhenBlobLeaseAcquired(leaseManager, token);

}
```


متد RunTaskWhenMutexAcquired در نمونه کد بالا روش RunTaskWhenBlobLeaseAcquired را که در نمونه کد زیر نشان داده شده است فراخوانی می کند تا در واقع lease را بدست آورد. متد RunTaskWhenBlobLeaseAcquired به صورت ناهمزمان اجرا می شود. اگر lease با موفقیت به دست آید، role instance به عنوان leader انتخاب شده است. هدف از taskToRunWhenLeaseAcquired نماینده انجام کاری است که سایر role instance را هماهنگ می کند. اگر lease تملک نشود، role instance دیگری به عنوان رهبر انتخاب شده است و role instance فعلی تابع باقی می ماند. توجه داشته باشید که روش TryAcquireLeaseOrWait یک روش کمکی است که از شی BlobLeaseManager برای به دست آوردن lease استفاده می کند.

```csharp
private async Task RunTaskWhenBlobLeaseAcquired(
    BlobLeaseManager leaseManager, CancellationToken token)
  {
    while (!token.IsCancellationRequested)
    {
      // Try to acquire the blob lease.
      // Otherwise wait for a short time before trying again.
      string leaseId = await this.TryAcquireLeaseOrWait(leaseManager, token);

      if (!string.IsNullOrEmpty(leaseId))
      {
        // Create a new linked cancellation token source so that if either the
        // original token is canceled or the lease can't be renewed, the
        // leader task can be canceled.
        using (var leaseCts =
          CancellationTokenSource.CreateLinkedTokenSource(new[] { token }))
        {
          // Run the leader task.
          var leaderTask = this.taskToRunWhenLeaseAcquired.Invoke(leaseCts.Token);
          ...
        }
      }
    }
    ...
  }
```

یک task آغاز شده توسط leader نیز به صورت ناهمزمان اجرا می شود. در حالی که این کار در حال اجرا است، روش RunTaskWhenBlobLeaseAcquired نشان داده شده در نمونه کد زیر به صورت دوره ای سعی می کند lease را تمدید کند. این کمک می کند تا اطمینان حاصل شود که role instance به عنوان leader باقی می ماند. در راه حل نمونه، تاخیر بین درخواست های تمدید کمتر از زمان مشخص شده برای مدت lease است تا از انتخاب role instance دیگری به عنوان leader جلوگیری شود. اگر تمدید به هر دلیلی ناموفق باشد، کار لغو می شود.

اگر lease تمدید نشد یا کار لغو شد (احتمالاً در نتیجه خاموش شدن role instance)، lease آزاد می شود. در این مرحله، این یا role instance دیگری ممکن است به عنوان leader انتخاب شود. استخراج کد زیر این بخش از فرآیند را نشان می دهد.

```csharp
private async Task RunTaskWhenBlobLeaseAcquired(
    BlobLeaseManager leaseManager, CancellationToken token)
  {
    while (...)
    {
      ...
      if (...)
      {
        ...
        using (var leaseCts = ...)
        {
          ...
          // Keep renewing the lease in regular intervals.
          // If the lease can't be renewed, then the task completes.
          var renewLeaseTask =
            this.KeepRenewingLease(leaseManager, leaseId, leaseCts.Token);

          // When any task completes (either the leader task itself or when it
          // couldn't renew the lease) then cancel the other task.
          await CancelAllWhenAnyCompletes(leaderTask, renewLeaseTask, leaseCts);
        }
      }
    }
  }
  ...
}
```

متد KeepRenewingLease یکی دیگر از روش های کمکی است که از شی BlobLeaseManager برای تمدید lease استفاده می کند. متد CancelAllWhenAnyCompletes وظایف مشخص شده به عنوان دو پارامتر اول را لغو می کند. نمودار زیر استفاده از کلاس BlobDistributedMutex را برای انتخاب leader و اجرای task ای که عملیات را هماهنگ می کند، نشان می دهد.

![[Pasted image 20240209175344.png]]

مثال کد زیر نحوه استفاده از کلاس BlobDistributedMutex را در نقش worker نشان می دهد. این کد بر روی bolb به نام MyLeaderCoordinatorTask در lease's container در development storage، مجوز می‌گیرد و مشخص می‌کند که اگر role instance به عنوان leader انتخاب شود، کد تعریف‌شده در متد MyLeaderCoordinatorTask باید اجرا شود.

```csharp
var settings = new BlobSettings(CloudStorageAccount.DevelopmentStorageAccount,
  "leases", "MyLeaderCoordinatorTask");
var cts = new CancellationTokenSource();
var mutex = new BlobDistributedMutex(settings, MyLeaderCoordinatorTask);
mutex.RunTaskWhenMutexAcquired(this.cts.Token);
...

// Method that runs if the role instance is elected the leader
private static async Task MyLeaderCoordinatorTask(CancellationToken token)
{
  ...
}
```

در مورد کد نمونه بالا به نکات زیر توجه کنید:


- یک blob یک نقطه به شدت failure است. اگر سرویس blob در دسترس نباشد، یا غیر قابل دسترسی باشد، leader نمی تواند lease را تمدید کند و هیچ role instance دیگری نمی تواندlease را به دست آورد. در این صورت، هیچ role instance نمی تواند به عنوان leader عمل کند. با این حال، سرویس blob به گونه ای طراحی شده است که انعطاف پذیر باشد، بنابراین شکست کامل سرویس blob بسیار بعید در نظر گرفته می شود.
- اگر task ای که توسط leader انجام می شود متوقف شود، leader ممکن است به تمدید lease ادامه دهد و از گرفتن lease و به عهده گرفتن نقش leader به منظور هماهنگ کردن task ها جلوگیری کند. در دنیای واقعی، health بودن leader باید در فواصل زمانی مکرر بررسی شود.
- روند انتخابات/election غیر قطعی است. شما نمی توانید هیچ فرضی در مورد اینکه کدام role instance در نهایت blob lease را به دست می آورد و leader می شود، ندارید.
- همیشه blob مورد استفاده به عنوان هدف blob lease نباید برای هیچ هدف دیگری استفاده شود. اگر یک role instance تلاش کند داده ها را در این blob ذخیره کند، این داده ها قابل دسترسی نخواهند بود مگر اینکه role instance یک leader باشد و blob lease را نگه دارد.


### مراحل بعدی:

راهنمایی زیر ممکن است هنگام اجرای این الگو نیز مرتبط باشد:

- این الگو دارای نمونه کاربردی قابل [دانلود](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fgithub.com%2Fmspnp%2Fcloud-design-patterns%2Ftree%2Fmaster%2Fleader-election&si=mqo0rbtvo9ma&st=post&k=iJohi4w9KP4tRuyeKJmtHxLP3SeFaSPZRYNKzL83njM%3D) می باشد.
- راهنمای[Autoscaling Guidance](https://virgool.io/p/mqo0rbtvo9ma/edit). با تغییر load روی برنامه، می‌توان نمونه‌های task host را شروع و متوقف کرد. Autoscaling می تواند به حفظ توان و عملکرد در زمان اوج پردازش کمک کند.
- راهنمای پارتیشن بندی([Compute Partitioning Guidance](https://virgool.io/p/mqo0rbtvo9ma/edit)) را محاسبه کنید. این راهنما نحوهallocate task به host ها در یک سرویس ابری را توضیح می دهد به گونه ای که به حداقل رساندن هزینه های جاری و حفظ scalability، performance، در availability و امنیت سرویس کمک می کند.
- الگوی[Task-based Asynchronous pattern](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Fdotnet%2Fstandard%2Fasynchronous-programming-patterns%2Ftask-based-asynchronous-pattern-tap&si=mqo0rbtvo9ma&st=post&k=u3W6UHhWoq0wsZAldRkH7sN%2Fbiz5%2FVD5Hw8j%2FzLKYa0%3D)..
- مثالی که [Bully Algorithm](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.cs.colostate.edu%2F~cs551%2FCourseNotes%2FSynchronization%2FBullyExample.html&si=mqo0rbtvo9ma&st=post&k=38lsn%2BDxRYKpvsKm4orgMCh5%2Bkvk19vepv440AVk098%3D) را نشان می دهد.
- مثالی که [Ring Algorithm](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fwww.cs.colostate.edu%2F~cs551%2FCourseNotes%2FSynchronization%2FRingElectExample.html&si=mqo0rbtvo9ma&st=post&k=FPhX7OfJuR80mXY6T8v%2FSy9r%2B6%2FrlY9Ga3FW7xY3JBM%3D) را نشان می دهد.
- استفاده از [Apache Curator](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Fcurator.apache.org%2F&si=mqo0rbtvo9ma&st=post&k=SlkBsBnyEZL2xFhinucGd%2BS22IPNywJNpsjy05Mmlj8%3D) یک کتابخانه client برای Apache ZooKeeper.
- مقاله [Lease Blob (REST API)](https://l.vrgl.ir/r?ad=1&l=https%3A%2F%2Flearn.microsoft.com%2Fen-us%2Frest%2Fapi%2Fstorageservices%2FLease-Blob&si=mqo0rbtvo9ma&st=post&k=6WkQD5tgQhiRzsWn3rMnPGBKeR%2BaQRmKZxGzYLCDhE8%3D) در MSDN.