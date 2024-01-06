
تنوع زیاد سیستم ها و دستگاه ها در <mark style="background: #FFF3A3A6;">طبقه فروشگاه</mark> می تواند پیکربندی workload را به یک مشکل دشوار تبدیل کند. این مقاله روش هایی برای حل آن ارائه می دهد.

## Context and problem

شرکت های تولیدی، به عنوان بخشی از سفر مهاجرت به دنیای دیجیتال خود، به طور فزاینده ای بر ساخت راه حل های نرم افزاری تمرکز می کنند که می توانند به عنوان قابلیت های مشترک مورد استفاده مجدد قرار گیرند. با توجه به انواع دستگاه ها و سیستم ها در طبقه فروشگاه، بارهای کاری(workload) ماژولار برای پشتیبانی از پروتکل ها، درایورها و فرمت های مختلف داده پیکربندی شده اند. گاهی اوقات حتی چندین نمونه از یک workload با پیکربندی های مختلف در یک مکان لبه(edge location) اجرا می شوند. برای برخی از بارهای کاری، پیکربندی ها بیش از یک بار در روز به update می شوند. بنابراین، مدیریت پیکربندی(configuration management) به طور فزاینده‌ای برای مقیاس‌بندی(scaling out) راه‌حل‌های  مربوط به لبه(edge) اهمیت دارد.

## Solution

چند ویژگی مشترک مدیریت پیکربندی برای بارهای کاری لبه وجود دارد:

* چندین configuration management وجود دارد که می‌توان آنها را در لایه‌های مجزا دسته‌بندی کرد، مانند:
‏software source, CI/CD pipeline, cloud tenant, and edge location

![[Pasted image 20231205104835.png]]

* لایه های مختلف می تواند توسط افراد مختلف به روزرسانی شود.  
* مهم نیست که چگونه پیکربندی ها به روزرسانی می شوند، آن‌ها باید به دقت ردیابی و ممیزی شوند.  
* برای تداوم کسب و کار(business)، لازم است که به  تنظیمات در لبه به صورت آفلاین  دسترسی داشت. 
* همچنین لازم است که یک نمای کلی از تنظیمات موجود در فضای ابری وجود داشته باشد.

## Issues and considerations

هنگام تصمیم گیری در مورد نحوه اجرای این الگوی ، نکات زیر را در نظر بگیرید:  
  
* اجازه ویرایش ها در صورتی که اتصال edge به ابر برقرار نیست، پیچیدگی configuration management را به میزان قابل توجهی افزایش می دهد. تکرار تغییرات در ابر امکان پذیر است ، اما چالش هایی به صورت زیر دارد:
	* احراز هویت کاربر(authentication) ، زیرا به یک سرویس ابری مانند Microsoft Entra ID متکی             است.  
	* تضاد و درگیری پس از اتصال مجدد(reconnection) ، اگر workloadها تنظیمات غیر منتظره ای را           که نیاز به دستکاری دارند را دریافت می کنند.    
