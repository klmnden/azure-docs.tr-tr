---
title: Azure mobil uygulaması (Xamarin.Forms) için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: Xamarin.Forms uygulamanızda çevrimdışı veri önbelleği ve eşitleme için App Service Mobile Apps'ı kullanmayı öğrenin
documentationcenter: xamarin
author: conceptdev
manager: yochayk
editor: ''
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: crdun
ms.openlocfilehash: 506c59ca24aeafbac59b1508bb78142051302765
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127888"
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Xamarin.Forms mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici için Xamarin.Forms çevrimdışı eşitleme özelliği Azure Mobile Apps tanıtır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile verileri--değiştirme ile mobil uygulama--etkileşime olanak tanır. Değişiklikler, yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğretici, Mobile Apps için [bir Xamarin iOS uygulaması oluşturma] öğreticiyi tamamladıktan sonra oluşturduğunuz Xamarin.Forms hızlı çözüm üzerinde temel alır. Xamarin.Forms için hızlı başlangıç çözümü etkinleştirilmesi gerekiyor çevrimdışı eşitleme desteği için kod içerir. Bu öğreticide, Azure Mobile Apps'ın çevrimdışı özellikleri etkinleştirmek için hızlı başlangıç çözüm güncelleştirin. Biz de çevrimdışı özgü kod uygulamasında vurgulayın. İndirilen hızlı başlangıç çözümü kullanmıyorsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma][1].

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile apps'te çevrimdışı veri eşitleme][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Hızlı Başlangıç çözümdeki çevrimdışı eşitleme işlevselliğini etkinleştirin
Çevrimdışı eşitleme kod kullanarak projeye dahil C# önişlemci yönergeleri. Zaman **çevrimdışı\_eşitleme\_etkin** sembol tanımlanır, bu kod yolları yapıya dahil edilir. Windows uygulamaları için SQLite platformu da yüklemeniz gerekir.

1. Visual Studio'da çözüme sağ tıklayın > **çözüm için NuGet paketlerini Yönet...** , ardından aramak ve yüklemek **Microsoft.Azure.Mobile.Client.SQLiteStore** Çözümdeki tüm projeleri için NuGet paketi.
2. Çözüm Gezgini'nde, ile proje Todoıtemmanager.cs dosyasını açın. **taşınabilir** , taşınabilir sınıf kitaplığı proje adı, ardından aşağıdaki önişlemci yönergesi açıklamasını kaldırın:

        #define OFFLINE_SYNC_ENABLED
3. (İsteğe bağlı) Windows cihazları desteklemek için aşağıdaki SQLite çalışma zamanı paketlerini yükleyin:

   * **Windows 8.1 çalışma zamanı:** Yükleme [Windows 8.1 için SQLite][3].
   * **Windows Phone 8.1:** Yükleme [Windows Phone 8.1 için SQLite][4].
   * **Evrensel Windows platformu** yükleme [Evrensel Windows Evrensel için SQLite][5].

     Bu hızlı başlangıçta, bir evrensel Windows projesi içermiyor olsa da, Evrensel Windows platformu, Xamarin Forms ile desteklenir.
