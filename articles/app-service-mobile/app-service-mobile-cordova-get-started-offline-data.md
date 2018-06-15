---
title: Azure mobil uygulaması (Cordova) için çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: Mobil uygulama hizmeti için önbellek ve eşitleme çevrimdışı veri Cordova uygulamanızda nasıl kullanılacağını öğrenin
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
ms.openlocfilehash: c12328a441a8cc438fa3e974863cc8adf8651b50
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27593723"
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Cordova mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Bu öğretici için Cordova Azure Mobile Apps, çevrimdışı eşitleme özelliği sunmaktadır. Çevrimdışı eşitleme son kullanıcıların mobil uygulama ile etkileşim sağlar&mdash;görüntüleme, ekleme veya değiştirme veri&mdash;hiçbir ağ bağlantısı olduğunda bile. Değişiklikler yerel bir veritabanında depolanır.  Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak hizmeti ile eşitlenir.

Bu öğretici Mobile Apps için öğreticiyi tamamladığınızda oluşturduğunuz Cordova hızlı başlangıç çözümü temel [Apache Cordova Hızlı Başlangıç]. Bu öğreticide, Azure Mobile Apps çevrimdışı özelliklerini eklemek için hızlı başlangıç çözüm güncelleştirin.  Biz de uygulama çevrimdışı özgü kodu vurgulayın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme]. API kullanım ayrıntıları için bkz: [API belgelerine](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Çevrimdışı eşitleme hızlı başlangıç ekleyin
Çevrimdışı eşitleme kod uygulamasına eklenmelidir. Çevrimdışı eşitleme Azure Mobile Apps eklentisi projeye dahil bağlandığınızda, uygulamanızın otomatik olarak eklenen cordova sqlite depolama eklentisi gerektiriyor. Hızlı başlangıç projesi bu eklenti her ikisi de içerir.

1. Visual Studio'nun Çözüm Gezgini'nde, index.js açın ve aşağıdaki kodu değiştirin

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    Bu kodu:

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. Ardından, aşağıdaki kodu değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Bu kodu:

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

    Önceki kod eklemeler yerel deposunu başlatmak ve kullanılan sütun değerlerini eşleşen yerel bir tablo tanımlayın Azure son yedekleyin. (Bu kodda tüm sütun değerlerini içeren gerekmez.)  `version` Alan mobil arka uç tarafından korunur ve Çakışma çözümlemesi için kullanılır.

    Eşitleme bağlamı için bir başvuru çağırarak alma **getSyncContext**. Eşitleme bağlamı izleme ve bir istemci uygulaması değiştiren tüm tablolarda değişiklikleri Ftp'den tablo ilişkileri korunmasına yardımcı olur `.push()` olarak adlandırılır.

3. Uygulama URL'si mobil uygulamayı uygulama URL'nizi güncelleştirin.

4. Ardından, bu kodu değiştirin:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Bu kodu:

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

    Önceki kod eşitleme bağlamını başlatır ve ardından yerel tablo için bir başvuru almak için (yerine getTable) getSyncTable çağırır.

    Tüm yerel veritabanı oluşturma, okuma, bu kod kullanır güncelleştirme ve silme (CRUD) tablo işlemleri.

    Bu örnekte basit hata eşitleme çakışmalarını işleme gerçekleştirir. Gerçek bir uygulamanın ağ koşulları, sunucu çakışmaları ve diğerleri gibi çeşitli hatalar işlemesi. Kod örnekleri için bkz: [çevrimdışı eşitleme örnek].

5. Ardından, gerçek eşitleme gerçekleştirmek için bu işlevi ekleyin.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Ne zaman çağırarak mobil uygulama arka ucuna değişiklikleri anında karar vermenize **syncContext.push()**. Örneğin, çağırabilirsiniz **syncBackend** düğmesi olay işleyicisi içinde bir Eşitle düğmesi bağlanır.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme konuları

Örnekte, **itme** yöntemi **syncContext** yalnızca uygulama başlangıç oturum açma için geri çağırma işlevi adı verilir.  Gerçek dünya uygulamada el ile tetiklenen bu eşitleme işlevine veya ağ durumu değiştiğinde de yapabilirsiniz.

Çekme beklemede olan bir tabloda karşı çalıştırıldığında yerel güncelleştirmeleri bir anında iletme işlemi otomatik olarak Tetikleyicileri istek bağlamı tarafından izlenir. Yenilerken ekleme ve bu örnek, öğeleri Tamamlanıyor, açık atlayabilirsiniz **itme** yedek olabileceğinden çağırın.

Sağlanan kod içinde uzaktan Todoıtem tablosu tüm kayıtları seçmeleri istenir, ancak sorgu kimliği geçirerek kayıtlarını filtreleyip için sorgu mümkündür **itme**. Daha fazla bilgi için bkz *Artımlı eşitleme* içinde [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="optional-disable-authentication"></a>(İsteğe bağlı) Kimlik doğrulaması devre dışı bırak

Sınama çevrimdışı eşitlemeden önce kimlik doğrulamasını ayarlamak için oturum açma için geri çağırma işlevi çıkışı açıklama istemediğiniz ancak içinde kod bırakırsanız geri çağırma işlevi uncommented.  Oturum açma çizgi yorum sonra kod aşağıdaki gibidir:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Şimdi, uygulama eşitlemeler uygulamayı çalıştırdığınızda Azure arka ucu ile.

## <a name="run-the-client-app"></a>İstemci uygulaması çalıştırın
Şu anda etkin çevrimdışı eşitleme ile istemci uygulaması en az bir kez yerel deposu veritabanı doldurmak için her platformda çalışabilir. Daha sonra çevrimdışı bir senaryo benzetimini yapmak ve verileri yerel depodaki uygulama çevrimdışı durumdayken değiştirin.

## <a name="optional-test-the-sync-behavior"></a>(İsteğe bağlı) Eşitleme davranışını sınama
Bu bölümde, çevrimdışı bir senaryo arka ucunuz için geçersiz uygulama URL'si kullanarak benzetimini yapmak için istemci projesi değiştirin. Eklediğinizde veya veri öğeleri değiştirmek bu değişiklikler yerel depoda tutulur, ancak arka uç veri deposuna bağlantı yeniden kurulur kurulmaz kadar eşitlenmiş değil.

1. Çözüm Gezgini'nde, index.js proje dosyasını açın ve aşağıdaki kodu gibi geçersiz bir URL işaret etmek için uygulama URL'si değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. CSP index.HTML içinde güncelleştirme `<meta>` öğesi ile aynı geçersiz URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Derleme ve istemci uygulamayı çalıştırın ve uygulama arka uç ile oturum açtıktan sonra eşitleme girişiminde bulunduğunda bir özel durum konsolda oturum dikkat edin. Bunlar mobil arka ucuna gönderilen kadar eklediğiniz tüm yeni öğeler yalnızca yerel depoda mevcut. İstemci uygulama arka ucuna bağlı gibi davranır.

4. Uygulamasını kapatın ve oluşturduğunuz yeni öğeleri yerel depoya kalıcı olduğunu doğrulamak için yeniden başlatın.

5. (İsteğe bağlı) Arka uç veritabanındaki verileri değişmediğini görmek için Azure SQL veritabanı tablosunda görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da açın **Sunucu Gezgini**. Veritabanınızda gidin **Azure**->**SQL veritabanları**. Veritabanınıza sağ tıklatıp **SQL Server nesne Gezgini'nde Aç**. Şimdi, SQL veritabanı tablosunda ve içeriği göz atabilirsiniz.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(İsteğe bağlı) Yeniden bağlanma, mobil arka uç için test etme

Bu bölümde, çevrimiçi duruma gelmeye uygulama benzetim mobil arka uygulamaya yeniden bağlanın. Oturum açtığınızda, veriler mobil arka ucunuza eşitlenip.

1. İndex.js yeniden açın ve uygulama URL'si geri yükleyin.
2. İndex.HTML yeniden açın ve CSP uygulama URL'SİNDE düzeltmek `<meta>` öğesi.
3. Yeniden oluşturun ve istemci uygulaması çalıştırın. Uygulama mobil uygulama arka ucu ile oturum açtıktan sonra eşitleme girişiminde bulunur. Hiçbir özel durum hata ayıklama konsolunda tutulduğunu doğrulayın.
4. (İsteğe bağlı) SQL Server Nesne Gezgini veya Fiddler gibi bir REST araç kullanılarak güncelleştirilmiş verileri görüntüleyin. Arka uç veritabanı ve yerel depo arasında veri eşitlendiğinden dikkat edin.

    Veri ve yerel depo veritabanı arasında eşitlenen ve uygulamanızı değilken eklediğiniz öğelerini içeren dikkat edin.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Mobile Apps çevrimdışı veri eşitleme]
* [Apache Cordova için Visual Studio Araçları]

## <a name="next-steps"></a>Sonraki adımlar
* Çevrimdışı eşitleme özelliklerini çakışma çözümlemesinde gibi daha gelişmiş gözden [çevrimdışı eşitleme örnek]
* Çevrimdışı eşitleme API'si başvurusunda gözden [API belgelerine](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova Hızlı Başlangıç]: app-service-mobile-cordova-get-started.md
[çevrimdışı eşitleme örnek]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Azure Mobile Apps çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
