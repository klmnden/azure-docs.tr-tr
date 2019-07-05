---
title: Azure mobil uygulaması (Xamarin iOS) için çevrimdışı eşitlemeyi etkinleştirme
description: App Service mobil uygulaması, Xamarin iOS uygulamanızda çevrimdışı veri önbelleği ve eşitleme için kullanmayı öğrenin
documentationcenter: xamarin
author: elamalani
manager: cfowler
editor: ''
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: b87a1d86370e3abdb200b691d5216b1262512b3e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440054"
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Xamarin.iOS mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirin
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-ios-get-started-offline-data) bugün.
>

## <a name="overview"></a>Genel Bakış
Bu öğretici, Xamarin.iOS için Azure Mobile Apps, çevrimdışı eşitleme özelliği tanıtır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile verileri--değiştirme ile mobil uygulama--etkileşime olanak tanır. Değişiklikler, yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğreticide, Xamarin.iOS uygulaması projesini güncelleştirin [bir Xamarin iOS uygulaması oluşturma] Azure Mobile Apps çevrimdışı özellikleri desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özelliklerini desteklemek üzere istemci uygulamasını güncelleştirme
Azure mobil uygulama çevrimdışı özellikleri, bir çevrimdışı senaryoda olduğunuzda bir yerel veritabanıyla etkileşim kurmanıza imkan tanır. Uygulamanızda bu özellikleri kullanmak için başlatma bir [SyncContext] yerel bir depo için. Tablonuzu [IMobileServiceSyncTable] arabirimi aracılığıyla başvuru. SQLite, cihazdaki yerel deposu olarak kullanılır.

1. İçinde tamamlanan projedeki NuGet paket yöneticisini açın [bir Xamarin iOS uygulaması oluşturma] öğretici, daha sonra arama ve yükleme **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
2. QSTodoService.cs dosyasını açın ve açıklama durumundan çıkarın `#define OFFLINE_SYNC_ENABLED` tanımı.
3. Yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme özelliği etkin önce olduğu gibi uygulama aynı şekilde çalışır. Ancak, yerel veritabanı artık bir çevrimdışı senaryoda kullanılan verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucuna bir çevrimdışı duruma benzetimini yapmak için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyicinizde, uygulamayı çevrimdışı modda olduğunu bildirir. Bu durumda, yeni öğeleri yerel depoya eklendi ve anında iletme sonraki bağlı durumda çalıştırıldığında mobil uygulama arka ucuna eşitlenecektir.

1. Paylaşılan projede QSToDoService.cs düzenleyin. Değişiklik **applicationURL** geçersiz bir URL ile işaret edecek şekilde:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Çevrimdışı davranışı de, wifi ve cihazdaki hücresel ağlar devre dışı bırakma veya uçak modu kullanarak da gösterebilirsiniz.
2. Uygulamayı derleyin ve çalıştırın. Uygulama başlatıldığında yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme her tıkladığınızda [CancelledByNetworkError] durumu ile başarısız olduğunu fark **Kaydet**. Ancak mobil uygulama arka ucuna gönderilen kadar yeni todo öğeleri yerel depoda mevcut.  Bu özel durumlar bastır hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Bir PC'de Visual Studio yüklü değilse, açmak **Sunucu Gezgini**. Veritabanınızda gidin **Azure**-> **SQL veritabanları**. Veritabanınıza sağ tıklayıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosuna ve içeriği için göz atabilirsiniz. Arka uç veritabanı verilerini değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak mobil arka uç, sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka ucunuzu yeniden bağlanmak için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucu için uygulamayı yeniden bağlanın. Bu, uygulamanın mobil uygulama arka ucu ile çevrimiçi duruma çevrimdışı bir durumdan taşıma benzetimini yapar.   Ağ bağlantısı devre dışı bırakarak ağ arıza simülasyonu, kod değişikliği yapmadan gereklidir.
Ağ yeniden etkinleştirin.  Uygulama ilk kez çalıştırdığınızda `RefreshDataAsync` yöntemi çağrılır. Bu sırayla çağırır `SyncAsync` yerel deponuzun arka uç veritabanı ile eşitlenecek.

