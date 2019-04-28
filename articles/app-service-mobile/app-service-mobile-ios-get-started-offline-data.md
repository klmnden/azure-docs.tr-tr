---
title: İOS mobil uygulamalarla çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs
description: İOS uygulamalarında Azure App Service mobil uygulamalar, çevrimdışı veri önbelleği ve eşitleme için kullanmayı öğrenin.
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 1283f812799fe71ef6987dbc7fab092aed4d3417
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112659"
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>İOS mobil uygulamalarla çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Azure App Service Mobile Apps özelliğini iOS için çevrimdışı eşitleme kapsar. Çevrimdışı eşitleme son kullanıcılara görüntüleme, ekleme veya bunlar hiçbir ağ bağlantısı olduğunda bile verileri değiştirmek için bir mobil uygulama ile etkileşim kurabilir. Değişiklikler, yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduktan sonra değişiklikleri uzak arka ucu ile eşitlenir.

Mobile Apps ile ilk deneyiminiz varsa, ilk öğreticiyi tamamlamanız gereken [bir iOS uygulaması oluşturma]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için bkz: [Mobile apps'te çevrimdışı veri eşitleme].

## <a name="review-sync"></a>İstemci eşitleme kodu gözden geçirin
İçin indirdiğiniz istemci projesi [bir iOS uygulaması oluşturma] öğretici temel veri tabanlı bir yerel veritabanı çevrimdışı eşitlemeyi destekleyen kodu zaten içeriyor. Bu bölümde, eğitmen kodu zaten dahil özetlenmektedir. Özellik kavramsal bir genel bakış için bkz: [Mobile apps'te çevrimdışı veri eşitleme].

Ağ erişilemez olduğunda bile Mobile Apps, çevrimdışı veri eşitleme özelliğini kullanarak yerel bir veritabanı ile son kullanıcılar etkileşim kurabilir. Uygulamanızda bu özellikleri kullanmak için eşitleme bağlamı başlatılamıyor. `MSClient` ve yerel bir depo başvuru. Tablonuzu aracılığıyla başvuru sonra **MSSyncTable** arabirimi.

İçinde **QSTodoService.m** (Objective-C) veya **ToDoTableViewController.swift** dikkat (Swift), üye türünü **syncTable** olduğu **MSSyncTable** . Çevrimdışı eşitleme yerine bu eşitleme tablo arabirimi kullanır **MSTable**. Bir eşitleme tablo kullanıldığında, tüm işlemler yerel depoya gidin ve yalnızca açık gönderme ve çekme işlemleri ile uzak arka ucu ile eşitlenir.

 Bir eşitleme tablosuna bir başvuru almak için kullanın **syncTableWithName** metodunda `MSClient`. Çevrimdışı eşitleme işlevselliği kaldırmak için **tableWithName** yerine.

Yerel depo, herhangi bir tablo işlem gerçekleştirilmeden önce başlatılmalıdır. İlgili kod aşağıdaki gibidir:

* **Objective-C**. İçinde **QSTodoService.init** yöntemi:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **Swift**. İçinde **ToDoTableViewController.viewDidLoad** yöntemi:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Bu yöntemi kullanarak yerel bir depo oluşturur `MSCoreDataStore` arabirimi, Mobile Apps SDK'sı sağlar. Uygulayarak farklı bir yerel depo alternatif olarak, sağlayabilir `MSSyncContextDataSource` protokolü. Ayrıca, ilk parametresinin **MSSyncContext** çakışma işleyicisi belirtmek için kullanılır. Geçirilen olduğundan `nil`, üzerinde herhangi bir çakışma başarısız varsayılan çakışma işleyici aldığımız.

Şimdi, şimdi gerçek eşitleme işlemi ve uzak bir arka uçtan veri alın:

* **Objective-C**. `syncData` ilk yeni değişiklikleri gönderir ve sonra çağıran **pullData** uzak arka ucundan veri almak için. Buna karşılık, **pullData** yöntemi bir sorguyla eşleşen yeni verileri alır:

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in the sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from the remote server into the local table.
       // We're pulling all items and filtering in the view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets the caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* **Swift**:
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc. via the MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep the server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation to item \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting to server's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

Objective-C sürüm içinde `syncData`, ilk diyoruz **pushWithCompletion** üzerinde eşitleme bağlamı. Bu yöntem bir üyesidir `MSSyncContext` (ve eşitleme tablonun kendisini değil) çünkü tüm tabloları arasında değişiklikleri gönderir. Yerel olarak (CUD işlemleri üzerinden) şekilde değiştirilmiş kayıtlar sunucuya gönderilir. Ardından yardımcı **pullData** çağrılır, çağıran **MSSyncTable.pullWithQuery** uzak verileri almak ve yerel veritabanında depolamak için.

Swift sürüm olmadığından zorlama işlemi kesinlikle gerekli yoktur hiçbir **pushWithCompletion**. Her zaman bir anında iletme işlemi yapıyor tablo için eşitleme bağlamı bekleyen herhangi bir değişiklik varsa, ilk sorunları bir anında iletme çekin. Ancak, birden fazla eşitleme tablonuz varsa, her şeyi ilgili tabloları arasında tutarlı olmasını sağlamak için anında iletme açıkça çağırmak en iyisidir.

Hem Objective-C ve Swift sürümlerinde kullanabileceğiniz **pullWithQuery** almak istediğiniz kayıtları filtrelemek için bir sorgu belirtmek için yöntemi. Bu örnekte sorgu, uzak tüm kayıtları alır `TodoItem` tablo.

İkinci parametresi **pullWithQuery** için kullanılan bir sorgu kimliği *Artımlı eşitleme*. Artımlı eşitleme yalnızca kaydın kullanarak en son eşitlemeden bu yana değiştirilmiş olan kayıtları alır `UpdatedAt` zaman damgası (adlı `updatedAt` Yerel Depodaki.) Sorgu kimliği uygulamanızda mantıksal her sorgu için benzersiz açıklayıcı bir dize olmalıdır. Artımlı eşitleme dışında bırakmak geçirmek `nil` olarak sorgu kimliği Bu yaklaşım, çünkü her çekme işlemi tüm kayıtları alır potansiyel olarak verimsiz olabilir.

Objective-C uygulama, değiştirme veya kullanıcı yenileme hareket gerçekleştirdiğinde veri eklediğinizde ve başlatmada eşitler.

Swift uygulaması, kullanıcının yenileme hareket gerçekleştirdiğinde ve başlatmada eşitler.

Veri olduğunda uygulama eşitlemeler (Objective-C) değiştirdiğinden ya da uygulama (Objective-C ve Swift) başlatıldığında uygulama kullanıcının çevrimiçi olduğunu varsayar. Böylece kullanıcılar çevrimdışı olduklarında bile düzenleyebilirsiniz, bir sonraki bölümde, uygulama güncelleştirilir.

## <a name="review-core-data"></a>Temel veri modeli gözden geçirin
Temel veri çevrimdışı depolama kullandığınızda, belirli tablolar ve alanlar veri modelinizde tanımlamanız gerekir. Örnek uygulamayı zaten doğru biçime sahip bir veri modeli içerir. Bu bölümde, nasıl kullanıldıkları göstermek için bu tablolar inceleyeceğiz.

Açık **QSDataModel.xcdatamodeld**. Dört tablo tanımlanmış--üç SDK tarafından kullanılır ve yapılacaklar için kullanılan bir kendilerini öğeleri:
  * MS_TableOperations: Sunucu ile eşitlenmesi gereken öğeleri izler.
  * MS_TableOperationErrors: Çevrimdışı eşitleme sırasında gerçekleşen hataları izler.
  * MS_TableConfig: Parçaları son zaman tüm çekme işlemleri için son eşitleme işlemi için güncelleştirildi.
  * Todoıtem: Yapılacaklar öğelerini depolar. Sistem sütunlarıdır **createdAt**, **updatedAt**, ve **sürüm** isteğe bağlı sistem özelliklerdir.

> [!NOTE]
> Mobile Apps SDK'sı ile başlayan sütun adlarını saklar "**``**". Bu ön ek sistem sütun dışında herhangi bir şeyle kullanmayın. Aksi takdirde, uzak arka ucu kullanırken, sütun adları değiştirilmiştir.
>
>

Çevrimdışı eşitleme özelliği kullandığınızda, üç Sistem tabloları ve veri tablosunu tanımlayın.

### <a name="system-tables"></a>Sistem tabloları

**MS_TableOperations**  

![MS_TableOperations tablo öznitelikleri][defining-core-data-tableoperations-entity]

| Öznitelik | Tür |
| --- | --- |
| id | Integer 64 |
| itemId | String |
| properties | İkili veriler |
| tablo | String |
| tableKind | Tamsayı 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tablo öznitelikleri][defining-core-data-tableoperationerrors-entity]

| Öznitelik | Tür |
| --- | --- |
| id |String |
| operationId |Integer 64 |
| properties |İkili veriler |
| tableKind |Tamsayı 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Öznitelik | Tür |
| --- | --- |
| id |String |
| anahtar |String |
| KeyType |Integer 64 |
| tablo |String |
| value |String |

### <a name="data-table"></a>Veri tablosu

**Todoıtem**

| Öznitelik | Tür | Not |
| --- | --- | --- |
| id | Dize, gerekli olarak işaretlenmiş |Uzak depoda birincil anahtar |
| Tamamlayın | Boolean | Yapılacak iş öğesi alanı |
| metin |String |Yapılacak iş öğesi alanı |
| createdAt | Tarih | (isteğe bağlı) Eşlendiği **createdAt** sistem özelliği |
| updatedAt | Tarih | (isteğe bağlı) Eşlendiği **updatedAt** sistem özelliği |
| version | String | (isteğe bağlı) Çakışmalar, sürüm eşlenir algılamak için kullanılan |

## <a name="setup-sync"></a>Uygulama eşitleme davranışını değiştirme
Bu bölümde, uygulama başlangıç veya ne zaman eklemek ve öğeleri güncelleştirme eşitlemez şekilde uygulamayı değiştirin. Yalnızca hareketi Yenile düğmesini gerçekleştirildiğinde, eşitler.

**Objective-C**:

1. İçinde **QSTodoListViewController.m**, değiştirme **viewDidLoad** yöntemi çağrısını kaldırın `[self refresh]` yönteminin sonunda. Artık uygulama başlangıç menüsünde sunucusu ile veri eşitlenmedi. Bunun yerine, yerel depo içeriğini eşitlenmiş.
2. İçinde **QSTodoService.m**, tanımını değiştirme `addItem` böylece öğesi eklendikten sonra eşitleme değil. Kaldırma `self syncData` engelleme ve içerikleri şu kodla değiştirin:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Tanımını değiştirme `completeItem` daha önce belirtildiği gibi. Kaldırmak için bloğu `self syncData` aşağıdakiyle değiştirin:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**Swift**:

İçinde `viewDidLoad`, **ToDoTableViewController.swift**, uygulama başlatıldığında eşitleme durdurmak için burada gösterilen iki satırı yorum. Bu makalenin yazıldığı sırada, birisi ekler veya bir öğe tamamlanan Swift Todo uygulaması hizmet güncelleştirmez. Bu hizmet yalnızca uygulama başlangıç menüsünde güncelleştirir.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>Uygulamayı test etme
Bu bölümde, çevrimdışı bir senaryonun benzetimini yapmak için geçersiz bir URL bağlanın. Veri öğeleri eklediğinizde, bunlar içinde tutulan yerel çekirdek verileri depolamak, ancak bunlar mobil uygulama arka ucu ile eşitlenmedi.

1. Mobil uygulama URL'yi değiştirmeniz **QSTodoService.m** geçersiz URL ve uygulamayı yeniden çalıştırın:

   **Objective-C**. QSTodoService.m içinde:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **Swift**. ToDoTableViewController.swift içinde:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Bazı Yapılacaklar öğelerini ekleyin. Simülatör Çık (veya zorla uygulamayı kapatmak) ve yeniden başlatın. Yaptığınız değişiklikleri kalıcı olduğunu doğrulayın.

3. Uzak içeriğini görüntülemek **Todoıtem** tablosu:
   * Node.js arka ucu için Git [Azure portalında](https://portal.azure.com/) ve, mobil uygulama arka ucu içinde tıklayın **kolay tablolar** > **Todoıtem**.  
   * Bir .NET geri için bitiş, SQL Server Management Studio gibi bir SQL aracı veya Fiddler veya Postman gibi bir REST istemcisi kullanın.  

4. Yeni öğelerin bulunduğunu doğrulayın *değil* edilmiş sunucuyla eşitlenir.

5. Doğru bir şekilde geri URL'yi değiştirmeniz **QSTodoService.m**, uygulamayı yeniden çalıştırın.

6. Öğeleri listede aşağı çekerek yenileme hareketi gerçekleştirin.  
Devam eden bir değer değiştirici görüntülenir.

7. Görünüm **Todoıtem** tekrar veri. Yeni ve değiştirilen Yapılacaklar öğelerini artık görüntülenmesi gerekir.

## <a name="summary"></a>Özet
Çevrimdışı eşitleme özelliği desteklemek için kullandığımız `MSSyncTable` arabirim ve başlatılmış `MSClient.syncContext` ile yerel bir depo. Bu durumda, yerel depo temel veri tabanlı bir veritabanı oluştu.

Temel veri yerel bir depo kullandığınızda, birçok tabloları ile tanımlamalıdır [düzeltmek Sistem Özellikleri](#review-core-data).

Normal oluşturma, okuma, güncelleştirme ve uygulama hala bağlı, ancak karşı yerel depoya tüm işlemleri ortaya gibi mobil uygulamalar iş için (CRUD) işlemleri Sil.

Yerel depo sunucuyla eşitlediğinizde kullandık **MSSyncTable.pullWithQuery** yöntemi.

## <a name="additional-resources"></a>Ek kaynaklar
* [Mobile apps'te çevrimdışı veri eşitleme]
* [Cloud Cover: Azure mobil Hizmetleri çevrimdışı eşitleme] \(mobil hizmetler hakkında video olduğunda, ancak benzer şekilde Mobile Apps çevrimdışı eşitleme çalışır.\)

<!-- URLs. -->


[Bir iOS uygulaması oluşturma]: app-service-mobile-ios-get-started.md
[Mobile apps'te çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud Cover: Azure mobil Hizmetleri çevrimdışı eşitleme]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
