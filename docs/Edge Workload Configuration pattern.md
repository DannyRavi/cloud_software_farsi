
# ‏Edge Workload Configuration pattern

 
تنوع زیاد سیستم‌ها و دستگاه‌ها در محیط تولیدی می‌تواند پیکربندی workload را به یک مشکل دشوار تبدیل کند. این مقاله روش‌هایی برای حل آن ارائه می‌دهد.

 
## زمینه و مشکل

 
شرکت‌های تولیدی، به‌عنوان بخشی از سفر مهاجرت به دنیای دیجیتال خود، به طور فزاینده‌ای بر ساخت راه‌حل‌های نرم‌افزاری تمرکز می‌کنند که می‌توانند به‌عنوان قابلیت‌های مشترک مورداستفاده مجدد قرار گیرند. باتوجه‌به انواع دستگاه‌ها و سیستم‌ها در طبقه فروشگاه، بارهای کاری (workload) ماژولار برای پشتیبانی از پروتکل‌ها، درایورها و فرمت‌های مختلف داده پیکربندی شده‌اند. گاهی اوقات حتی چندین نمونه از یک workload با پیکربندی‌های مختلف در یک مکان لبه (edge location) اجرا می‌شوند. برای برخی از workload ها، پیکربندی‌ها بیش از یک‌بار در روز به‌روزرسانی می‌شوند؛ بنابراین، مدیریت پیکربندی به طور فزاینده‌ای برای مقیاس‌بندی به‌صورت scaling out راه‌حل‌های مربوط به لبه (edge) اهمیت دارد.

 
## راه‌حل

 
چند ویژگی مشترک مدیریت پیکربندی برای workloadها لبه وجود دارد: 

 
* چندین مدیریت پیکربندی وجود دارد که می‌توان آنها را در لایه‌های مجزا دسته‌بندی کرد، مانند: 

 
*‏ software source

 
*‏ CI/CD pipeline

 
*‏ cloud tenant

 
*‏ edge location

 
 
![](../assets/design_implementation/edge-workload-configuration-1.png)

 
 
*‏ لایه‌های مختلف می‌تواند توسط افراد مختلف به‌روزرسانی شود.

 
*‏ مهم نیست که چگونه پیکربندی‌ها به‌روزرسانی می‌شوند، آن‌ها باید به‌دقت ردیابی و ممیزی شوند.

 
*‏ برای تداوم کسب‌وکار تجاری، لازم است که به تنظیمات در لبه به‌صورت آفلاین دسترسی داشت.

 
*‏ همچنین لازم است که یک نمای کلی از تنظیمات موجود در فضای ابری وجود داشته باشد.

 
## مسائل و ملاحظات: 

 
هنگام تصمیم‌گیری در مورد نحوه اجرای این الگوی، نکات زیر را در نظر بگیرید: 
*‏ مجوز ویرایش درصورتی‌که اتصال لبه (edge) به ابر برقرار نیست، پیچیدگی مدیریت پیکربندی را به میزان قابل‌توجهی افزایش می‌دهد. تکرار تغییرات در ابر امکان‌پذیر است، اما چالش‌هایی به‌صورت زیر دارد: 

 
 *‏ احراز هویت کاربر (authentication)، زیرا به یک سرویس ابری مانند Microsoft Entra ID وابسته است.

 
 *‏ تضاد و تداخل پس از اتصال مجدد(reconnection)، اگر workloadها تنظیمات غیرمنتظره‌ای را که نیاز به دست‌کاری دارند را دریافت کنند.

 
