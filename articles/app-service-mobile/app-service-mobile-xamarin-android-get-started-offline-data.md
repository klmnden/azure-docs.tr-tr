---
title: "Azure mobil uygulaması (Xamarin Android) için çevrimdışı eşitlemeyi etkinleştirme"
description: "Önbellek ve eşitleme çevrimdışı verilere Xamarin Android uygulamanızda mobil uygulama hizmeti kullanmayı öğrenin"
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 471433c7ef2f6f128210ed145f685b42b44eea92
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Xamarin.Android mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici Xamarin.Android için Azure Mobile Apps, çevrimdışı eşitleme özelliği sunmaktadır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile veri--değiştirme bir mobil uygulamayla--etkileşime olanak sağlar. Değişiklikler yerel bir veritabanında depolanır.
Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğreticide, öğreticiyi istemci projeden güncelleştirme [bir Xamarin Android uygulaması oluşturma] Azure Mobile Apps çevrimdışı özelliklerini desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için İstemci uygulamayı güncelleştirme
Azure mobil uygulama çevrimdışı özellikler, çevrimdışı bir senaryoda olduğunda yerel bir veritabanı ile etkileşim kurmasına olanak sağlar. Bu özellikler, uygulamanızda kullanmak için başlatma bir [SyncContext] yerel depolama. Ardından tablonuz [IMobileServiceSyncTable] [IMobileServiceSyncTable] arabirimi aracılığıyla başvuru. SQLite yerel depolama aygıtında olarak kullanılır.

1. Visual Studio'da NuGet Paket Yöneticisi de tamamladığınız projesinde açın [bir Xamarin Android uygulaması oluşturma] Öğreticisi.  Arayın ve yükleyin **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
2. ToDoActivity.cs dosyasını açın ve açıklama durumundan çıkarmanız `#define OFFLINE_SYNC_ENABLED` tanımı.
3. Visual Studio'da basın **F5** anahtarı yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme etkinleştirilmeden önce yaptığınız gibi uygulamayı aynı şekilde çalışır. Ancak, yerel veritabanı, artık çevrimdışı bir senaryoda kullanılabilir verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme
Bu bölümde, bir çevrimdışı duruma benzetimini yapmak için mobil uygulama arka ucu için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyici uygulama çevrimdışı modda olduğunu söyler. Bu durumda, yeni öğeler yerel depoya eklendi ve bağlı bir durumda bir itme çalıştırıldığında mobil uygulama arka ucuna eşitlenir.

1. ToDoActivity.cs paylaşılan projesinde düzenleyin. Değişiklik **applicationURL** geçersiz bir URL'ye yönlendirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, wifi ve aygıt cep telefonu ağlarda devre dışı bırakma veya uçak modu kullanılarak çevrimdışı davranışı sergiler.
2. Tuşuna **F5** oluşturun ve uygulamayı çalıştırın. Uygulama başlatıldığında üzerinde yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme her tıkladığınızda [CancelledByNetworkError] durumu ile başarısız fark **kaydetmek**. Ancak, mobil uygulama arka ucuna gönderilemez kadar yeni Yapılacaklar öğelerini yerel depoda mevcut.  Bu özel durumlar bastırmak hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamasını kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklatıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosunda ve içeriği göz atabilirsiniz. Arka uç veritabanındaki verileri değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak, mobil arka uç sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka yeniden bağlanmak için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucu uygulamaya yeniden bağlanın. Uygulama ilk kez çalıştırdığınızda `OnCreate` olay işleyicisi çağrılarını `OnRefreshItemsSelected`. Bu yöntemi çağırır `SyncAsync` yerel deponuza arka uç veritabanı ile eşitlemek için.

1. Paylaşılan projesinde ToDoActivity.cs açın ve, değişikliği geri **applicationURL** özelliği.
2. Tuşuna **F5** anahtarı yeniden oluşturmak ve uygulamayı çalıştırmak için. Uygulama yerel değişikliklerinizi anında iletme ve çekme işlemlerini kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnRefreshItemsSelected` yöntemini yürütür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanılarak güncelleştirilmiş verileri görüntüleyin. Bildirim verileri Azure mobil uygulama arka uç veritabanı ve yerel depo arasında eşitlendi.
4. Uygulamada, yerel depoda tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `CheckItem`çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğesine. `SyncAsync`hem İtme hem de çekme çağırır. **İstemci için değişiklikler yaptı bir tabloda bir çekme execute olduğunda, bir itme her zaman otomatik olarak yürütülmeden**. Bu, tüm tabloları ilişkileri yanı sıra Yerel Depodaki tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir. Bu davranış hakkında daha fazla bilgi için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirme
Öğretici tamamlandığında yüklediğiniz Xamarin istemci projesi [bir Xamarin Android uygulaması oluşturma] zaten yerel bir SQLite veritabanı kullanarak çevrimdışı eşitlemeyi destekleyen kodunu içerir. Aşağıda, öğretici kodu zaten eklenmiş bir kısa genel bakış verilmiştir. Özellik kavramsal bir genel bakış için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme].

* Tablo işlemleri gerçekleştirilmeden önce yerel deposu başlatılması gerekir. Yerel deposu veritabanı başlatılmış zaman `ToDoActivity.OnCreate()` yürütür `ToDoActivity.InitLocalStoreAsync()`. Bu yöntem, yerel SQLite kullanarak bir veritabanı oluşturur `MobileServiceSQLiteStore` Azure Mobile Apps istemci SDK'sı tarafından sağlanan sınıfı.

    `DefineTable` Yöntemi, sağlanan türü alanları eşleşen yerel deposunda bir tablo oluşturur `ToDoItem` bu durumda. Türü Uzak veritabanında bulunan tüm sütunlar içermesi gerekmez. Yalnızca bir sütun alt kümesini depolamak mümkündür.

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
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* `toDoTable` Üyesi `ToDoActivity` değil `IMobileServiceSyncTable` yerine tür `IMobileServiceTable`. IMobileServiceSyncTable tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri yerel depolama alanı veritabanına yönlendirir.

    Ne zaman değişiklikleri Azure mobil uygulama arka ucuna çağırarak gönderilen karar `IMobileServiceSyncContext.PushAsync()`. Eşitleme bağlamı izleme ve bir istemci uygulaması değiştiren tüm tablolarda değişiklikleri Ftp'den tablo ilişkileri korunmasına yardımcı olur `PushAsync` olarak adlandırılır.

    Sağlanan kod çağrıları `ToDoActivity.SyncAsync()` todoıtem liste yenilendiğinde veya todoıtem eklenen veya tamamlanan her eşitleme. Kod eşitlemeler her yerel değişiklikten sonra.

    Sağlanan kodda, tüm kayıtları uzaktan `TodoItem` tablo sorgulanabilir ancak sorgu kimliği geçirerek kayıtlarını filtreleyip için sorgu mümkündür `PushAsync`. Daha fazla bilgi için bkz *Artımlı eşitleme* içinde [Azure Mobile Apps çevrimdışı veri eşitleme].

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
* [Azure Mobile Apps çevrimdışı veri eşitleme]
* [Azure Mobile Apps .NET SDK'sı nasıl yapılır][8]

<!-- URLs. -->
[bir Xamarin Android uygulaması oluşturma]: ../app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps çevrimdışı veri eşitleme]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
