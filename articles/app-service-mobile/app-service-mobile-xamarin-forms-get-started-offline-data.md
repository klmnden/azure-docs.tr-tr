---
title: Azure mobil uygulaması (Xamarin.Forms) için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: Mobil uygulama hizmeti için önbellek ve eşitleme çevrimdışı veri Xamarin.Forms uygulamanızda nasıl kullanılacağını öğrenin
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
ms.openlocfilehash: f88e6a4037bcca54982359742cdc6021f020882d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27594726"
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Xamarin.Forms mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici Xamarin.Forms için Azure Mobile Apps, çevrimdışı eşitleme özelliği sunmaktadır. Çevrimdışı eşitleme son kullanıcıların görüntüleme, ekleme veya ağ bağlantısı olduğunda bile veri--değiştirme bir mobil uygulamayla--etkileşime olanak sağlar. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğretici Xamarin.Forms hızlı başlangıç çözümü Mobile Apps için [bir Xamarin iOS uygulaması oluştur] öğreticiyi tamamladığınızda oluşturduğunuz temel alır. Xamarin.Forms için hızlı başlangıç çözümü etkinleştirilmesi gerekiyor çevrimdışı eşitleme desteklemek için kodunu içerir. Bu öğreticide, Azure Mobile Apps çevrimdışı özelliklerini etkinleştirme için hızlı başlangıç çözüm güncelleştirin. Biz de uygulama çevrimdışı özgü kodu vurgulayın. İndirilen hızlı başlangıç çözümü kullanmıyorsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş][1].

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Hızlı Başlangıç çözümde çevrimdışı eşitleme işlevselliğini etkinleştirmek
Çevrimdışı eşitleme kod projesinde, C# önişlemci yönergeleri kullanarak eklenmiştir. Zaman **çevrimdışı\_eşitleme\_etkin** simgesiyle tanımlanır, bu kod yolları yapı dahil edilir. Windows uygulamaları için SQLite platform de yüklemeniz gerekir.

1. Visual Studio'da çözüme sağ tıklayın > **çözüm için NuGet paketlerini Yönet...** , ardından arayın ve yükleyin **Microsoft.Azure.Mobile.Client.SQLiteStore** Çözümdeki tüm projeleri için NuGet paketi.
2. Çözüm Gezgini'nde, ile projenizden Todoıtemmanager.cs dosyasını açın. **taşınabilir** taşınabilir sınıf kitaplığı proje olan adlarında aşağıdaki önişlemci yönergesi sonra açıklamadan çıkarın:

        #define OFFLINE_SYNC_ENABLED
3. (İsteğe bağlı) Windows cihazları desteklemek için aşağıdaki SQLite çalışma zamanı paketlerden birini yükleyin:

   * **Windows 8.1 çalışma zamanı:** yükleme [Windows 8.1 için SQLite][3].
   * **Windows Phone 8.1:** yükleme [Windows Phone 8.1 için SQLite][4].
   * **Evrensel Windows platformu** yükleme [Evrensel Windows Evrensel SQLite][5].

     Hızlı Başlangıç Evrensel Windows projesi içermiyor rağmen Evrensel Windows platformu Xamarin Forms ile desteklenir.
4. (İsteğe bağlı) Her Windows uygulama projesine sağ **başvuruları** > **Başvuru Ekle...** , genişletin **Windows** klasörü > **uzantıları**.
    Uygun etkinleştirme **SQLite for Windows** SDK ile birlikte **için Visual C++ 2013 çalışma zamanı Windows** SDK.
    SQLite SDK adları, her Windows platformuyla biraz farklılık gösterir.

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodu gözden geçirme
Zaten içinde Eğitmen kodu içereceği kısa genel bakış işte `#if OFFLINE_SYNC_ENABLED` yönergeleri. Taşınabilir sınıf kitaplığı proje Todoıtemmanager.cs proje dosyasında çevrimdışı eşitleme işlevdir. Özellik kavramsal bir genel bakış için bkz: [çevrimdışı veri eşitlemeye Azure Mobile Apps içinde][2].

* Tablo işlemleri gerçekleştirilmeden önce yerel deposu başlatılması gerekir. Yerel deposu veritabanının içindeki başlatıldığından **TodoItemManager** aşağıdaki kodu kullanarak sınıfı oluşturucusu:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Bu kodu kullanarak yeni bir yerel SQLite veritabanı oluşturur **MobileServiceSQLiteStore** sınıfı.

    **DefineTable** yöntemi, sağlanan türü alanları eşleşen yerel deposunda bir tablo oluşturur.  Türü Uzak veritabanında bulunan tüm sütunlar içermesi gerekmez. Bir sütun alt kümesini depolamak mümkündür.
