---
title: "Azure Mobile uygulamanız (Android) için çevrimdışı eşitlemeyi etkinleştirme"
description: "App Service Mobile Apps için önbellek ve eşitleme çevrimdışı veri Android uygulamanızdaki nasıl kullanılacağını öğrenin"
documentationcenter: android
author: conceptdev
manager: crdun
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 152702bed0ea061c3cb86e2ff6f88bf204f9d243
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Android mobil uygulamanızı için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, Android için Azure Mobile Apps, çevrimdışı eşitleme özelliği kapsar. Çevrimdışı eşitleme son kullanıcıların mobil uygulama ile etkileşim sağlar&mdash;görüntüleme, ekleme veya değiştirme veri&mdash;hiçbir ağ bağlantısı olduğunda bile. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak arka uç ile eşitlenir.

Bu ilk Azure Mobile Apps deneyiminizi ise, öğreticinin ilk tamamlanmalıdır [Android uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps çevrimdışı veri eşitleme].

## <a name="update-the-app-to-support-offline-sync"></a>Çevrimdışı eşitleme desteklemek için uygulamayı güncelleştirme
Çevrimdışı eşitleme için okuma ve yazma gelen bir *eşitleme tablo* (kullanarak *IMobileServiceSyncTable* arabirimi), parçası olduğu bir **SQLite** aygıtınızda veritabanı.

İtme ve Azure Mobile Services ve aygıt arasındaki değişiklikleri çekme için kullandığınız bir *eşitleme bağlamı* (*MobileServiceClient.SyncContext*), hangi depolamak için yerel veritabanını başlatılamadı yerel veriler.

1. İçinde `TodoActivity.java`, varolan tanımı çıkışı açıklama `mToDoTable` ve eşitleme tablo sürümü açıklamadan çıkarın:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. İçinde `onCreate` yöntemi, mevcut başlatılması çıkışı açıklama `mToDoTable` ve bu tanımı açıklamadan çıkarın:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. İçinde `refreshItemsFromTable` tanımını çıkışı açıklama `results` ve bu tanımı açıklamadan çıkarın:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Açıklama tanımını çıkışı `refreshItemsFromMobileServiceTable`.
5. Tanımını açıklamadan çıkarın `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Tanımını açıklamadan çıkarın `sync`:
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Uygulamayı test etme
Bu bölümde, WiFi ile davranışı üzerinde test ve çevrimdışı bir senaryo oluşturmak için WiFi devre dışı bırakma.

Veri öğeleri eklediğinizde, bunlar yerel SQLite deposunda tutulur, ancak tuşuna kadar mobil hizmete eşitlenmedi **yenileme** düğmesi. Diğer uygulamalara veri eşitlenmesi gerektiği zaman ilgili farklı gereksinimlere sahip, ancak tanıtım amacıyla bu öğreticide açıkça isteği kullanıcı vardır.

Bu düğmesine bastığınızda, yeni bir arka plan görevi başlatır. İlk eşitleme bağlamı sonra Azure çeken tüm değiştirilen verileri yerel tabloya kullanarak yerel depolama için yapılan tüm değişiklikler de iter.

### <a name="offline-testing"></a>Çevrimdışı test etme
1. Cihaz veya benzeticisinde yerleştirin *uçak modu*. Bu, çevrimdışı bir senaryo doğurur.
2. Bazı eklemek *Yapılacaklar* öğeler veya bazı öğeler tamamlandı olarak işaretle. Cihaz veya simulator çıkın (veya zorla uygulamayı kapatmak) ve yeniden başlatın. Tutuluyor çünkü yaptığınız değişiklikler aygıtta devam ediyor olduğunu doğrulayın yerel SQLite depodaki.
3. Azure içeriğini görüntüleyebilir *Todoıtem* SQL aracıyla ya da gibi tablo *SQL Server Management Studio*, veya bir REST istemcisi gibi *Fiddler* veya  *Postman*. Yeni öğelerin bulunduğunu doğrulayın *değil* edilmiş sunucuya eşitlendi
   
       + Node.js arka ucu için Git [Azure portal](https://portal.azure.com/), tıklatıp mobil uygulamanızın arka uç **kolay tablolar** > **Todoıtem** içeriğinigörüntülemekiçin`TodoItem`tablo.
       + Bir .NET arka ucu için bir SQL ile ya da aracı gibi İçindekiler görüntülemek *SQL Server Management Studio*, veya bir REST istemcisi gibi *Fiddler* veya *Postman*.
4. WiFi üzerinde aygıt ya da simulator açın. Ardından, basın **yenileme** düğmesi.
5. Todoıtem verileri Azure portalında yeniden görüntüleyin. Yeni ve değiştirilmiş Todoıtems şimdi gösterilmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps çevrimdışı veri eşitleme]
* [Bulut kapsar: Azure Mobile Services'ın çevrimdışı eşitleme] \(Not: video Mobile Services'de olsa da, çevrimdışı eşitleme Azure Mobile Apps benzer şekilde çalışır\)

<!-- URLs. -->

[Azure Mobile Apps çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md

[Android uygulaması oluşturma]: app-service-mobile-android-get-started.md

[Bulut kapsar: Azure Mobile Services'ın çevrimdışı eşitleme]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

