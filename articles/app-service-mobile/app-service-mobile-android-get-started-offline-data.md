---
title: Azure mobil uygulaması (Android) için çevrimdışı eşitlemeyi etkinleştirme
description: App Service Mobile Apps Android uygulamanızda çevrimdışı veri önbelleği ve eşitleme için kullanmayı öğrenin
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
ms.openlocfilehash: a20c79acce8c9dc9051651a0473fd07b8e62f5de
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126911"
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Android mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Android için Azure Mobile Apps, çevrimdışı eşitleme özelliği kapsar. Çevrimdışı eşitleme son kullanıcıların bir mobil uygulama ile etkileşim sağlar&mdash;görüntüleme, ekleme veya verileri değiştirme&mdash;hiçbir ağ bağlantısı olduğunda bile. Değişiklikler, yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduğunda, bu değişiklikleri uzak arka uç ile eşitlenir.

Azure Mobile Apps ile ilk deneyiminiz varsa, ilk öğreticiyi tamamlamanız gereken [Android uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için Ek Yardım konusuna [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="update-the-app-to-support-offline-sync"></a>Çevrimdışı eşitleme desteklemek için uygulamayı güncelleştirme
Çevrimdışı Eşitleme ile okuma ve yazma gelen bir *tablosunu eşitleme* (kullanarak *IMobileServiceSyncTable* arabirimi), parçası olduğu bir **SQLite** Cihazınızda veritabanı.

Gönderme ve çekme cihaz ve Azure Mobile Services arasındaki değişiklikleri için kullandığınız bir *eşitleme bağlamı* (*MobileServiceClient.SyncContext*), hangi depolamak için yerel veritabanı ile başlatılamıyor yerel olarak veri.

1. İçinde `TodoActivity.java`, mevcut tanımı yorum `mToDoTable` ve eşitleme tablo sürümü açıklamasını kaldırın:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. İçinde `onCreate` yöntemi, mevcut başlatılması yorum `mToDoTable` ve bu tanımı açıklamasını kaldırın:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. İçinde `refreshItemsFromTable` tanımını yorum `results` ve bu tanımı açıklamasını kaldırın:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Açıklama satırı tanımını `refreshItemsFromMobileServiceTable`.
5. Tanımına ilişkin açıklamayı kaldırır `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Tanımına ilişkin açıklamayı kaldırır `sync`:
   
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

## <a name="test-the-app"></a>Uygulamayı test edin
Bu bölümde, Wi-Fi ile davranışı üzerinde test ve bir çevrimdışı senaryoda oluşturmak için Wi-Fi ' ı açın.

Veri öğeleri eklediğinizde, bunlar yerel bir SQLite deposunda tutulan, ancak tuşuna kadar mobil hizmete eşitlenmedi **Yenile** düğmesi. Diğer uygulamalara veri eşitlenmesi gerektiğinde ilgili farklı gereksinimleri olabilir, ancak tanıtım amacıyla bu öğreticide, onu açıkça talep kullanıcının.

Bu düğmesine bastığınızda, yeni bir arka plan görevini başlatır. İlk eşitleme bağlamı, ardından Azure çeken tüm değiştirilen verileri yerel tabloya kullanarak yerel depo için yapılan tüm değişiklikler anında bildirim gönderir.

### <a name="offline-testing"></a>Çevrimdışı test
1. Cihaz veya simülatör'de *uçak modu*. Bu bir çevrimdışı senaryoda oluşturur.
2. Bazı ekleme *ToDo* öğeleri veya bazı öğeleri tamamlandı olarak işaretle. Cihaz veya simülatör Çık (veya zorla uygulamayı kapatmak) ve yeniden başlatın. Değişikliklerinizi bunlar tutulur çünkü cihazda yerleştirildi doğrulayın yerel bir SQLite depodaki.
3. Azure içeriğini görüntülemek *Todoıtem* ile bir SQL aracı gibi tablo *SQL Server Management Studio*, veya bir REST istemcisi gibi *Fiddler* veya  *Postman*. Yeni öğelerin bulunduğunu doğrulayın *değil* olan sunucuya eşitlendi
   
       + Node.js arka ucu için Git [Azure portalında](https://portal.azure.com/), mobil uygulamanızın arka ucu tıklayın **kolay tablolar** > **Todoıtem** içeriğinigörüntülemekiçin`TodoItem`tablo.
       + Bir .NET arka ucu için bir SQL ile ya da aracı gibi tablo içeriklerini görüntülemek *SQL Server Management Studio*, veya bir REST istemcisi gibi *Fiddler* veya *Postman*.
4. Wi-Fi, cihaz veya simülatör'ü etkinleştirin. Ardından, basın **Yenile** düğmesi.
5. Todoıtem veriler, Azure portalında yeniden görüntüleyin. Yeni ve değiştirilen Todoıtems görüntülenmesi gerekir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Cloud Cover: Azure mobil Hizmetleri çevrimdışı eşitleme] \(Not: video Mobile Services'de olsa da, çevrimdışı eşitleme Azure Mobile Apps benzer şekilde çalışır\)

<!-- URLs. -->

[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md

[Android uygulaması oluşturma]: app-service-mobile-android-get-started.md

[Cloud Cover: Azure mobil Hizmetleri çevrimdışı eşitleme]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

