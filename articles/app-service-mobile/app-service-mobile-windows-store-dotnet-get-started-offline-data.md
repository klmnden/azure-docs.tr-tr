---
title: Evrensel Windows Platformu (UWP) uygulamanızı Mobile Apps için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: Çevrimdışı veri önbelleği ve eşitleme için Azure mobil uygulaması Evrensel Windows Platformu (UWP) uygulamanızı kullanmayı öğrenin.
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: a16de4cef82c29f9b6becfae1901662ee1936934
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27594488"
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Windows uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici bir Azure mobil uygulama arka ucu kullanarak bir evrensel Windows Platformu (UWP) uygulaması çevrimdışı destek eklemeyi gösterilmiştir. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile veri - değiştirme bir mobil uygulamayla--etkileşime olanak sağlar. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak arka uç ile eşitlenir.

Bu öğreticide, UWP uygulaması projesi öğreticiden güncelleştirme [bir Windows uygulaması oluşturma] Azure Mobile Apps çevrimdışı özelliklerini desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="requirements"></a>Gereksinimler
Bu öğretici aşağıdaki önkoşulları gerektirir:

* Windows 8.1 veya sonraki sürümlerde çalışan Visual Studio 2013.
* Tamamlanmasından [bir Windows uygulaması oluşturma][bir windows uygulaması oluşturma].
* [Azure Mobile Services SQLite deposu][sqlite store nuget]
* [SQLite Evrensel Windows platformu geliştirme](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için İstemci uygulamayı güncelleştirme
Azure mobil uygulama çevrimdışı özellikler, çevrimdışı bir senaryoda olduğunda yerel bir veritabanı ile etkileşim kurmasına olanak sağlar. Bu özellikler, uygulamanızda kullanmak için başlatma bir [SyncContext] [ synccontext] yerel depolama. Tablonuz aracılığıyla başvuru [IMobileServiceSyncTable][IMobileServiceSyncTable] arabirimi. SQLite yerel depolama aygıtında olarak kullanılır.

1. Yükleme [SQLite çalışma zamanı Evrensel Windows platformu için](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. Visual Studio'da'tamamlandığı UWP uygulaması projesi için NuGet Paket Yöneticisi açın [bir Windows uygulaması oluşturma] Öğreticisi.
    Arayın ve yükleyin **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
3. Çözüm Gezgini'nde sağ **başvuruları** > **Başvuru Ekle...** >**Evrensel Windows** > **uzantıları**, her ikisi de etkinleştirmek **Evrensel Windows platformu için SQLite** ve **Evrensel Windows platformu uygulamalar için Visual C++ 2015 çalışma zamanı**.

    ![SQLite UWP başvuru ekleme][1]
4. MainPage.xaml.cs dosyasını açın ve açıklama durumundan çıkarmanız `#define OFFLINE_SYNC_ENABLED` tanımı.
5. Visual Studio'da basın **F5** anahtarı yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme etkinleştirilmeden önce yaptığınız gibi uygulamayı aynı şekilde çalışır. Ancak, yerel veritabanı, artık çevrimdışı bir senaryoda kullanılabilir verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme
Bu bölümde, bir çevrimdışı duruma benzetimini yapmak için mobil uygulama arka ucu için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyici uygulama çevrimdışı modda olduğunu söyler. Bu durumda, yeni öğeler yerel depoya eklendi ve anında iletme bağlı bir durumda İleri çalıştırıldığında mobil uygulama arka ucuna senkronize edilir.

1. App.xaml.cs paylaşılan projesinde düzenleyin. Açıklama başlatılması çıkışı **MobileServiceClient** ve geçersiz mobil uygulama URL'sini kullanır aşağıdaki satırı ekleyin:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Ayrıca, wifi ve aygıt cep telefonu ağlarda devre dışı bırakarak çevrimdışı davranışı sergiler veya uçak modu kullanabilirsiniz.
2. Tuşuna **F5** oluşturun ve uygulamayı çalıştırın. Uygulama başlatıldığında üzerinde yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme ile başarısız olduğunu fark bir [CancelledByNetworkError] durum her tıkladığınızda **kaydetmek**. Ancak, mobil uygulama arka ucuna gönderilemez kadar yeni Yapılacaklar öğelerini yerel depoda mevcut.  Bu özel durumlar bastırmak hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamasını kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklatıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosunda ve içeriği göz atabilirsiniz. Arka uç veritabanındaki verileri değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak, mobil arka uç sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka yeniden bağlanmak için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucu uygulamaya yeniden bağlanın. Bu değişiklikler uygulamayı bir ağ yeniden bağlanıldığında benzetimi.

Uygulama ilk kez çalıştırdığınızda `OnNavigatedTo` olay işleyicisi çağrılarını `InitLocalStoreAsync`. Bu yöntem sırayla çağırır `SyncAsync` yerel deponuza arka uç veritabanı ile eşitlemek için. Uygulama başlangıcında eşitleme girişiminde bulunur.

1. App.xaml.cs paylaşılan projeyi açın ve önceki başlangıcına açıklamadan çıkarın `MobileServiceClient` doğru mobil uygulama URL'sini kullanmak üzere.
2. Tuşuna **F5** anahtarı yeniden oluşturmak ve uygulamayı çalıştırmak için. Uygulama yerel değişikliklerinizi anında iletme ve çekme işlemlerini kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnNavigatedTo` olay işleyicisi yürütür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanılarak güncelleştirilmiş verileri görüntüleyin. Bildirim verileri Azure mobil uygulama arka uç veritabanı ve yerel depo arasında eşitlendi.
4. Uygulamada, yerel depoda tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `UpdateCheckedTodoItem`çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğesine. `SyncAsync`hem İtme hem de çekme çağırır. Ancak, **istemci için değişiklikler yaptı bir tabloda bir çekme execute olduğunda, bir itme her zaman otomatik olarak yürütülmeden**. Bu davranış yerel depodaki ilişkileri yanı sıra tüm tabloları tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir.  Bu davranış hakkında daha fazla bilgi için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="api-summary"></a>API özeti
Mobil hizmetler çevrimdışı özelliklerini destekleyecek şekilde kullandık [IMobileServiceSyncTable] arabirim ve başlatıldı [MobileServiceClient.SyncContext] [ synccontext] yerel bir SQLite veritabanı ile. Çevrimdışı iken, yerel depolama karşı işlemleri meydana gelirken uygulama hala bağlı olarak normal CRUD işlemleri mobil uygulamalar için çalışır. Aşağıdaki yöntemlerden yerel deposu sunucusu ile eşitlemek için kullanılır:

* **[PushAsync]**  bu yöntemi bir üyesi olduğundan [IMobileServicesSyncContext], tüm tablolar arasında değişiklikleri arka ucuna gönderilir. Yalnızca yerel değişiklikler kayıtları sunucuya gönderilir.
* **[PullAsync]**  çekme başlattığınız bir [IMobileServiceSyncTable]. Tabloda izlenen değişiklik olduğunda Yerel Depodaki ilişkileri yanı sıra tüm tabloları tutarlı kalmasını emin olmak için örtük bir anında iletme çalıştırılır. *PushOtherTables* olup olmadığı diğer bağlamda tabloları örtük bir anında iletme içinde gönderilen parametresi denetler. *Sorgu* parametresi alan bir [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] veya döndürülen verileri filtrelemek için OData sorgu dizesi. *QueryId* parametresi Artımlı eşitleme tanımlamak için kullanılır. Daha fazla bilgi için bkz: [çevrimdışı veri eşitlemeye Azure Mobile Apps içinde](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  uygulamanızı düzenli aralıklarla yerel deposu eski verileri temizlemek için bu yöntemi çağırmanız gerekir. Kullanım *zorla* henüz eşitlenmemiş değişiklikleri temizlemek gerektiğinde parametresi.

Bu kavramlar hakkında daha fazla bilgi için bkz: [çevrimdışı veri eşitlemeye Azure Mobile Apps içinde](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Daha fazla bilgi
Aşağıdaki konular, Mobile Apps çevrimdışı eşitleme özelliği hakkında ek arka plan bilgileri sağlar:

* [Azure Mobile Apps çevrimdışı veri eşitleme]
* [Azure Mobile Apps .NET SDK'sı nasıl yapılır][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Azure Mobile Apps çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md
[bir windows uygulaması oluşturma]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
