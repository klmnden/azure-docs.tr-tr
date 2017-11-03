---
title: "Azure mobil uygulaması (Xamarin iOS) için çevrimdışı eşitlemeyi etkinleştirme"
description: "Mobil uygulama hizmeti için önbellek ve eşitleme çevrimdışı veri Xamarin iOS uygulamanızın içinde nasıl kullanılacağını öğrenin"
documentationcenter: xamarin
author: ggailey777
manager: cfowler
editor: 
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: b5878d8a2e18cf08b6e9074ecf40cd732624f0c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Xamarin.iOS mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirin
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici Xamarin.iOS için Azure Mobile Apps, çevrimdışı eşitleme özelliği sunmaktadır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile veri--değiştirme bir mobil uygulamayla--etkileşime olanak sağlar. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğreticide, Xamarin.iOS uygulaması projeden güncelleştirme [bir Xamarin iOS uygulaması oluşturma] Azure Mobile Apps çevrimdışı özelliklerini desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için İstemci uygulamayı güncelleştirme
Azure mobil uygulama çevrimdışı özellikler, çevrimdışı bir senaryoda olduğunda yerel bir veritabanı ile etkileşim kurmasına olanak sağlar. Bu özellikler, uygulamanızda kullanmak için başlatma bir [SyncContext] yerel depolama. Tablonuz [IMobileServiceSyncTable] arabirimi aracılığıyla başvuru. SQLite yerel depolama aygıtında olarak kullanılır.

1. ' Tamamlandığı projesinde NuGet Paket Yöneticisi'ni açın [bir Xamarin iOS uygulaması oluşturma] öğretici, ardından arayın ve yükleme **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
2. QSTodoService.cs dosyasını açın ve açıklama durumundan çıkarmanız `#define OFFLINE_SYNC_ENABLED` tanımı.
3. Yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme etkinleştirilmeden önce yaptığınız gibi uygulamayı aynı şekilde çalışır. Ancak, yerel veritabanı, artık çevrimdışı bir senaryoda kullanılabilir verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme
Bu bölümde, bir çevrimdışı duruma benzetimini yapmak için mobil uygulama arka ucu için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyici uygulama çevrimdışı modda olduğunu söyler. Bu durumda, yeni öğeler yerel depoya eklendi ve anında iletme bağlı bir durumda İleri çalıştırıldığında mobil uygulama arka ucuna senkronize edilir.

1. QSToDoService.cs paylaşılan projesinde düzenleyin. Değişiklik **applicationURL** geçersiz bir URL'ye yönlendirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, wifi ve aygıt cep telefonu ağlarda devre dışı bırakma veya uçak modu kullanılarak çevrimdışı davranışı sergiler.
2. Derleme ve uygulamayı çalıştırın. Uygulama başlatıldığında üzerinde yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme her tıkladığınızda [CancelledByNetworkError] durumu ile başarısız fark **kaydetmek**. Ancak, mobil uygulama arka ucuna gönderilemez kadar yeni Yapılacaklar öğelerini yerel depoda mevcut.  Bu özel durumlar bastırmak hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamasını kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Bir Bilgisayara Visual Studio yüklüyse açmak **Sunucu Gezgini**. Veritabanınızda gidin **Azure**-> **SQL veritabanları**. Veritabanınıza sağ tıklatıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosunda ve içeriği göz atabilirsiniz. Arka uç veritabanındaki verileri değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak, mobil arka uç sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka yeniden bağlanmak için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucu uygulamaya yeniden bağlanın. Bu, mobil uygulama arka ucu ile çevrimiçi duruma bir çevrimdışı durumdan taşınılacak uygulama benzetimini yapar.   Ağ bağlantısını devre dışı bırakarak ağ arıza benzetimli, hiçbir kod değişiklikleri gereklidir.
Ağ yeniden etkinleştirin.  Uygulama ilk kez çalıştırdığınızda `RefreshDataAsync` yöntemi çağrılır. Bu sırayla çağırır `SyncAsync` yerel deponuza arka uç veritabanı ile eşitlemek için.