* **TodoTable** alanındaki **TodoItemManager** olan bir **IMobileServiceSyncTable** yerine tür **IMobileServiceTable**. Tüm yerel veritabanı oluşturma, okuma, bu sınıfı kullanır güncelleştirme ve silme (CRUD) tablo işlemleri. Ne zaman bu değişiklikleri mobil uygulama arka ucuna çağırarak gönderilen karar **PushAsync** üzerinde **IMobileServiceSyncContext**. Eşitleme bağlamı izleme ve bir istemci uygulaması değiştiren tüm tablolarda değişiklikleri Ftp'den tablo ilişkileri korunmasına yardımcı olur **PushAsync** olarak adlandırılır.

    Aşağıdaki **SyncAsync** yöntemi çağrıldığında mobil uygulama arka ucu ile eşitlemek amacıyla:

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

    Bu örnekte basit hata varsayılan eşitleme işleyiciyle işleme kullanır. Özel bir kullanarak ağ koşulları ve sunucu çakışmaları gibi çeşitli hatalar gerçek bir uygulamada işlemek **IMobileServiceSyncHandler** uygulaması.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme konuları
Örnekte, **SyncAsync** yöntemi yalnızca adlı başlatma ve ne zaman bir eşitleme istedi.  Öğeleri listesinde bir Android veya iOS uygulama eşitleme başlatmak için çekmek; Windows için **eşitleme** düğmesi. Gerçek dünya uygulamada ağ durumu değiştiğinde eşitleme tetikleyici de yapabilirsiniz.

Bir çekme beklemede olan bir tabloda karşı çalıştırıldığında yerel güncelleştirmeleri önceki bağlam push işlemi otomatik olarak Tetikleyicileri istek bağlamı tarafından izlenen. Yenilerken ekleme ve bu örnek, öğeleri Tamamlanıyor, açık atlayabilirsiniz **PushAsync** çağırın.

Sağlanan kod içinde uzaktan Todoıtem tablosu tüm kayıtları seçmeleri istenir, ancak sorgu kimliği geçirerek kayıtlarını filtreleyip için sorgu mümkündür **PushAsync**. Daha fazla bilgi için bkz *Artımlı eşitleme* içinde [çevrimdışı veri eşitlemeye Azure Mobile Apps içinde][2].

## <a name="run-the-client-app"></a>İstemci uygulaması çalıştırın
Şu anda etkin çevrimdışı eşitleme ile istemci uygulamasını yerel deposu veritabanı doldurmak için her platformda en az bir kez çalıştırın. Daha sonra çevrimdışı bir senaryo benzetimini yapmak ve verileri yerel depodaki uygulama çevrimdışı durumdayken değiştirin.

## <a name="update-the-sync-behavior-of-the-client-app"></a>İstemci uygulaması eşitleme davranışını güncelleştir
Bu bölümde, çevrimdışı bir senaryo arka ucunuz için geçersiz uygulama URL'si kullanarak benzetimini yapmak için istemci projesi değiştirin. Alternatif olarak, "Uçak modu." Cihazınızı taşıyarak ağ bağlantıları devre dışı dışı bırakabilirsiniz  Veri öğeleri ekleyin veya değiştirildiğinde, bu değişiklikler yerel depoda tutulan ancak bağlantı yeniden kurulur kurulmaz kadar arka uç veri deposuna eşitlenmedi.

1. Çözüm Gezgini'nde Constants.cs proje dosyasından açın **taşınabilir** proje ve değerini değiştirmek `ApplicationURL` geçersiz bir URL'ye yönlendirin:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Todoıtemmanager.cs dosyadan Aç **taşınabilir** proje ardından ekleyin bir **catch** tabanı için **özel durum** sınıfının **try... catch** engelleyin **SyncAsync**. Bu **catch** blok özel durum iletisi aşağıdaki gibi konsola yazar:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Derleme ve istemci uygulaması çalıştırın.  Bazı yeni öğeleri ekleyin. Bir özel durum arka uç ile eşitlemek her bir denemesi için konsolunda kaydedilir dikkat edin. Mobil arka ucuna gönderilemez kadar bu yeni öğeleri yalnızca yerel depoda mevcut. Bağlı olduğu gibi arka uç, tüm oluşturma, okuma destekleyen, güncelleştirme, silme (CRUD) işlemleri istemci uygulaması davranır.
4. Uygulamasını kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.
5. (İsteğe bağlı) Arka uç veritabanındaki verileri değişmediğini görmek için Azure SQL veritabanı tablosunda görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklatıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosunda ve içeriği göz atabilirsiniz.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Mobil arka yeniden bağlanmak için İstemci uygulamayı güncelleştirme
Bu bölümde, çevrimiçi duruma gelmeye uygulama benzetim mobil arka uygulamaya yeniden bağlanın. Veri yenileme hareketi gerçekleştirdiğinizde, mobil arka ucunuza eşitlenip.

1. Constants.cs yeniden açın. Düzeltmek `applicationURL` doğru URL'ye yönlendirin.
2. Yeniden oluşturun ve istemci uygulaması çalıştırın. Uygulama başlatıldıktan sonra mobil uygulama arka ucu ile eşitleme girişiminde bulunur. Hiçbir özel durum hata ayıklama konsolunda tutulduğunu doğrulayın.
3. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanılarak güncelleştirilmiş verileri görüntülemek veya [Postman][6]. Arka uç veritabanı ve yerel depo arasında veri eşitlendiğinden dikkat edin.

    Veri ve yerel depo veritabanı arasında eşitlenen ve uygulamanızı değilken eklediğiniz öğelerini içeren dikkat edin.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps çevrimdışı veri eşitleme][2]
* [Azure Mobile Apps .NET SDK'sı nasıl yapılır][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
