---
title: Mobile Apps ile Evrensel Windows Platformu (UWP) uygulamanız için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: Çevrimdışı veri önbelleği ve eşitleme için Azure mobil uygulaması, Evrensel Windows Platformu (UWP) uygulamanızda kullanmayı öğrenin.
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
ms.openlocfilehash: 69ee9e7101a2b7337e1e42ff5ae09954fbfd50b2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128058"
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Windows uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide bir Azure mobil uygulaması arka ucunu kullanarak bir evrensel Windows Platformu (UWP) uygulamasına çevrimdışı destek eklemeyi gösterilmektedir. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile verileri - değiştirme ile mobil uygulama--etkileşime olanak tanır. Değişiklikler, yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak arka uç ile eşitlenir.

Bu öğreticide, UWP uygulaması projesini öğreticiden güncelleştirme [bir Windows uygulaması oluşturma] Azure Mobile Apps çevrimdışı özellikleri desteklemek için. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="requirements"></a>Gereksinimler  
Bu öğretici aşağıdaki önkoşullar gereklidir:

* Windows 8.1 veya üzeri çalıştıran Visual Studio 2013.
* Tamamlanmasından [bir Windows uygulaması oluşturma][bir windows uygulaması oluşturma].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite Evrensel Windows platformu geliştirme](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform) 

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özelliklerini desteklemek üzere istemci uygulamasını güncelleştirme
Azure mobil uygulama çevrimdışı özellikleri, bir çevrimdışı senaryoda olduğunuzda bir yerel veritabanıyla etkileşim kurmanıza imkan tanır. Uygulamanızda bu özellikleri kullanmak için başlatma bir [SyncContext] [ synccontext] yerel bir depo için. Ardından tablonuzun aracılığıyla başvuru [IMobileServiceSyncTable][IMobileServiceSyncTable] arabirimi. SQLite, cihazdaki yerel deposu olarak kullanılır.

