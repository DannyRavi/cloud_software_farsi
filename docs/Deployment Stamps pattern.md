الگوی Deployment Stamps  شامل (قوانین) provisioning، مدیریت و نظارت یا مانیتورینگ بر یک گروه ناهمگن از منابع برای میزبانی و بهره برداری از workloadها یا tenantهای متعدد است. هر کپی منفرد یک _stamp_ یا گاهی اوقات یک _service unit_, _scale unit_ یا _cell_ نامیده می شود. در یک محیط  دارای چند tenant، هر واحد stamp یا scale می‌تواند به تعداد از پیش تعریف‌شده tenant سرویس دهی کند. <mark style="background: #FFF3A3A6;">stampهای متعددی را می توان به کار برد تا راه حل  را تقریباً خطی کند و به تعداد فزاینده ای از tenantآن سرویس دهی کند. </mark>این رویکرد می‌تواند مقیاس‌پذیری(scalability) راه‌حل شما را بهبود بخشد، به شما امکان می‌دهد نمونه‌هایی را در چندین منطقه deploy کنید و داده‌های مشتری خود را جدا کنید.

## Context and problem

هنگام میزبانی(hosting) یک برنامه در فضای ابری، ملاحظات خاصی وجود دارد. یکی از نکات کلیدی که باید در نظر داشته باشید عملکرد و قابلیت اطمینان برنامه شما است. اگر یک نمونه از راه حل خود را hosting می کنید، ممکن است مشمول محدودیت های زیر باشید:  
  
* **Scale limits**. deployment یا deploy یک نمونه از برنامه شما ممکن است منجر به محدودیت های scaling به صورت طبیعی شود. برای مثال، ممکن است از سرویس هایی استفاده کنید که محدودیت هایی در تعداد اتصالات ورودی، نام هاست، سوکت های TCP یا منابع دیگر دارند.  
* **مقیاس بندی غیر خطی یا افزایش هزینه.** برخی از مؤلفه‌های مورد نظر شما ممکن است scale خطی نداشته باشند و با افزایش تعداد request‌ها یا  داده ها این احتمال افزایش یابد. <mark style="background: #FFF3A3A6;">همینطور، زمانی که شرط‌های یک آستانه برآورده شود، ممکن است کاهش ناگهانی در عملکرد یا افزایش هزینه ها وجود داشته باشد.</mark> برای مثال، ممکن است از یک پایگاه داده استفاده کنید و متوجه شوید که هزینه نهایی اضافه کردن ظرفیت بیشتر (scaling up) گران‌تر می‌شود و استراتژی کاهش مقیاس‌دهی( scaling out)  راه حل مقرون‌ به‌ صرفه‌تری است.  
* **جداسازی مشتریان (Separation of customers)**. ممکن است لازم باشد داده‌های مشتریان خاص را از داده‌های مشتریان دیگر جدا نگه دارید. به طور مشابه، ممکن است مشتریانی داشته باشید که به منابع سیستم بیشتری نسبت به سایرین نیاز دارند و آنها را در زیرساخت های مختلف گروه بندی کنید.  
* **رسیدگی به موارد تک و چند tenantای.** ممکن است مشتریان بزرگی داشته باشید که به نمونه های مستقل خود از راه حل شما نیاز دارند. همچنین ممکن است مجموعه ای از مشتریان کوچکتر داشته باشید که می توانند یک deployment چند tenant را به اشتراک بگذارند.  
* **الزامات deployment پیچیده.** ممکن است لازم باشد به‌روزرسانی‌ها را به شیوه‌ای کنترل‌شده در سرویس خود اجرا کنید و در زمان‌های مختلف در زیرمجموعه‌های مختلف زیرساخت های مربوط مشتریان خود deploy کنید.  
* **فرکانس به روز رسانی Update frequency** ممکن است مشتریانی داشته باشید که به‌روزرسانی‌های مکرر سیستم شما را تحمل می‌کنند، در حالی که برخی دیگر ممکن است ریسک گریز باشند و بخواهند سیستمی که درخواست‌هایشان را پاسخ می‌دهد به‌روزرسانی‌های نادری انجام شود. ممکن است منطقی باشد که این مشتریان در محیط های ایزوله deploy شوند.  
* **محدودیت های جغرافیایی یا ژئوپلیتیکی.** برای داشتن یک معماری با تاخیر کم و سرعت مناسب یا رعایت الزامات حاکمیت داده(data sovereignty)، ممکن است برخی از مشتریان خود را در مناطق خاصی deploy کنید.

