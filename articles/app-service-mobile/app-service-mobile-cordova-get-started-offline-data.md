---
title: Azure mobil uygulaması (Cordova) için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: App Service Mobile Apps Cordova uygulamanızı çevrimdışı verileri önbellek ve eşitleme için kullanmayı öğrenin
documentationcenter: cordova
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 44c54b570a38eb1a3b9ca773893599d1d497dfa2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62111001"
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Cordova mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Bu öğretici Azure Mobile Apps, çevrimdışı eşitleme özelliği için Cordova tanıtır. Çevrimdışı eşitleme son kullanıcıların bir mobil uygulama ile etkileşim sağlar&mdash;görüntüleme, ekleme veya verileri değiştirme&mdash;hiçbir ağ bağlantısı olduğunda bile. Değişiklikler, yerel bir veritabanında depolanır.  Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğretici, Mobile Apps için öğreticiyi tamamladığınızda, oluşturduğunuz Cordova hızlı çözüm üzerinde dayanır [Apache Cordova Hızlı Başlangıç]. Bu öğreticide, Azure Mobile Apps çevrimdışı özellikleri eklemek için hızlı başlangıç çözüm güncelleştirin.  Biz de çevrimdışı özgü kod uygulamasında vurgulayın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]. API kullanım ayrıntıları için bkz. [API belgeleri](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Hızlı Başlangıç çözüme çevrimdışı eşitleme özelliği ekleyin
Çevrimdışı eşitleme kod uygulamasına eklenmelidir. Azure mobil uygulamaları eklentisi projeye eklendiğinde uygulamanıza otomatik olarak eklenen olan cordova sqlite depolama eklentisi, çevrimdışı eşitleme gerektirir. Hızlı başlangıç projesi bu eklentiler her ikisi de içerir.

1. Visual Studio Çözüm Gezgini'nde, index.js açın ve aşağıdaki kodu değiştirin

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    Bu kod ile:

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. Ardından, aşağıdaki kodu değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Bu kod ile:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Önceki kod eklemeleri yerel deposunu başlatmak ve kullanılan sütun değerleri eşleşen yerel bir tablo tanımlayın, Azure'da arka ucu. (Tüm sütun değerleri bu kodu eklemeniz gerekmez.)  `version` Alan mobil arka ucu tarafından korunur ve Çakışma çözümlemesi için kullanılır.

    Çağırarak eşitleme bağlamı için bir başvuru almak **getSyncContext**. Tablo ilişkileri izleme ve bir istemci uygulaması değiştiren tüm tablolardaki değişiklikleri gönderme tarafından korumak eşitleme bağlamı yardımcı `.push()` çağrılır.

3. Uygulama URL'si, mobil uygulama uygulama URL'sine güncelleştirin.

4. Ardından, bu kodu değiştirin:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Bu kod ile:

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Yukarıdaki kod, eşitleme bağlamını başlatır ve ardından yerel tablosuna bir başvuru almak için (yerine getTable) getSyncTable çağırır.

    Bu kodu kullanan tüm yerel veritabanı oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri.

    Bu örnek, basit hata eşitleme çakışmalarını işleme gerçekleştirir. Gerçek bir uygulamanın, ağ koşulları, sunucu çakışmaları ve diğerleri gibi çeşitli hatalar işlemesi. Kod örnekleri için bkz: [çevrimdışı eşitleme örnek].

5. Ardından, gerçek eşitleme gerçekleştirmek için bu işlevi ekleyin.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Çağırarak mobil uygulama arka ucuna değişiklikleri göndermek ne zaman karar **syncContext.push()** . Örneğin, çağırabilir **syncBackend** düğmesi olay işleyicisi bir eşitleme düğmeye bağlı.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme konuları

Örnekte, **anında iletme** yöntemi **syncContext** yalnızca uygulama başlangıç oturum açma için geri çağırma işlevi çağrılır.  Gerçek bir uygulamada, bu eşitleme, el ile tetiklenen işlev ya da ağ durumu değiştiğinde de yapabilirsiniz.

Beklemede olan bir tabloya karşı bir çekme yürütüldüğünde yerel güncelleştirmeleri bir anında iletme işlemi Tetikleyicileri otomatik olarak çekmesini bağlam tarafından izlenir. Yenilerken, ekleme ve bu örnekte, öğeleri Tamamlanıyor açık atlayabilirsiniz **anında iletme** yedekli olabileceğinden çağırın.

Sağlanan kod içinde uzak Todoıtem tablosu tüm kayıtları seçmeleri istenir, ancak sorgu kimliği geçirerek kayıtlarını filtrelemek ve için sorgular mümkündür **anında iletme**. Daha fazla bilgi için konudaki *Artımlı eşitleme* içinde [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="optional-disable-authentication"></a>(İsteğe bağlı) Kimlik doğrulamasını devre dışı

Geri çağırma işlevi istemiyorsanız test çevrimdışı eşitleme önce kimlik doğrulaması ayarlama, oturum açma için geri çağırma işlevi açıklama satırı yapın, ancak kod içindeki açıklamalar.  Oturum açma satırları açıklama sonra kod aşağıdaki gibidir:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Şimdi uygulamayı çalıştırdığınızda Azure arka ucu ile uygulama eşitlemeleri.

## <a name="run-the-client-app"></a>İstemci uygulaması çalıştırın
Artık etkin çevrimdışı eşitleme ile istemci uygulaması en az bir kez yerel depo veritabanını doldurmak için her platformda çalışabilir. Daha sonra çevrimdışı bir senaryonun benzetimini yapmak ve uygulamayı çevrimdışı durumdayken yerel deposundaki verileri değiştirebilirsiniz.

## <a name="optional-test-the-sync-behavior"></a>(İsteğe bağlı) Eşitleme davranışını sınama
Bu bölümde, arka ucunuz için bir geçersiz uygulama URL'si kullanılarak bir çevrimdışı senaryoda benzetimini yapmak için istemci projesini değiştirin. Veri öğeleri ekleyin veya değiştirdiğinizde, bu değişiklikleri yerel depoya tutulur, ancak arka uç veri deposuna bağlantı yeniden kurulur kadar eşitlenmedi.

1. Çözüm Gezgini'nde proje index.js dosyasını açın ve uygulama URL'si şu kod gibi geçersiz bir URL işaret edecek şekilde değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. CSP index.HTML içinde güncelleştirme `<meta>` öğesiyle aynı geçersiz URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Derleme ve istemci uygulamayı çalıştırın ve uygulama arka ucu ile oturum açtıktan sonra eşitleme girişiminde bulunduğunda bir özel durum konsolda günlüğe dikkat edin. Bunlar mobil arka ucuna gönderilen kadar eklediğiniz tüm yeni öğeler yalnızca yerel depoda mevcut. İstemci uygulaması arka ucuna bağlı olduğu gibi davranır.

4. Uygulamayı kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.

5. (İsteğe bağlı) Arka uç veritabanındaki verilerin değiştirilmesi etkiye görmek için Azure SQL veritabanı tablosunu görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklayıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosuna ve içeriği için göz atabilirsiniz.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(İsteğe bağlı) Mobil arka uç için yeniden test edin

Bu bölümde, çevrimiçi duruma geri dönmeyi uygulama taklit eden mobil arka uygulamaya yeniden bağlanın. Oturum açtığınızda, veriler mobil arka ucunuza eşitlenir.

1. İndex.js yeniden açın ve uygulama URL'sini geri yükleyin.
2. İndex.HTML yeniden açın ve uygulama URL'sini CSP'de düzeltmek `<meta>` öğesi.
3. Yeniden oluşturun ve istemci uygulaması çalıştırın. Uygulamanın, oturum açtıktan sonra mobil uygulama arka ucu ile eşitlemek çalışır. Özel durum yok hata ayıklama konsolunu açtığınızdan emin olun.
4. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanarak güncelleştirilmiş verileri görüntüleyin. Yerel depo ve arka uç veritabanı arasında veri eşitlenmiş dikkat edin.

    Uyarı verileri, veritabanı ve yerel depo arasında eşitlenmesini ve uygulamanızı değilken, eklenen öğeleri içeriyor.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Apache Cordova için Visual Studio Araçları]

## <a name="next-steps"></a>Sonraki adımlar
* Çevrimdışı eşitleme özelliklerini çakışma çözümlemesinde gibi daha gelişmiş gözden geçirme [çevrimdışı eşitleme örnek]
* Çevrimdışı eşitleme API başvurusunda gözden [API belgeleri](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[Çevrimdışı eşitleme örnek]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: https://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