1. Paylaşılan projede QSToDoService.cs açın ve, değişikliği geri **applicationURL** özelliği.
2. Yeniden oluşturun ve uygulamayı çalıştırın. Uygulama, yerel değişikliklerinizi gönderme ve çekme işlemleri kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnRefreshItemsSelected` yöntemini yürütür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanarak güncelleştirilmiş verileri görüntüleyin. Veri Uyarısı, Azure mobil uygulaması arka uç veritabanı ve yerel depo arasında eşitlenmiş.
4. Uygulamada, yerel depoya tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `CompleteItemAsync` çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğe için. `SyncAsync` hem İtme hem de çekme çağırır.
   **Bir çekme istemci değişiklikler yaptı tabloya karşı yürüttüğünüz her bir anında iletme istemci eşitleme bağlamı üzerinde her zaman önce otomatik olarak yürütülmeden**. Örtük anında iletme ilişkileri yanı sıra Yerel Depodaki tüm tabloları tutarlı kalmasını sağlar. Bu davranışı hakkında daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirin
Öğretici tamamlandığında, indirdiğiniz Xamarin istemci projesi [bir Xamarin iOS uygulaması oluşturma] zaten yerel bir SQLite veritabanından kullanarak çevrimdışı eşitlemeyi destekleyen kodunu içerir. Eğitmen kodu zaten dahil, kısa bir genel bakış aşağıda verilmiştir. Özellik kavramsal bir genel bakış için bkz: [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

* Yerel depo, herhangi bir tablo işlem gerçekleştirilmeden önce başlatılmalıdır. Yerel depo veritabanı başlatılır, `QSTodoListViewController.ViewDidLoad()` yürütür `QSTodoService.InitializeStoreAsync()`. Bu yöntemi kullanarak yeni bir yerel SQLite veritabanı oluşturur `MobileServiceSQLiteStore` Azure mobil uygulama istemci SDK'sı tarafından sağlanan sınıfı.

    `DefineTable` Yöntemi sağlanan türü alanları ile eşleşen yerel deposunda bir tablo oluşturur `ToDoItem` bu durumda. Türü, uzak veritabanındaki tüm sütunları eklemek gerekmez. Yalnızca bir sütun alt kümesi depolamak mümkündür.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* `todoTable` Üyesi `QSTodoService` değil `IMobileServiceSyncTable` türü yerine `IMobileServiceTable`. Tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri yerel depolama alanı veritabanına IMobileServiceSyncTable yönlendirir.

    Çağırarak bu değişiklikleri Azure mobil uygulama arka ucuna zaman itilir karar `IMobileServiceSyncContext.PushAsync()`. Tablo ilişkileri izleme ve bir istemci uygulaması değiştiren tüm tablolardaki değişiklikleri gönderme tarafından korumak eşitleme bağlamı yardımcı `PushAsync` çağrılır.

    Sağlanan kod çağrıları `QSTodoService.SyncAsync()` todoıtem liste yenilenir veya bir todoıtem eklendiğinde veya tamamlanmış olduğunda eşitlenecek. Yerel her değişiklikten sonra uygulama eşitler. Bir çekme bekleyen yerel güncelleştirmeleri izlenen bir tabloya karşı bağlam tarafından yürütülürse, o çekme işlemi otomatik olarak bir bağlam anında iletme ilk tetikler.

    Sağlanan kod içinde tüm kayıtları uzaktan `TodoItem` tablosunu sorgulanan, ancak sorgu kimliği geçirerek kayıtlarını filtrelemek ve için sorgular mümkündür `PushAsync`. Daha fazla bilgi için konudaki *Artımlı eşitleme* içinde [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

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
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobile Apps .NET SDK nasıl yapılır][8]

<!-- Images -->

<!-- URLs. -->
[Bir Xamarin iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