این محدودیت‌ها اغلب برای فروشندگان نرم‌افزار مستقل independent software vendors (ISVs) که نرم‌افزار را به‌عنوان سرویس (SaaS) می‌سازند، که اغلب برای چند tenant بودن طراحی می‌شوند، اعمال می‌شود. با این حال، همین محدودیت ها می تواند برای سناریوهای دیگر نیز اعمال شود.

## Solution

برای جلوگیری از این مسائل، منابع را در _scale unitها_ گروه بندی کنید و چندین نسخه از _stampهای_ خود تهیه کنید. هر scale unit میزبان زیرمجموعه ای از tenantهای شما خواهد بود و به آنها سرویس‌دهی می کند. stampها مستقل از یکدیگر عمل می کنند و می توانند به طور مستقل deploy و update شوند. یک منطقه جغرافیایی منفرد ممکن است حاوی یک یا چندین stamp باشد تا امکان کاهش مقیاس دهی افقی(horizontal scale-out) در داخل منطقه را فراهم کند. stampها شامل زیرمجموعه ای از مشتریان شما هستند.

![[Pasted image 20231204000549.png]]

گزینه Deployment stamps   چه در حالت  استفاده از infrastructure as a service (IaaS) یا  platform as a service (PaaS) یا حالت های ترکیبی از این دو به کار می‌رود. معمولاً workload مورد استفاده در IaaS نیاز به دستکاری بیشتری برای مقیاس بندی(scale) دارند، بنابراین این الگو ممکن است برای workloadهای سنگین در IaaS مفید باشد تا امکان کاهش مقیاس(scaling out) را فراهم کند.  
  