4. (İsteğe bağlı) Her Windows uygulama projesine sağ tıklayın **başvuruları** > **Başvuru Ekle...** , genişletme **Windows** klasör > **uzantıları**.
    Uygun etkinleştirme **için SQLite Windows** SDK'sı ile birlikte **için Visual C++ 2013 çalışma zamanı Windows** SDK.
    SQLite SDK adları, her Windows platformuyla biraz farklılık gösterir.

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirin
Ne öğretici kod zaten eklenmiştir, kısa bir genel bakış şöyledir `#if OFFLINE_SYNC_ENABLED` yönergeleri. Taşınabilir sınıf kitaplığı proje Todoıtemmanager.cs proje dosyasında çevrimdışı eşitleme işlevdir. Özellik kavramsal bir genel bakış için bkz: [Azure Mobile apps'te çevrimdışı veri eşitleme][2].

* Yerel depo, herhangi bir tablo işlem gerçekleştirilmeden önce başlatılmalıdır. Yerel depo veritabanı içinde başlatılır **TodoItemManager** sınıfı Oluşturucu aşağıdaki kodu kullanarak:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Bu kod kullanarak yeni bir yerel SQLite veritabanı oluşturur ve **MobileServiceSQLiteStore** sınıfı.

    **DefineTable** yöntemi sağlanan türü alanları ile eşleşen yerel deposunda bir tablo oluşturur.  Türü, uzak veritabanındaki tüm sütunları eklemek gerekmez. Bir sütun alt kümesi depolamak mümkündür.
* **TodoTable** alanındaki **TodoItemManager** olduğu bir **IMobileServiceSyncTable** türü yerine **IMobileServiceTable**. Bu sınıfı kullanan tüm yerel veritabanı oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri. Çağırarak bu değişiklikleri mobil uygulama arka ucuna zaman itilir karar **PushAsync** üzerinde **IMobileServiceSyncContext**. Tablo ilişkileri izleme ve bir istemci uygulaması değiştiren tüm tablolardaki değişiklikleri gönderme tarafından korumak eşitleme bağlamı yardımcı **PushAsync** çağrılır.

    Aşağıdaki **SyncAsync** yöntemi çağrıldığında mobil uygulama arka ucu ile eşitlemek için:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Bu örnek, basit hata varsayılan eşitleme işleyici ile işleme kullanır. Gerçek bir uygulamada özel bir kullanarak ağ koşulları ve sunucu çakışmaları gibi çeşitli hatalar'ı işleme **IMobileServiceSyncHandler** uygulaması.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme konuları
Örnekte, **SyncAsync** yöntemi başlatma ve ne zaman eşitleme istenen yalnızca çağrılır.  Bir Android veya iOS uygulama eşitleme başlatmak için öğeleri listesinde çekme; Windows için kullanmak **eşitleme** düğmesi. Gerçek bir uygulamada, ağ durumu değiştiğinde eşitleme tetikleyici de yapabilirsiniz.

Beklemede olan bir tabloya karşı bir çekme yürütüldüğünde yerel güncelleştirmeleri önceki bir bağlam anında iletme işlemi Tetikleyicileri otomatik olarak çekmesini bağlam tarafından izlenir. Yenilerken, ekleme ve bu örnekte, öğeleri Tamamlanıyor açık atlayabilirsiniz **PushAsync** çağırın.

Sağlanan kod içinde uzak Todoıtem tablosundaki tüm kayıtlar seçmeleri istenir, ancak sorgu kimliği geçirerek kayıtlarını filtrelemek ve için sorgular mümkündür **PushAsync**. Daha fazla bilgi için konudaki *Artımlı eşitleme* içinde [Azure Mobile apps'te çevrimdışı veri eşitleme][2].

## <a name="run-the-client-app"></a>İstemci uygulaması çalıştırın
İstemci uygulaması, artık etkin çevrimdışı eşitleme ile her platformda yerel depo veritabanını doldurmak için en az bir kez çalıştırın. Daha sonra çevrimdışı bir senaryonun benzetimini yapmak ve uygulamayı çevrimdışı durumdayken yerel deposundaki verileri değiştirebilirsiniz.

## <a name="update-the-sync-behavior-of-the-client-app"></a>İstemci uygulaması eşitleme davranışını güncelleştir
Bu bölümde, arka ucunuz için bir geçersiz uygulama URL'si kullanılarak bir çevrimdışı senaryoda benzetimini yapmak için istemci projesini değiştirin. Alternatif olarak, "Uçak modu." Cihazınızı taşıyarak ağ bağlantılarını devre dışı dışı bırakabilirsiniz  Veri öğeleri ekleyin veya değiştirdiğinizde, bu değişiklikleri yerel deposunda tutulan ancak arka uç veri deposuna bağlantı yeniden kurulur kadar eşitlenmedi.

1. Çözüm Gezgini'nde Constants.cs proje dosyasından açın **taşınabilir** değiştirin ve proje `ApplicationURL` geçersiz bir URL ile işaret edecek şekilde:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Todoıtemmanager.cs dosya açma **taşınabilir** proje ardından ekleyin bir **catch** tabanı için **özel durum** sınıfının **try... catch**engelleyin **SyncAsync**. Bu **catch** blok özel durum iletisi şu şekilde konsola yazar:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Oluşturun ve istemci uygulaması çalıştırın.  Bazı yeni öğeleri ekleyin. Bir özel durum için arka uç ile eşitlemek için her girişim konsolunda kaydedilir dikkat edin. Mobil arka ucuna gönderilen kadar bu yeni öğeleri yalnızca yerel depoda mevcut. Bağlı olduğu gibi arka uç, tüm oluşturma, okuma destekleyen, güncelleştirme, silme (CRUD) işlemleri için istemci uygulaması davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Arka uç veritabanındaki verilerin değiştirilmesi etkiye görmek için Azure SQL veritabanı tablosunu görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklayıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosuna ve içeriği için göz atabilirsiniz.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Mobil arka ucunuzdaki yeniden bağlanmak için istemci uygulamasını güncelleştirme
Bu bölümde, çevrimiçi duruma geri dönmeyi uygulama taklit eden mobil arka uygulamaya yeniden bağlanın. Yenileme hareket gerçekleştirdiğinizde, veriler mobil arka ucunuza eşitlenir.

1. Constants.cs yeniden açın. Düzeltme `applicationURL` doğru URL'ye yönlendirin.
2. Yeniden oluşturun ve istemci uygulaması çalıştırın. Uygulama başlatıldıktan sonra mobil uygulama arka ucu ile eşitlemek çalışır. Özel durum yok hata ayıklama konsolunu açtığınızdan emin olun.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanarak güncelleştirilmiş verileri görüntülemek veya [Postman][6]. Yerel depo ve arka uç veritabanı arasında veri eşitlenmiş dikkat edin.

    Uyarı verileri, veritabanı ve yerel depo arasında eşitlenmesini ve uygulamanızı değilken, eklenen öğeleri içeriyor.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile apps'te çevrimdışı veri eşitleme][2]
* [Azure Mobile Apps .NET SDK nasıl yapılır][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: https://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: https://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: https://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: https://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
