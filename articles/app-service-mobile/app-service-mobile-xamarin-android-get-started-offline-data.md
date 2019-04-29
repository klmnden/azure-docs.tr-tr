---
title: Azure mobil uygulaması (Xamarin Android) için çevrimdışı eşitlemeyi etkinleştirme
description: App Service mobil uygulaması, Xamarin Android uygulamanızda çevrimdışı veri önbellek ve eşitleme için kullanmayı öğrenin
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 7e951b9f2c2fda3c63f154b5b144addcbf65aacf
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127973"
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Xamarin.Android mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme

[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, Xamarin.Android için Azure Mobile Apps, çevrimdışı eşitleme özelliği tanıtır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile verileri--değiştirme ile mobil uygulama--etkileşime olanak tanır. Değişiklikler, yerel bir veritabanında depolanır.
Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğreticide, eğitmen istemci projeyi güncelleştirmeniz [Xamarin Android uygulaması oluşturma] Azure Mobile Apps çevrimdışı özellikleri desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özelliklerini desteklemek üzere istemci uygulamasını güncelleştirme

Azure mobil uygulama çevrimdışı özellikleri, bir çevrimdışı senaryoda olduğunuzda bir yerel veritabanıyla etkileşim kurmanıza imkan tanır. Uygulamanızda bu özellikleri kullanmak için başlatma bir [SyncContext] yerel bir depo için. Ardından tablonuzun aracılığıyla başvuru [IMobileServiceSyncTable](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable?view=azure-dotnet) arabirimi. SQLite, cihazdaki yerel deposu olarak kullanılır.

1. Visual Studio'da NuGet Paket Yöneticisi'adımda tamamlandı projesinde açın [Xamarin Android uygulaması oluşturma] öğretici.  Arama ve yükleme **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
2. ToDoActivity.cs dosyasını açın ve açıklama durumundan çıkarın `#define OFFLINE_SYNC_ENABLED` tanımı.
3. Visual Studio'da **F5** anahtarı yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme özelliği etkin önce olduğu gibi uygulama aynı şekilde çalışır. Ancak, yerel veritabanı artık bir çevrimdışı senaryoda kullanılan verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme

Bu bölümde, mobil uygulama arka ucuna bir çevrimdışı duruma benzetimini yapmak için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyicinizde, uygulamayı çevrimdışı modda olduğunu bildirir. Bu durumda, yeni öğeleri yerel depoya eklendi ve bağlı bir durumda bir anında iletme çalıştırıldığında mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projede ToDoActivity.cs düzenleyin. Değişiklik **applicationURL** geçersiz bir URL ile işaret edecek şekilde:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Çevrimdışı davranışı de, wifi ve cihazdaki hücresel ağlar devre dışı bırakma veya uçak modu kullanarak da gösterebilirsiniz.
2. Tuşuna **F5** oluşturun ve uygulamayı çalıştırın. Uygulama başlatıldığında yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme her tıkladığınızda [CancelledByNetworkError] durumu ile başarısız olduğunu fark **Kaydet**. Ancak mobil uygulama arka ucuna gönderilen kadar yeni todo öğeleri yerel depoda mevcut.  Bu özel durumlar bastır hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklayıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosuna ve içeriği için göz atabilirsiniz. Arka uç veritabanı verilerini değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak mobil arka uç, sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka ucunuzu yeniden bağlanmak için uygulamayı güncelleştirme

Bu bölümde, mobil uygulama arka ucu için uygulamayı yeniden bağlanın. Uygulama ilk kez çalıştırdığınızda `OnCreate` olay işleyicisi çağrılarını `OnRefreshItemsSelected`. Bu yöntemin çağırdığı `SyncAsync` yerel deponuzun arka uç veritabanı ile eşitlenecek.

1. Paylaşılan projede ToDoActivity.cs açın ve, değişikliği geri **applicationURL** özelliği.
2. Tuşuna **F5** anahtarını yeniden oluşturun ve uygulamayı çalıştırın. Uygulama, yerel değişikliklerinizi gönderme ve çekme işlemleri kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnRefreshItemsSelected` yöntemini yürütür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanarak güncelleştirilmiş verileri görüntüleyin. Veri Uyarısı, Azure mobil uygulaması arka uç veritabanı ve yerel depo arasında eşitlenmiş.
4. Uygulamada, yerel depoya tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `CheckItem` çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğe için. `SyncAsync` hem İtme hem de çekme çağırır. **Bir çekme istemci değişiklikler yaptı tabloya karşı yürüttüğünüz her bir anında iletme her zaman otomatik olarak yürütülmeden**. Bu, tüm tabloları ilişkileri yanı sıra Yerel Depodaki tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir. Bu davranışı hakkında daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirin

Öğretici tamamlandığında, indirdiğiniz Xamarin istemci projesi [Xamarin Android uygulaması oluşturma] zaten yerel bir SQLite veritabanından kullanarak çevrimdışı eşitlemeyi destekleyen kodunu içerir. Eğitmen kodu zaten dahil, kısa bir genel bakış aşağıda verilmiştir. Özellik kavramsal bir genel bakış için bkz: [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

* Yerel depo, herhangi bir tablo işlem gerçekleştirilmeden önce başlatılmalıdır. Yerel depo veritabanı başlatılır, `ToDoActivity.OnCreate()` yürütür `ToDoActivity.InitLocalStoreAsync()`. Bu yöntem, yerel bir SQLite veritabanı kullanarak oluşturur `MobileServiceSQLiteStore` Azure Mobile Apps istemci SDK'sı tarafından sağlanan sınıfı.

    `DefineTable` Yöntemi sağlanan türü alanları ile eşleşen yerel deposunda bir tablo oluşturur `ToDoItem` bu durumda. Türü, uzak veritabanındaki tüm sütunları eklemek gerekmez. Yalnızca bir sütun alt kümesi depolamak mümkündür.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see https://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* `toDoTable` Üyesi `ToDoActivity` değil `IMobileServiceSyncTable` türü yerine `IMobileServiceTable`. Tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri yerel depolama alanı veritabanına IMobileServiceSyncTable yönlendirir.

    Değişiklikleri Azure mobil uygulama arka ucuna çağırarak zaman itilir karar `IMobileServiceSyncContext.PushAsync()`. Tablo ilişkileri izleme ve bir istemci uygulaması değiştiren tüm tablolardaki değişiklikleri gönderme tarafından korumak eşitleme bağlamı yardımcı `PushAsync` çağrılır.

    Sağlanan kod çağrıları `ToDoActivity.SyncAsync()` todoıtem liste yenilenir veya bir todoıtem eklendiğinde veya tamamlanmış olduğunda eşitlenecek. Yerel her değişiklikten sonra kodun eşitler.

    Sağlanan kod içinde tüm kayıtları uzaktan `TodoItem` tablosunu sorgulanan, ancak sorgu kimliği geçirerek kayıtlarını filtrelemek ve için sorgular mümkündür `PushAsync`. Daha fazla bilgi için konudaki *Artımlı eşitleme* içinde [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Ek Kaynaklar

* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobile Apps .NET SDK nasıl yapılır][8]

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: ./app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: ./app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: https://xamarin.com/download
[Xamarin extension]: https://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