از stampها می توان برای اجرای حلقه های deployment([deployment rings](https://learn.microsoft.com/en-us/azure/devops/migrate/phase-rollout-with-rings)) استفاده کرد. اگر مشتریان مختلف بخواهند به‌روزرسانی‌های خدمات را در زمان‌های مختلف دریافت کنند، می‌توان آن‌ها را در stampهای مختلف گروه‌بندی کرد و هر stamp می‌تواند به‌روزرسانی‌هایی را در وزن‌های متفاوتی داشته باشد.  
  
از آنجا که stampها به طور مستقل از یکدیگر اجرا می شوند، داده ها به طور ضمنی <mark style="background: #FFF3A3A6;">خرد(_sharded_)</mark> می شوند. علاوه بر این، یک stamp می‌تواند از <mark style="background: #FFF3A3A6;">خرد(sharding)</mark> شدن بیشتر داده‌ها استفاده کند تا به صورت داخلی امکان مقیاس‌پذیری و elasticity درون stamp را فراهم کند.  
  
الگوی deployment stamp  به صورت داخلی توسط بسیاری از سرویس‌های Azure از جمله App Service، Azure Stack و Azure Storage استفاده می‌شود.  
  
‏Deployment stamps  با اینکه مربوط به [geodes](https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes) هستند، اما متمایز از  آنها نیز هستند. در معماری Deployment stamps، چندین نمونه مستقل از سیستم شما Deploy می شوند که شامل زیرمجموعه ای از مشتریان و کاربران شما هستند. در  [geodes](https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes)، همه نمونه‌ها می‌توانند درخواست‌های هر کاربر را ارائه دهند، اما این معماری اغلب برای طراحی و ساخت پیچیده‌تر است. همچنین ممکن است ترکیب دو الگو را در یک محلول در نظر بگیرید. رویکرد مسیریابی ترافیک که در زیر توضیح داده شده است نمونه ای از چنین سناریوی ترکیبی است.

### Deployment

به دلیل پیچیدگی که در deployment کپی‌های یکسان از اجزای یکسان وجود دارد، اقدامات خوب یک متخصص DevOps برای اطمینان از موفقیت در اجرای این الگو بسیار مهم است. در نظر بگیرید که زیرساخت خود را به صورت کد توصیف کنید، مانند استفاده از قالب‌های[Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview), [JSON Azure Resource Manager templates (ARM templates)](https://learn.microsoft.com/en-us/azure/azure-resource-manager/template-deployment-overview), [Terraform](https://learn.microsoft.com/en-us/azure/developer/terraform/overview), و اسکریپت‌ها. با این رویکرد، می توانید اطمینان حاصل کنید که deployment(Deployment) هر stamp قابل پیش بینی و تکرار است. همچنین احتمال خطاهای انسانی مانند عدم تطابق تصادفی در پیکربندی بین stampها را کاهش می دهد.  
  
می‌توانید به‌روزرسانی‌ها را به‌طور خودکار برای همه stampها به صورت موازی اجرا کنید، در این صورت ممکن است فناوری‌هایی مانند الگوهای [Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview) یا Resource Manager را برای هماهنگ کردن deployment زیرساخت‌ها و برنامه‌های کاربردی خود در نظر بگیرید. از طرف دیگر، ممکن است تصمیم بگیرید ابتدا به‌روزرسانی‌ها را برای برخی از stampها و سپس به تدریج برای برخی دیگر به‌روزرسانی کنید. استفاده از یک ابزار مدیریت انتشار(release management) مانند [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) یا  [GitHub Actions](https://docs.github.com/actions) را برای هماهنگ کردن Deploymentها در هر stamp در نظر بگیرید. برای اطلاعات بیشتر:

- ‏[Integrate Bicep with Azure Pipelines](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/add-template-to-azure-pipelines)
- ‏[Integrate JSON ARM templates with Azure Pipelines](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/add-template-to-azure-pipelines)

توپولوژی اشتراک های Azure و resource groups را برای deployment خود با دقت در نظر بگیرید:  
  
* به طور معمول یک اشتراک(subscription) شامل کلیه منابع برای یک راه حل واحد خواهد بود ، بنابراین به طور کلی استفاده از یک اشتراک واحد برای همه stampها را در نظر بگیرید. با این حال ، برخی از خدمات Azure سهمیه های گسترده ای را تحمیل([Azure services impose subscription-wide quotas](https://learn.microsoft.com/en-us/azure/azure-subscription-service-limits)) می کنند ، بنابراین اگر از این الگوی استفاده می کنید تا درجه بالایی از مقیاس را فراهم کنید ، ممکن است لازم باشد که stampها را در اشتراک های مختلف در نظر بگیرید.  
* ‏Resource groupها به طور کلی برای deployment اجزای مختلف با  چرخه عمر مشخص استفاده می شوند. اگر قصد دارید به طور همزمان به روزرسانی ها را برای همه stampهای خود deploy کنید ، ا<mark style="background: #FFF3A3A6;">ستفاده از یک گروه منبع منفرد (single resource group) را در نظر بگیرید تا تمام اجزای مختلف  را برای همه stampهای خود داشته باشید و از قراردادها و برچسب های نامگذاری منابع استفاده کنید تا مؤلفه هایی را که متعلق به هر stamp هستند ، شناسایی کنید. از طرف دیگر ، اگر قصد دارید به</mark> روزرسانی های هر stamp را به طور مستقل deploy کنید ، در نظر بگیرید که هر stamp را در resource group خود deploy کنید.


### Capacity planning

از آزمایش load و performance برای تعیین تقریبی load  که یک stamp معین می تواند تحمل کند، استفاده کنید. ‌<mark style="background: #FFF3A3A6;">متریک‌های مربوط به  load ممکن است بر اساس تعداد customers/tenant هایی باشد که یک stamp می‌تواند در خود جای دهد یا متریک‌های مربوط به سرویس‌هایی که اجزای درونی stamp منتشر می‌کنند.</mark> اطمینان حاصل کنید که ابزار کافی برای اندازه گیری زمانی که یک stamp به ظرفیت نهایی خود نزدیک شده است به همراه توانایی deployment سریع stampهای جدید برای پاسخگویی به تقاضا را دارید.

### Traffic routing

الگوی Deployment Stamp اگر به هر stamp به طور مستقل پرداخته شود، به خوبی کار می کند. به عنوان مثال، اگر Contoso یک برنامه API یکسان را در چند stamp deploy کند، ممکن است استفاده از DNS را برای هدایت ترافیک به stamp مربوطه در نظر بگیرد:  
  
* ‏unit1.aus.myapi.contoso.com ترافیک را به سمت stamp unit1 در یک منطقه استرالیا هدایت می کند.  
* ‏unit2.aus.myapi.contoso.com ترافیک را به سمت stamp unit2 در یک منطقه استرالیا هدایت می کند.  
* ‏unit1.eu.myapi.contoso.com ترافیک را به سمت stamp unit1 در یک منطقه اروپایی هدایت می کند.  
سپس مشتریان مسئول اتصال به stamp صحیح هستند.  
  
اگر یک نقطه ورودی(ingress) یکسان برای تمام ترافیک عبوری مورد نیاز باشد، می‌توان از سرویس مسیریابی ترافیک(traffic routing) برای رفع stamp درخواست، مشتری یا tenant استفاده کرد. سرویس مسیریابی ترافیک یا مشتری را به URL مربوطه برای stamp هدایت می کند (به عنوان مثال، با استفاده از کد وضعیت پاسخ HTTP 302)، یا ممکن است به عنوان یک پروکسی معکوس(reverse proxy) عمل کرده و ترافیک را بدون اطلاع مشتری به stamp مربوطه ارسال کند. .  
  
یک سرویس مسیریابی ترافیک متمرکز می تواند جزء پیچیده ای برای طراحی باشد، به خصوص زمانی که یک راه حل در چندین منطقه اجرا می شود. deployment سرویس مسیریابی ترافیک را در چندین منطقه در نظر بگیرید (به طور بالقوه شامل هر منطقه ای که stampها در آن deploy شده اند)، و سپس اطمینان حاصل کنید که ذخیره داده (نگاشت tenant به stamp) هماهنگ(synchronized) است. مولفه مسیریابی ترافیک ممکن است خود با نمونه ای از الگوی [geode](https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes) باشد.  
  
به عنوان مثال، [Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/) می تواند برای عمل در نقش سرویس مسیریابی ترافیک deploy شود. این گزینه می‌تواند با جستجوی داده‌ها در مجموعه [Azure Cosmos DB](https://learn.microsoft.com/en-us/azure/cosmos-db)که نگاشت(mapping) بین tenantها و stampها را ذخیره می‌کند و در نتیجه stamp مناسب را برای درخواست کردن تعیین کند. سپس API Management می‌تواند به صورت پویا URL بک‌اند( [dynamically set the back-end URL](https://learn.microsoft.com/en-us/azure/api-management/api-management-transformation-policies#SetBackendService)) را روی سرویس API stamp مربوطه تنظیم کند.  
  
برای فعال کردن توزیع جغرافیایی(geo-distribution) درخواست‌ها و افزونگی جغرافیایی(geo-redundancy) سرویس مسیریابی ترافیک، API Management را می‌توان در چندین منطقه deploy کرد یا از [Azure Front Door](https://learn.microsoft.com/en-us/azure/frontdoor/) برای هدایت ترافیک به نزدیک‌ترین نمونه(instance) استفاده کرد. Front Door را می‌توان با یک [backend pool](https://learn.microsoft.com/en-us/azure/frontdoor/front-door-backend-pool#backend-pools) پیکربندی کرد و درخواست‌ها را به نزدیک‌ترین نمونه API Management موجود هدایت کرد. اگر برنامه شما از طریق HTTP/S نمایش یا در معرض قرار  داده نمی‌شود، می‌توانید از متعادل‌کننده بار Azure بین منطقه‌ای([cross-region Azure Load Balancer](https://learn.microsoft.com/en-us/azure/load-balancer/cross-region-overview)) برای توزیع تماس‌های دریافتی بین Azure Load Balancer منطقه‌ای استفاده کنید. از ویژگی[global distribution feature of Azure Cosmos DB](https://learn.microsoft.com/en-us/azure/cosmos-db/distribute-data-globally) می توان برای به روز نگه داشتن اطلاعات نگاشت شده(mapping) در هر منطقه استفاده کرد.  
  
اگر یک سرویس مسیریابی ترافیک(traffic-routing) در راه حل مرود استفاده شما گنجانده شده است، در نظر بگیرید که آیا به عنوان یک [gateway](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing) عمل می کند؟  بنابراین می تواند بارگذاری دروازه([gateway offloading](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading)) را برای سرویس های دیگر، مانند authorization، throttling و validation انجام دهد.


#### Example traffic routing architecture

نمونه زیر معماری مسیریابی ترافیک زیر را در نظر بگیرید که از Azure Front Door، Azure API Management و Azure Cosmos DB برای مسیریابی ترافیک جهانی و سپس یک سری از stampهای خاص منطقه استفاده می کند:

![[Pasted image 20231204000925.png]]

فرض کنید یک کاربر به طور معمول در نیویورک زندگی می کند. داده های آنها در stamp 3، در منطقه شرق ایالات متحده ذخیره می شود.  
  
اگر کاربر به کالیفرنیا سفر کند و سپس به سیستم دسترسی پیدا کند، احتمالاً ارتباط آنها از طریق منطقه West US 2 انجام می شود زیرا در هنگام درخواست، این منطقه از نظر جغرافیایی نزدیکترین مکان است. با این حال، درخواست در نهایت باید با stamp 3 ارائه شود، زیرا داده های آنها در آنجا ذخیره می شود. سیستم مسیریابی ترافیک، اطمینان حاصل می کند که درخواست به stamp صحیح هدایت می شود.

## Issues and considerations

هنگام تصمیم گیری در مورد نحوه اجرای این الگو باید نکات زیر را در نظر بگیرید:  
  
* **‏Deployment process**. هنگام deployment چندین stamp، بسیار توصیه می شود که فرآیندهای deployment خودکار و کاملاً تکرار شونده داشته باشید. استفاده از [Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview), [JSON ARM templates](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)یا ماژول‌های [Terraform](https://learn.microsoft.com/en-us/azure/developer/terraform/overview) را در نظر بگیرید تا stamp را به صورت ثابت تعریف کنید.  
* **‏Cross-stamp operations**. هنگامی که راه حل شما به طور مستقل در چندین stamp ،‏ deploy می شود، سؤالاتی مانند: «چند مشتری در تمام stampهایمان داریم؟» پاسخ دادن را می تواند پیچیده تر شود. ممکن است لازم باشد پرس و جوها در مقابل هر stamp اجرا شوند و نتیجه‌ها یک پارچه شوند. همینطور، در نظر بگیرید که تمام stampها داده ها را در یک انبار داده(data warehouse) متمرکز برای گزارش  منتشر کنند.  
* **‏Determining scale-out policies.** ‏stampها ظرفیت محدودی دارند که ممکن است با استفاده از یک متریک یا معیار پراکسی مانند تعدادی tenantهایی که می‌توانند روی stamp ،‏ deploy شوند، تعریف شود. نظارت بر ظرفیت موجود و ظرفیت مورد استفاده برای هر stamp و به کارگیری فعال stampهای اضافی <mark style="background: #FFF3A3A6;">برای اجازه دادن به فعالیت tenantهای جدید که به سمت آنها هدایت شود، مهم است.</mark>  
* **‏Minimum number of stamps.** هنگامی که از الگوی Deployment Stamp استفاده می کنید، توصیه می شود حداقل دو stamp از راه حل خود را deploy کنید. اگر فقط یک stamp را به کار می‌گیرید، به راحتی می‌توانید مفروضاتی را به‌طور تصادفی در کد یا پیکربندی خود hard-code کنید که در زمان scale out اعمال و اجرا نمی‌شوند.  
* **هزینه**. الگوی Deployment Stamp شامل deployment چندین نسخه از مؤلفه زیرساخت شما است که احتمالاً شامل افزایش قابل توجهی در هزینه راه‌حل شما خواهد بود.  
* **حرکت بین stampها.** از آنجایی که هر stamp به طور مستقل deploy شده و کار می کند، جابجایی tenantها بین stampها می تواند دشوار باشد. برنامه شما به منطق سفارشی نیاز دارد تا اطلاعات مربوط به یک مشتری خاص را به stamp دیگری منتقل کند و سپس اطلاعات tenant را از stamp اصلی حذف کند. این فرآیند ممکن است به یک صفحه پشتی(backplane) برای ارتباط بین stampها نیاز داشته باشد که پیچیدگی راه حل کلی را بیشتر می کند.  
* **‏Traffic routing**. همانطور که در بالا توضیح داده شد، مسیریابی ترافیکی به stamp صحیح برای یک درخواست معین می‌تواند به یک مؤلفه اضافی برای حل و فصل tenantها به stampها نیاز داشته باشد. این مؤلفه، به نوبه خود، ممکن است نیاز داشته باشد که highly available باشد.  
* **اجزای مشترک(Shared components).** ممکن است اجزایی داشته باشید که بتوان آنها را در stampها به اشتراک گذاشت. به عنوان مثال، اگر یک برنامه تک صفحه ای مشترک(shared single-page app) برای همه tenantها دارید، آن را در یک منطقه deploy کنید و از  [Azure CDN](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) برای تکرار آن در سطح جهانی استفاده کنید.

## When to use this pattern

این الگو زمانی مفید است که:  
  
* **محدودیت های طبیعی در مقیاس پذیری(scalability).** به عنوان مثال، اگر برخی از مؤلفه‌ها نمی‌توانند یا نباید از تعداد معینی مشتری یا درخواست scale شوند، با استفاده از stampها گزینه scaling out نظر بگیرید.  
* الزامی برای جداسازی برخی tenantها از دیگران. اگر مشتریانی دارید که به دلیل نگرانی‌های امنیتی نمی‌توانند در یک stamp چند tenant ای با مشتریان دیگر deploy شوند، می‌توانند روی stamp جدا شده خودشان deploy شوند.  
* نیاز به داشتن چند tenant در نسخه های مختلف، به طور همزمان.  
* برنامه های چند منطقه ای که در آن داده ها و ترافیک هر tenant باید به یک منطقه خاص هدایت شود.  
* دستیابی به انعطاف پذیری در هنگام خاموشی یا قطع برق (outages). از آنجایی که stampها مستقل از یکدیگر هستند، اگر قطعی یک stamp را تحت تأثیر قرار دهد، tenantها که در stampهای دیگر deploy شده اند نباید تحت تأثیر قرار گیرند. این جداسازی به مهار «شعاع انفجار(blast radius)» یک حادثه یا قطعی برق/خاموشی کمک می کند.  

این الگو برای موارد زیر مناسب نیست:  
  
* راه حل های ساده ای که نیازی به مقیاس بندی در درجه بالایی ندارند.  
* سیستم هایی که می توانند به راحتی در یک نمونه کوچک یا بزرگ شوند، مانند افزایش اندازه لایه برنامه یا با افزایش ظرفیت رزرو شده برای پایگاه های داده و لایه ذخیره سازی.  
* راه حل هایی که در آن داده ها باید در تمام نمونه های deploy شده تکرار شوند. الگوی  [geode](https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes) را برای این سناریو در نظر بگیرید.  
* راه حل هایی که در آنها فقط برخی از مؤلفه ها باید مقیاس شوند، اما برخی دیگر نه. برای مثال، در نظر بگیرید که آیا راه‌حل شما می‌تواند با تقسیم کردن ذخیره‌گاه داده( [sharding the data store](https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding)) به جای deployment یک کپی جدید از همه اجزای راه‌حل، مقیاس‌پذیر شود یا خیر.  
* راه حل ها صرفاً از محتوای ثابت، مانند یک برنامه JavaScript از نوع front-end تشکیل شده اند. ذخیره چنین محتوایی در یک [storage account](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) و استفاده از [Azure CDN](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) را در نظر بگیرید.

## Supporting technologies

* زیرساخت به عنوان کد(Infrastructure as code) به عنوان مثال، Resource Manager templates Bicep،  Manager، Azure CLI، Terraform، PowerShell، Bash.  
* ‏Azure Front Door، که می تواند ترافیک را به یک stamp خاص یا به یک سرویس مسیریابی ترافیک هدایت کند.

## Example

مثال زیر چندین stamp از یک راه حل ساده PaaS را با یک سرویس app و یک پایگاه داده SQL در هر  stamp ،‏ deploy می کند. در حالی که stampها را می توان در هر منطقه ای پیکربندی کرد که از سرویس‌های deploy شده در الگوی مورد نظر پشتیبانی می کند، به منظور  تصور بهتر، این الگو دو stamp را در منطقه West US 2 و یک stamp دیگر را در منطقه غرب اروپا deploy می کند. در داخل یک stamp، سرویس برنامه نام میزبان DNS عمومی خود را دریافت می کند و می تواند اتصالات را مستقل از سایر stampها دریافت کند.

```
هشدار  
  
مثال زیر از SQL Server administrator account استفاده می کند. به طور کلی استفاده از یک حساب کاربری مدیریتی از برنامه شما کار خوبی نیست. برای یک برنامه واقعی، از یک هویت مدیریت شده برای اتصال از برنامه خود به پایگاه داده SQL استفاده کنید یا از یک حساب با حداقل دسترسی استفاده کنید.
```

برای deployment راه حل روی لینک زیر کلیک کنید.
[![Deploy to Azure](https://learn.microsoft.com/en-us/azure/architecture/_images/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fdeployment-stamp%2Fdeployment-stamp.json)


```
توجه داشته باشید  
  
روش‌های جایگزینی برای deployment stampها با الگوی Resource Manager وجود دارد، از جمله استفاده از الگوهای تودرتو یا الگوهای پیوندی برای جدا کردن تعریف هر stamp از تکرار مورد نیاز برای deployment چندین نسخه.
```

### Example traffic routing approach

مثال زیر پیاده سازی راه حل مسیریابی ترافیک(traffic routing) را به کار می گیرد که می تواند با مجموعه ای از stampهای deployment برای یک برنامه API فرضی استفاده شود. برای اجازه دادن به توزیع جغرافیایی درخواست‌های دریافتی، Front Door در کنار چندین نمونه از API Management در لایه مربوط به مصرف کننده deploy می‌شود. هر نمونه API Management شناسه tenant را از request URL می خواند و سپس stamp مربوط به request را از یک Azure Cosmos DB data store با توزیع جغرافیایی جستجو می کند. سپس درخواست به stamp پشتیبان مربوطه ارسال می شود.  
  
برای deployment راه حل بالا روی لینک زیر کلیک کنید.
[![Deploy to Azure](https://learn.microsoft.com/en-us/azure/architecture/_images/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fdeployment-stamp%2Ftraffic-routing.json)

## Contributors

این مقاله توسط مایکروسافت نگهداری می شود. در اصل توسط مشارکت کنندگان زیر نوشته شده است.

Principal author:

- [John Downs](http://linkedin.com/in/john-downs) | Principal Customer Engineer, FastTrack for Azure

Other contributors:

- [Daniel Larsen](http://linkedin.com/in/daniellarsennz) | Principal Customer Engineer, FastTrack for Azure
- [Angel Lopez](http://linkedin.com/in/iangellopez) | Senior Software Engineer, Azure Patterns and Practices
- [Paolo Salvatori](http://linkedin.com/in/paolo-salvatori) | Principal Customer Engineer, FastTrack for Azure
- [Arsen Vladimirskiy](http://linkedin.com/in/arsenv) | Principal Customer Engineer, FastTrack for Azure

_To see non-public LinkedIn profiles, sign in to LinkedIn._

## Related resources

* ‏Sharding می تواند به عنوان یک روش ساده تر دیگر برای کاهش سطح داده های شما استفاده شود. stampها به طور ضمنی داده های خود را خرد می کنند، اما اشتراک گذاری نیازی به Deployment Stamp ندارد. برای اطلاعات بیشتر، الگوی Sharding را ببینید.  
* اگر یک سرویس مسیریابی ترافیک deploy شود، الگوهای [Gateway Routing](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing) و [Gateway Offloading](https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading) می توانند با هم استفاده شوند تا بهترین استفاده را از این مؤلفه داشته باشند.