1. Paylaşılan projesinde QSToDoService.cs açın ve, değişikliği geri **applicationURL** özelliği.
2. Yeniden oluşturun ve uygulamayı çalıştırın. Uygulama yerel değişikliklerinizi anında iletme ve çekme işlemlerini kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnRefreshItemsSelected` yöntemini yürütür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanılarak güncelleştirilmiş verileri görüntüleyin. Bildirim verileri Azure mobil uygulama arka uç veritabanı ve yerel depo arasında eşitlendi.
4. Uygulamada, yerel depoda tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `CompleteItemAsync`çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğesine. `SyncAsync`hem İtme hem de çekme çağırır.
   **İstemci için değişiklikler yaptı bir tabloda bir çekme yürütme zaman push istemci eşitleme bağlamı üzerinde her zaman ilk otomatik olarak yürütülmeden**. Örtük itme tüm tabloları ilişkileri yanı sıra Yerel Depodaki tutarlı kalmasını sağlar. Bu davranış hakkında daha fazla bilgi için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirme
Öğretici tamamlandığında yüklediğiniz Xamarin istemci projesi [bir Xamarin iOS uygulaması oluşturma] zaten yerel bir SQLite veritabanı kullanarak çevrimdışı eşitlemeyi destekleyen kodunu içerir. Aşağıda, öğretici kodu zaten eklenmiş bir kısa genel bakış verilmiştir. Özellik kavramsal bir genel bakış için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme].

* Tablo işlemleri gerçekleştirilmeden önce yerel deposu başlatılması gerekir. Yerel deposu veritabanı başlatılmış zaman `QSTodoListViewController.ViewDidLoad()` yürütür `QSTodoService.InitializeStoreAsync()`. Bu yöntemi kullanarak yeni bir yerel SQLite veritabanı oluşturur `MobileServiceSQLiteStore` Azure mobil uygulaması istemci SDK'sı tarafından sağlanan sınıfı.

    `DefineTable` Yöntemi, sağlanan türü alanları eşleşen yerel deposunda bir tablo oluşturur `ToDoItem` bu durumda. Türü Uzak veritabanında bulunan tüm sütunlar içermesi gerekmez. Yalnızca bir sütun alt kümesini depolamak mümkündür.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* `todoTable` Üyesi `QSTodoService` değil `IMobileServiceSyncTable` yerine tür `IMobileServiceTable`. IMobileServiceSyncTable tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri yerel depolama alanı veritabanına yönlendirir.

    Ne zaman bu değişiklikleri Azure mobil uygulama arka ucuna çağırarak gönderilen karar `IMobileServiceSyncContext.PushAsync()`. Eşitleme bağlamı izleme ve bir istemci uygulaması değiştiren tüm tablolarda değişiklikleri Ftp'den tablo ilişkileri korunmasına yardımcı olur `PushAsync` olarak adlandırılır.

    Sağlanan kod çağrıları `QSTodoService.SyncAsync()` todoıtem liste yenilendiğinde veya todoıtem eklenen veya tamamlanan her eşitleme. Uygulama eşitlemeler her yerel değişiklikten sonra. Bir çekme bağlamdan bekleyen yerel güncelleştirmeleri izlenen bir tabloya karşı yürütülürse, o çekme işlemi otomatik olarak bir bağlam göndermeli ilk tetikler.

    Sağlanan kodda, tüm kayıtları uzaktan `TodoItem` tablo sorgulanabilir ancak sorgu kimliği geçirerek kayıtlarını filtreleyip için sorgu mümkündür `PushAsync`. Daha fazla bilgi için bkz *Artımlı eşitleme* içinde [Azure Mobile Apps çevrimdışı veri eşitleme].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps çevrimdışı veri eşitleme]
* [Azure Mobile Apps .NET SDK'sı nasıl yapılır][8]

<!-- Images -->

<!-- URLs. -->
[bir Xamarin iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
[Azure Mobile Apps çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