* اگر توپولوژی مطابق با الزامات ISA-95 باشد ، محیط لبه می تواند محدودیت های مرتبط با شبکه داشته باشد. شما می توانید با انتخاب فناوری ای که اتصال به لایه ها را ارائه می دهد ، مانند [device hierarchies](https://learn.microsoft.com/en-us/azure/iot-edge/tutorial-nested-iot-edge) در [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge) ، بر چنین محدودیت هایی غلبه کنید.  
* اگر تنظیمات زمان اجرا(run-time) از نسخه های اجرایی نرم افزار جدا شود ، باید تغییرات پیکربندی به طور جداگانه انجام شود. برای ارائه تاریخچه و ویژگی های rollback ، باید تنظیمات گذشته را در یک datastore در فضای ابری ذخیره کنید.  
* یک خطای در یک پیکربندی ، مانند یک  اتصال که به یک end-point ای که وجود خارجی ندارد ، می تواند workload را بشکند. بنابراین ، بهینه سازی تغییرات پیکربندی مهم است زیرا شما با سایر رخدادهای چرخه عمر استقرار(deployment lifecycle) در راه حل مورد بررسی برخورد، خواهید کرد ، به طوری که observability dashboard می توانند به همبستگی خطاهای سیستم در تغییرات پیکربندی کمک کنند. برای اطلاعات بیشتر در مورد مشاهده ، به راهنمای نظارت بر ابر مراجعه کنید([Cloud monitoring guide: Observability](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/manage/monitor/observability)).  
* نقش ها و وظایفی  را که cloud و edge datastores در پیوستگی business بازی می کنند ، درک کنید. اگر Datastore Cloud  تنها منبع  موجود باشد، باید edge workload با استفاده از فرآیندهای خودکار بتواند حالت های مورد نظر را بازیابی کند.  
* برای انعطاف پذیری بیشتر ، Datastore Edge باید به عنوان حافظه پنهان آفلاین(offline cache) عمل کند. این مورد بر ملاحظات تأخیر(latency) اولویت دارد.

## When to use this pattern

از این الگو زمانی استفاده کنید که:  
  
* نیاز به پیکربندی workloadها خارج از چرخه انتشار نرم افزار وجود دارد.  
* افراد مختلف باید بتوانند پیکربندی ها را بخوانند و به روزرسانی کنند.  
* حتی اگر هیچ اتصالی به فضای ابری وجود نداشته باشد، تنظیمات باید در دسترس باشند.  

نمونه workload:  
  
*<mark style="background: #FFF3A3A6;"> راه‌حل‌هایی که برای دریافت داده‌ها به دارایی‌های موجود در فروشگاه متصل می‌شوند - برای مثال OPC Publisher - و فرمان و کنترل  </mark>
* ‏workload یادگیری ماشین برای نگهداری و تعمیرات قابل پیش بینی شده  
* ‏workload یادگیری ماشینی که در زمان واقعی کیفیت را در خط تولید بررسی می کنند
## Examples

راه حل برای پیکربندی ‏های لبه در طول زمان اجرا(run-time) می تواند بر اساس یک external configuration controller یا یک ارائه دهنده internal configuration باشد.

### External configuration controller variation


![[Pasted image 20231205105157.png]]

این تغییر دارای یک کنترلر پیکربندی است که خارج از workload است. نقش مؤلفه کنترلر پیکربندی ابری این است که ویرایش ها را از datastore ابری به workload از طریق کنترلر پیکربندی لبه push دهد. لبه همچنین حاوی یک ذخیره‌سازی اطلاعات است تا سیستم حتی در صورت قطع ارتباط با ابر کار کند.  
  
با IoT Edge، کنترل‌کننده پیکربندی لبه را می‌توان به‌عنوان یک ماژول پیاده‌سازی کرد، و تنظیمات را می‌توان با [module twins](https://learn.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-module-twins) اعمال کرد. module twin دارای محدودیت اندازه است. اگر پیکربندی از حد مجاز فراتر رفت، راه حل را می توان با [extended with Azure Blob Storage](https://github.com/Azure-Samples/azure-iot-hub-large-twin-example) یا با تقسیم بارهای بزرگتر از طریق روش های مستقیم([direct methods](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-edgeagent-direct-method)) گسترش داد.  
  
برای  مثال end-to-end از تغییرات کنترل‌کننده پیکربندی خارجی، [Connected factory signal pipeline](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/iot/connected-factory-signal-pipeline) را ببینید.  
  
مزایای این تغییرات عبارتند از:

* خود workload نباید از سیستم پیکربندی آگاه باشد. اگر کد منبع workload قابل ویرایش نباشد - برای مثال، هنگام استفاده از یک ماژول از  [Azure IoT Edge Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)، این قابلیت الزامی است.  
* این امکان وجود دارد که پیکربندی چندین workload را همزمان با هماهنگ کردن تغییرات از طریق کنترلر پیکربندی ابری تغییر دهید.  
* اعتبار سنجی اضافی را می توان به عنوان بخشی از push pipeline اجرا کرد - به عنوان مثال، برای تأیید وجود endpoint در لبه قبل از push کردن پیکربندی به workload.

### Internal configuration provider variation

![[Pasted image 20231205105249.png]]

در تغییرات ارائه‌دهنده پیکربندی داخلی، workload پیکربندی‌ها را از یک ارائه‌دهنده پیکربندی pull‌ می‌کند. برای مثال مورد پیاده سازی، به [Implement a custom configuration provider in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/custom-configuration-provider). مراجعه کنید. این مثال از سی شارپ استفاده می کند، اما می توان از زبان های دیگر نیز استفاده کرد.
  
در این تغییر، workloadها دارای شناسه های منحصر به فرد هستند به طوری که کد منبع یکسانی که در محیط های مختلف اجرا می شود، می تواند پیکربندی های متفاوتی داشته باشد. یکی از راه های ساخت یک شناسه، الحاق رابطه سلسله مراتبی workloadها به یک گروه سطح بالا مانند tenantها است. برای IoT Edge، می‌تواند ترکیبی از Azure resource group و  IoT hub name و IoT Edge device name و شناسه ماژول باشد. این مقادیر با هم یک شناسه منحصر به فرد را تشکیل می دهند که به عنوان یک کلید در datastoreها کار می کند.  
  
اگرچه نسخه ماژول را می‌توان به شناسه منحصربه‌فرد اضافه کرد، این یک نیاز رایج برای تداوم پیکربندی‌ها در به‌روزرسانی‌های نرم‌افزار است. اگر نسخه بخشی از شناسه است، نسخه قدیمی پیکربندی باید با یک پیاده سازی اضافی به جلو منتقل شود.  
  
مزایای این تغییرات عبارتند از:

* به غیر از datastore ها، راه‌حل به اجزای سازنده نیاز ندارد و پیچیدگی را کاهش می‌دهد.  
* منطق مهاجرت از نسخه‌های قدیمی ناسازگار را می‌توان در به کار گرفتن workload انجام داد.

### Solutions based on IoT Edge

![[Pasted image 20231205105353.png]]

مؤلفه ابری پیاده‌سازی مرجع IoT Edge شامل یک هاب اینترنت اشیا است که به عنوان کنترل‌کننده پیکربندی ابر عمل می‌کند. عملکرد [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub) module twin تغییرات پیکربندی و اطلاعات مربوط به پیکربندی اعمال شده در حال حاضر را با استفاده از ویژگی‌های مورد نظر و گزارش شده ماژول منتشر می‌کند. سرویس  configuration management به عنوان منبع پیکربندی ها عمل می کند. همچنین می‌تواند از یک رابط کاربری برای  configuration management‌ها یا یک build system یا سایر ابزارهایی  که برای ساخت workload configurationها استفاده کنند.  
  
یک پایگاه داده  [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) تمام تنظیمات را ذخیره می کند. این می تواند با چندین هاب اینترنت اشیا تعامل داشته باشد و تاریخچه پیکربندی را ارائه دهد.  
  
<mark style="background: #FFF3A3A6;">پس از اینکه یک دستگاه لبه(edge device) از طریق ویژگی های گزارش شده، نشان داد که یک پیکربندی اعمال شده است، سرویس وضعیت پیکربندی رویداد را در نمونه‌ی پایگاه داده ذخیره می کند.  </mark>
  
هنگامی که یک پیکربندی جدید در سرویس مدیریت پیکربندی ایجاد می‌شود، در Azure Cosmos DB ذخیره می‌شود و ویژگی‌های مورد نظر ماژول لبه در هاب IoT که دستگاه در آن قرار دارد تغییر می‌کند. سپس پیکربندی توسط IoT Hub به دستگاه لبه منتقل می شود. انتظار می رود که ماژول لبه پیکربندی را اعمال کند و از طریق ماژول وضعیت پیکربندی را گزارش دهد. سپس سرویس وضعیت پیکربندی به رویدادهای تغییر دوگانه گوش می دهد و با تشخیص اینکه یک ماژول تغییر وضعیت پیکربندی را گزارش می دهد، تغییر مربوطه را در پایگاه داده Azure Cosmos DB ثبت می کند.  
  
اجزای لبه می تواند از کنترلر پیکربندی خارجی یا ارائه دهنده پیکربندی داخلی استفاده کند. در هر دو پیاده‌سازی، پیکربندی مورد نظر یا در ویژگی‌های دلخواه module twin منتقل می‌شود یا در صورت نیاز به انتقال پیکربندی‌های بزرگ، ویژگی‌های مورد نظر module twin حاوی URL به [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs) یا به سرویس دیگری است که می‌تواند برای بازیابی پیکربندی استفاده شود. سپس ماژول در ویژگی‌های گزارش شده module twin سیگنال می‌دهد که آیا پیکربندی جدید با موفقیت اعمال شده است؟ همینطور چه پیکربندی در حال حاضر اعمال می‌شود؟.

## Contributors

این مقاله توسط مایکروسافت نگهداری می شود. در اصل توسط مشارکت کنندگان زیر نوشته شده است.  
  
نویسنده اصلی:

- [Heather Camm](https://www.linkedin.com/in/heather-camm-2367ba15/) | Senior Program Manager

_To see non-public LinkedIn profiles, sign in to LinkedIn._


## Next steps

- [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge)
- [What is Azure IoT Edge?](https://learn.microsoft.com/en-us/azure/iot-edge/about-iot-edge)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub)
- [IoT Concepts and Azure IoT Hub](https://learn.microsoft.com/en-us/azure/iot-hub/iot-concepts-and-iot-hub)
- [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db)
- [Welcome to Azure Cosmos DB](https://learn.microsoft.com/en-us/azure/cosmos-db/introduction)
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs)
- [Introduction to Azure Blob storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)

[](https://learn.microsoft.com/en-us/azure/architecture/patterns/edge-workload-configuration#related-resources)

## Related resources

- [External Configuration Store pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/external-configuration-store)
- 