*‏ اگر توپولوژی مطابق با الزامات [ISA-95](https://en.wikipedia.org/wiki/ANSI/ISA-95) باشد، محیط لبه می‌تواند محدودیت‌های مرتبط با شبکه داشته باشد. شما می‌توانید با انتخاب فناوری ای که اتصال به لایه‌ها را ارائه می‌دهد، مانند [device hierarchies](https://learn.microsoft.com/en-us/azure/iot-edge/tutorial-nested-iot-edge) در [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge)، بر چنین محدودیت‌هایی غلبه کنید.

 
*‏ اگر تنظیمات زمان اجرا (run-time) از نسخه‌های اجرایی نرم‌افزار جدا شود، باید تغییرات پیکربندی به طور جداگانه انجام شود. برای ارائه تاریخچه و ویژگی‌های rollback ، باید تنظیمات گذشته را در یک ذخیره‌گاه داده در فضای ابری ذخیره کنید.

 
*‏ یک خطای در یک پیکربندی، مانند یک اتصال که به یک end-point ای که وجود خارجی ندارد، می‌تواند workload را تخریب کند؛ بنابراین، بهینه‌سازی تغییرات پیکربندی مهم است؛ زیرا شما با سایر رخدادهای چرخه عمر استقرار(deployment lifecycle) ;i در راه‌حل مشاهده‌پذیری رفتار می‌کنید باید بررسی کنید، به‌طوری‌که observability dashboard می‌توانند به همبستگی خطاهای سیستم در تغییرات پیکربندی کمک کنند. برای اطلاعات بیشتر در مورد مشاهده، به راهنمای نظارت برابر مراجعه کنید ([Cloud monitoring guide: Observability](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/manage/monitor/observability)).

 
*‏ نقش‌ها و وظایفی را که محیط ابری و ذخیره‌گاه داده در لبه (edge datastores) در پیوستگی مسائل تجاری برنامه بازی می‌کنند را به‌خوبی درک کنید. اگر ذخیره‌ساز ابری داده‌ها تنها منبع موجود باشد، باید workload لبه با استفاده از فرایندهای خودکار بتواند حالت‌های موردنظر را بازیابی کند.

 
*‏ برای انعطاف‌پذیری بیشتر، ذخیره‌ساز داده در لبه باید به‌عنوان حافظه cache آفلاین عمل کند. این مورد بر ملاحظات تأخیر زمانی اولویت دارد.

 
## چه زمانی از این الگو استفاده کنیم؟

 
از این الگو زمانی استفاده کنید که: 
*‏ نیاز به پیکربندی workloadها خارج از چرخه انتشار نرم‌افزار وجود دارد.

 
*‏ افراد مختلف باید بتوانند پیکربندی‌ها را بخوانند و به‌روزرسانی کنند.

 
*‏ حتی اگر هیچ اتصالی به فضای ابری وجود نداشته باشد، تنظیمات مربوط به آن باید در دسترس باشند.

 
نمونه workload:
*‏ راه‌حل‌هایی که برای دریافت داده‌ها به دارایی‌های موجود در فروشگاه متصل می‌شوند - برای مثال OPC Publisher - و فرمان و کنترل

 
*‏ workload یادگیری ماشین برای نگهداری و تعمیرات قابل‌پیش‌بینی شده

 
*‏ workload یادگیری ماشینی که در زمان واقعی کیفیت را در خط تولید بررسی می‌کنند

 
## مثال

راه‌حل برای پیکربندی‌های لبه در طول زمان اجرا (run-time) می‌تواند بر اساس یک کنترل‌کننده پیکربندی خارجی یا یک ارائه‌دهنده پیکربندی داخلی باشد.

 
### تغییر کنترلر پیکربندی خارجی

 
![](../assets/design_implementation/edge-workload-configuration-2.png)

 
 
این تغییرات دارای یک کنترلر پیکربندی است که خارج از workload است. نقش مؤلفه کنترلر پیکربندی ابری این است که ویرایش‌ها را از ذخیره‌گاه داده (datastore) ابری به workload از طریق کنترلر پیکربندی لبه هدایت کند. لبه همچنین حاوی یک ذخیره‌سازی اطلاعات است تا سیستم حتی در صورت قطع ارتباط با ابر کار کند.
با لبه در اینترنت اشیای (IoT Edge)، کنترل‌کننده پیکربندی لبه را می‌توان به‌عنوان یک ماژول پیاده‌سازی کرد و تنظیمات را می‌توان به ماژول دوقلو ([module twins](https://learn.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-module-twins)) اعمال کرد. این ماژول دوقلو دارای محدودیت در اندازه خود است. اگر پیکربندی از حد مجاز فراتر رفت، راه‌حل را می‌توان با [extended with Azure Blob Storage](https://github.com/Azure-Samples/azure-iot-hub-large-twin-example) یا با تقسیم بارهای بزرگ‌تر از طریق روش‌های مستقیم
([direct methods](https://learn.microsoft.com/en-us/azure/iot-edge/how-to-edgeagent-direct-method)) گسترش داد.
برای مثال از ابتدا تا انتها در تغییرات کنترل‌کننده پیکربندی خارجی، [Connected factory signal pipeline](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/iot/connected-factory-signal-pipeline) را ببینید.
مزایای این تغییرات عبارت‌اند از: 

 
*‏ خود workload نباید از سیستم پیکربندی آگاه باشد. اگر کد منبع workload قابل‌ویرایش نباشد برای مثال، هنگام استفاده از یک ماژول از [Azure IoT Edge Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)، این قابلیت الزامی است.
*‏ این امکان وجود دارد که پیکربندی چندین workload را هم‌زمان با هماهنگ‌کردن تغییرات از طریق کنترلر پیکربندی ابری تغییر دهید.

 
*‏ اعتبارسنجی اضافی را می‌توان به‌عنوان بخشی از push pipeline اجرا کرد - به‌عنوان‌مثال، برای تأیید وجود endpoint در لبه قبل از push کردن پیکربندی به workload آن را بررسی کرد.

 
### تنوع ارائه‌دهنده پیکربندی داخلی

 
![edge-workload-configuration-3](../assets/design_implementation/edge-workload-configuration-3.png)

 
 
در تغییرات ارائه‌دهنده پیکربندی داخلی، workload پیکربندی‌ها را از یک ارائه‌دهنده پیکربندی pull می‌کند. برای مثال مورد پیاده‌سازی، به [Implement a custom configuration provider in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/custom-configuration-provider). مراجعه کنید. این مثال از سی‌شارپ استفاده می‌کند، اما می‌توان از زبان‌های دیگر نیز استفاده کرد.
در این تغییر، workloadها دارای شناسه‌های منحصربه‌فرد هستند به‌طوری‌که کد منبع یکسانی که در محیط‌های مختلف اجرا می‌شود، می‌تواند پیکربندی‌های متفاوتی داشته باشد. یکی از راه‌های ساخت یک شناسه، الحاق رابطه سلسله‌مراتبی workloadها به یک گروه سطح بالا مانند tenantها است. برای IoT Edge، می‌تواند ترکیبی از Azure resource group و IoT hub name و IoT Edge device name و شناسه ماژول باشد. این مقادیر با هم یک شناسه منحصربه‌فرد را تشکیل می‌دهند که به‌عنوان یک کلید در ذخیره‌گاه داده کار کند.
اگرچه نسخه هر ماژول را می‌توان به مشخصه منحصربه‌فرد اضافه کرد و این یک نیاز رایج برای تداوم پیکربندی‌ها در به‌روزرسانی‌های نرم‌افزار است. گرچه نسخه نرم‌افزار بخشی از مشخصه آن است، نسخه قدیمی پیکربندی باید با یک پیاده‌سازی اضافی به مرحله بعد منتقل شود.
مزایای این تغییرات عبارت‌اند از: 

 
*‏ برخلاف ذخیره‌ساز داده‌ها، راه‌حل کلی به اجزای مختلف نیاز ندارد و پیچیدگی کلی را کاهش می‌دهد.

 
*‏ منطق مهاجرت از نسخه‌های قدیمی ناسازگار را می‌توان در به‌کارگیری مناسب از workload انجام داد.

 
### راه حلی مبتنی بر IoT Edge

![edge-workload-configuration-4](../assets/design_implementation/edge-workload-configuration-4.png)


مؤلفه ابری پیاده‌سازی IoT Edge شامل یک هاب اینترنت اشیا است که به‌عنوان کنترل‌کننده پیکربندی ابر عمل می‌کند. عملکرد ماژول دوقلو [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub) تغییرات پیکربندی و اطلاعات مربوط به پیکربندی اعمال شده در حال حاضر را با استفاده از ویژگی‌های موردنظر و گزارش شده ماژول منتشر می‌کند. سرویس مدیریت پیکربندی که به‌عنوان منبع پیکربندی‌ها عمل می‌کند. همچنین می‌تواند از یک رابط کاربری برای مدیریت پیکربندی‌ها یا یک build system یا سایر ابزارهایی که برای ساخت پیکربندی workloadها استفاده کنند.
یک پایگاه‌داده [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) تمام تنظیمات را ذخیره می‌کند. این کار می‌تواند با چندین هاب اینترنت اشیا تعامل داشته باشد و تاریخچه پیکربندی را ارائه دهد.
پس از اینکه یک دستگاه لبه(edge device) از طریق ویژگی‌های گزارش شده، نشان داد که یک پیکربندی اعمال شده است، سرویس وضعیت پیکربندی رویداد را در نمونه‌ی پایگاه داده ذخیره می‌کند.
هنگامی که یک پیکربندی جدید در سرویس مدیریت پیکربندی ایجاد می‌شود در ادامه در Azure Cosmos DB ذخیره می‌شود و ویژگی‌های موردنظر ماژول لبه در هاب IoT که دستگاه در آن قرار دارد تغییر می‌کند. سپس پیکربندی توسط IoT Hub به دستگاه لبه منتقل می‌شود. انتظار می‌رود که ماژول لبه پیکربندی را اعمال کند و از طریق ماژول وضعیت پیکربندی را گزارش دهد. سپس سرویس وضعیت پیکربندی به رویدادهای تغییر دوگانه گوش می‌دهد و با تشخیص اینکه یک ماژول تغییر وضعیت پیکربندی را گزارش می‌دهد، تغییر مربوطه را در پایگاه داده Azure Cosmos DB ثبت می‌کند.
اجزای لبه می‌تواند از کنترلر پیکربندی خارجی یا ارائه‌دهنده پیکربندی داخلی استفاده کند. در هر دو پیاده‌سازی، پیکربندی موردنظر یا در ویژگی‌های دلخواه ماژول دوقلو منتقل می‌شود یا در صورت نیاز به انتقال پیکربندی‌های بزرگ، ویژگی‌های موردنظر ماژول دوقلو حاوی URL به [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs) یا به سرویس دیگری است که می‌تواند برای بازیابی پیکربندی استفاده شود. سپس ماژول در ویژگی‌های گزارش شده ماژول دوقلو سیگنال می‌دهد که آیا پیکربندی جدید با موفقیت اعمال شده است؟ همین‌طور چه پیکربندی در حال حاضر اعمال می‌شود؟

 
## مشارکت‌کنندگان

 
این مقاله توسط مایکروسافت نگهداری می‌شود. در اصل توسط مشارکت‌کنندگان زیر نوشته شده است.
نویسنده اصلی: 

 
- [Heather Camm](https://www.linkedin.com/in/heather-camm-2367ba15/) | Senior Program Manager

 
 
 
## قدم بعدی

 
- [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge)
- [What is Azure IoT Edge?](https://learn.microsoft.com/en-us/azure/iot-edge/about-iot-edge)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub)
- [IoT Concepts and Azure IoT Hub](https://learn.microsoft.com/en-us/azure/iot-hub/iot-concepts-and-iot-hub)
- [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db)
- [Welcome to Azure Cosmos DB](https://learn.microsoft.com/en-us/azure/cosmos-db/introduction)
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs)
- [Introduction to Azure Blob storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)

 
[](./Edge%20Workload%20Configuration%20pattern.md)

 
## منابع مرتبط

 
- [External Configuration Store pattern](./External%20Configuration%20Store%20pattern.md)