1. Yükleme [Evrensel Windows platformu için SQLite çalışma zamanı](https://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. Visual Studio'da açın, tamamlanan UWP uygulaması projesi için NuGet Paket Yöneticisi [bir Windows uygulaması oluşturma] öğretici.
    Arama ve yükleme **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketi.
3. Çözüm Gezgini'nde sağ **başvuruları** > **Başvuru Ekle...** > **Evrensel Windows** > **uzantıları**, her ikisini de etkinleştirmek **Evrensel Windows platformu için SQLite** ve **Evrensel Windows için Visual C++ 2015 çalışma zamanı Platform uygulamaları**.

    ![SQLite UWP Başvurusu Ekle][1]
4. MainPage.xaml.cs dosyasını açın ve açıklama durumundan çıkarın `#define OFFLINE_SYNC_ENABLED` tanımı.
5. Visual Studio'da **F5** anahtarı yeniden oluşturun ve istemci uygulaması çalıştırın. Çevrimdışı eşitleme özelliği etkin önce olduğu gibi uygulama aynı şekilde çalışır. Ancak, yerel veritabanı artık bir çevrimdışı senaryoda kullanılan verilerle doldurulur.

## <a name="update-sync"></a>Arka ucunuzdan bağlantısını kesmek için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucuna bir çevrimdışı duruma benzetimini yapmak için bağlantıyı kesin. Veri öğeleri eklediğinizde, özel durum işleyicinizde, uygulamayı çevrimdışı modda olduğunu bildirir. Bu durumda, yeni öğeleri yerel depoya eklendi ve anında iletme sonraki bağlı durumda çalıştırıldığında mobil uygulama arka ucuna eşitlenecektir.

1. App.xaml.cs paylaşılan projede düzenleyin. Başlatma yorum **MobileServiceClient** ve geçersiz mobil uygulama URL'si kullanan aşağıdaki satırı ekleyin:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Ayrıca, wifi ve cihazdaki hücresel ağlar devre dışı bırakarak çevrimdışı davranışı sergiler veya uçak modu kullanın.
2. Tuşuna **F5** oluşturun ve uygulamayı çalıştırın. Uygulama başlatıldığında yenileme başarısız oldu, eşitleme dikkat edin.
3. Yeni öğeler girin ve anında iletme ile başarısız olduğunu fark bir [CancelledByNetworkError] durumu her zaman'ı **Kaydet**. Ancak mobil uygulama arka ucuna gönderilen kadar yeni todo öğeleri yerel depoda mevcut.  Bu özel durumlar bastır hala mobil uygulama arka ucuna bağlı olarak, bir üretim uygulamasında istemci uygulaması davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklayıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosuna ve içeriği için göz atabilirsiniz. Arka uç veritabanı verilerini değişmediğini doğrulayın.
6. (İsteğe bağlı) Bir REST Fiddler veya Postman gibi biçiminde bir GET sorgu kullanarak mobil arka uç, sorgu aracını `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Mobil uygulama arka ucunuzu yeniden bağlanmak için uygulamayı güncelleştirme
Bu bölümde, mobil uygulama arka ucu için uygulamayı yeniden bağlanın. Bu değişiklikler, uygulama bir ağ yeniden bağlanıldığında benzetimini yapar.

Uygulama ilk kez çalıştırdığınızda `OnNavigatedTo` olay işleyicisi çağrılarını `InitLocalStoreAsync`. Sırayla bu yöntemi çağırır `SyncAsync` yerel deponuzun arka uç veritabanı ile eşitlenecek. Uygulama başlangıcında eşitleme girişiminde bulunur.

1. App.xaml.cs paylaşılan projede açın ve önceki, başlatma açıklamadan çıkarın `MobileServiceClient` mobil uygulama URL'si doğru kullanılacak.
2. Tuşuna **F5** anahtarını yeniden oluşturun ve uygulamayı çalıştırın. Uygulama, yerel değişikliklerinizi gönderme ve çekme işlemleri kullanarak Azure mobil uygulama arka ucu ile eşitlenen zaman `OnNavigatedTo` olay işleyicisi yürütülür.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanarak güncelleştirilmiş verileri görüntüleyin. Veri Uyarısı, Azure mobil uygulaması arka uç veritabanı ve yerel depo arasında eşitlenmiş.
4. Uygulamada, yerel depoya tamamlanması birkaç öğelerin yanındaki onay kutusuna tıklayın.

   `UpdateCheckedTodoItem` çağrıları `SyncAsync` mobil uygulama arka ucu ile Eşitleme tamamlandı her öğe için. `SyncAsync` hem İtme hem de çekme çağırır. Ancak, **istemci değişiklikler yaptı bir tabloda bir çekme execute olduğunda bir anında iletme her zaman otomatik olarak yürütülmeden**. Bu davranış, tüm tabloları ilişkileri yanı sıra Yerel Depodaki tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir.  Bu davranışı hakkında daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="api-summary"></a>API özeti
Mobil Hizmetleri çevrimdışı özellikleri desteklemek için kullandığımız [IMobileServiceSyncTable] arabirim ve başlatılmış [MobileServiceClient.SyncContext] [ synccontext] ile bir yerel bir SQLite veritabanıdır. Yerel depo işlemleri meydana gelirken uygulama hala bağlı çevrimdışı olduğunda normal CRUD işlemleri mobil uygulamalar için çalışır. Aşağıdaki yöntemlerden yerel depo sunucusu ile eşitlemek için kullanılır:

* **[PushAsync]**  bu yöntem bir üyesi olduğundan [IMobileServicesSyncContext], tüm tablolarda değişiklikler, arka uca itilir. Yalnızca yerel değişiklikleri kayıtları sunucuya gönderilir.
* **[PullAsync]**  bir çekme başlattığınız bir [IMobileServiceSyncTable]. Tablodaki izlenen değişiklik olduğunda ilişkileri yanı sıra Yerel Depodaki tüm tabloları tutarlı kalmasını sağlamak için örtük bir anında iletme çalıştırın. *PushOtherTables* olup diğer bağlamında tabloları içinde örtük bir anında iletme itilir parametresi denetler. *Sorgu* parametre alan bir [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] veya döndürülen verileri filtrelemek için OData sorgu dizesi. *QueryId* parametresi Artımlı eşitleme tanımlamak için kullanılır. Daha fazla bilgi için [Azure Mobile apps'te çevrimdışı veri eşitleme](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  uygulamanıza düzenli aralıklarla yerel depodan eski verileri temizlemek için bu yöntemi çağırmanız gerekir. Kullanım *zorla* henüz eşitlenmemiş değişiklikleri temizlemek gerektiğinde parametresi.

Bu kavramlar hakkında daha fazla bilgi için bkz. [Azure Mobile apps'te çevrimdışı veri eşitleme](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Daha fazla bilgi
Aşağıdaki konular, Mobile Apps, çevrimdışı eşitleme özelliği ek arka plan bilgileri sağlar:

* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobile Apps .NET SDK nasıl yapılır][8]

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
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[bir windows uygulaması oluşturma]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: https://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: https://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: https://go.microsoft.com/fwlink/?LinkID=716921
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
