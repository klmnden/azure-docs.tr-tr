---
title: "İOS mobil uygulamaları ile çevrimdışı eşitlemeyi etkinleştirme | Microsoft Docs"
description: "Önbellek ve eşitleme çevrimdışı veri için Azure App Service mobile apps'de iOS uygulamalarında kullanmayı öğrenin."
documentationcenter: ios
author: conceptdev
manager: crdun
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: b676b51241e4883fb1b4c40caba8e281bfa68a4c
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Çevrimdışı iOS mobil uygulamaları ile eşitlemeyi etkinleştir
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Azure App Service Mobile Apps özelliğini iOS için çevrimdışı eşitleme kapsar. Çevrimdışı eşitleme son kullanıcılara görüntülemek, eklemek veya bile ağ bağlantısı olduğunda verileri değiştirmek için mobil uygulama ile etkileşim kurabilirsiniz. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduktan sonra değişiklikler uzak arka uç ile eşitlenir.

Mobile Apps ile ilk deneyiminizi varsa öğreticinin ilk tamamlanmalıdır [iOS uygulaması oluştur]. İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, veri erişimi uzantısı paketlerini projenize eklemeniz gerekir. Server uzantısı paketleri hakkında daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi için bkz: [mobil uygulamalarda çevrimdışı veri eşitlemeye].

## <a name="review-sync"></a>İstemci eşitleme kodu gözden geçirme
İçin indirdiğiniz istemci projesi [iOS uygulaması oluştur] öğretici zaten bir yerel çekirdek veri tabanlı veritabanı çevrimdışı eşitlemeyi destekleyen kodunu içerir. Bu bölümde, eğitmen kodu zaten dahil özetlenmektedir. Özellik kavramsal bir genel bakış için bkz: [mobil uygulamalarda çevrimdışı veri eşitlemeye].

Ağ erişilemez durumda olduğunda bile Mobile Apps çevrimdışı veri eşitleme özelliğini kullanarak, son kullanıcılar ile yerel bir veritabanı etkileşimli olarak çalışabilir. Bu özellikler, uygulamanızda kullanmak için eşitleme bağlamı başlatma `MSClient` ve yerel bir depo başvuru. Tablonuz aracılığıyla başvuru sonra **MSSyncTable** arabirimi.

İçinde **QSTodoService.m** (Objective-C) veya **ToDoTableViewController.swift** (Swift), dikkat üye türü **syncTable** olan **MSSyncTable** . Çevrimdışı eşitleme kullanan bu eşitleme tablo arabirimi yerine **MSTable**. Bir eşitleme tablo kullanıldığında, tüm işlemleri yerel mağazaya gidin ve yalnızca açık anında iletme ve çekme işlemleri ile uzaktan arka uç ile eşitlenir.

 Bir eşitleme tablo için bir başvuru almak için **syncTableWithName** yöntemi `MSClient`. Çevrimdışı eşitleme işlevselliği kaldırmak için kullanın **tableWithName** yerine.

Tablo işlemleri gerçekleştirilmeden önce yerel deposu başlatılması gerekir. İlgili kod aşağıdaki gibidir:

* **Objective-C**. İçinde **QSTodoService.init** yöntemi:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **SWIFT**. İçinde **ToDoTableViewController.viewDidLoad** yöntemi:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Bu yöntemi kullanarak yerel bir depo oluşturur `MSCoreDataStore` Mobile Apps SDK'sı sağlayan arabirim. Alternatif olarak, size uygulayarak farklı bir yerel deposu sağlayabilir `MSSyncContextDataSource` protokolü. Ayrıca, ilk parametresinin **MSSyncContext** bir çakışma işleyici belirtmek için kullanılır. Biz başarılı olduğundan `nil`, biz üzerinde herhangi bir çakışmanın başarısız varsayılan çakışma işleyici alın.

Şimdi, şimdi gerçek eşitleme işlemi yapmak ve verileri uzak arka uçtan alın:

* **Objective-C**. `syncData`Yeni değişiklikler ilk iter ve çağırır **pullData** uzak arka uçtan veri almak için. Buna karşılık, **pullData** yöntemi bir sorguyla eşleşen yeni verileri alır:

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
* **SWIFT**:
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via the MSSyncContextDelegate
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

Objective-C sürümünde içinde `syncData`, ilk diyoruz **pushWithCompletion** üzerinde eşitleme bağlamı. Bu yöntem bir üyesi olan `MSSyncContext` (ve eşitleme tablonun kendisini değil) tüm tablolar arasında değişiklikleri iter olduğundan. Yerel olarak (CUD işlemleri üzerinden) şekilde değiştirilmiş kayıt sunucusuna gönderilir. Ardından yardımcı **pullData** çağrıldığında hangi çağrıları **MSSyncTable.pullWithQuery** uzak verileri almak ve yerel veritabanında depolamak için.

SWIFT sürümde olmadığından zorlama işlemi kesinlikle gerekli yoktur hiçbir çağrısına **pushWithCompletion**. Gönderme işlemi yapıyor tablosu için eşitleme bağlamındaki bekleyen tüm değişiklikleri varsa, çekme her zaman bir itme ilk verir. Ancak, birden fazla eşitleme tablo varsa, her şeyi ilişkili tablolar arasında tutarlı olduğundan emin olmak için anında iletme açıkça çağırmak en iyisidir.

Objective-C hem SWIFT sürümleri de, kullanabileceğiniz **pullWithQuery** yöntemi almak istediğiniz kayıtlarını filtrelemek için bir sorgu belirtin. Bu örnekte sorgu, uzak tüm kayıtları alır `TodoItem` tablo.

Öğesinin ikinci parametresi, **pullWithQuery** için kullanılan bir sorgu kimliği *Artımlı eşitleme*. Artımlı eşitleme alır kaydın kullanarak en son eşitlemeden sonra değiştirilen kayıtları `UpdatedAt` zaman damgası (adlı `updatedAt` Yerel Depodaki.) Sorgu kimliği, uygulamanızda mantıksal her sorgu için benzersiz tanımlayıcı bir dize olmalıdır. Artımlı eşitleme dışında kabul etmek için geçirmek `nil` sorgu kimliği olarak Her çekme işlemi tüm kayıtları aldığı için bu yaklaşım olası verimsiz olabilir.

Objective-C uygulama değiştirmek veya bir kullanıcı yenileme hareketi gerçekleştirdiğinde veri eklediğinizde ve başlatma eşitlenir.

SWIFT uygulama kullanıcı yenileme hareketi gerçekleştirdiğinde ve başlatma eşitlenir.

Veri olduğunda uygulama eşitlemeler (Objective-C) değiştirdiğinden veya uygulama (Objective-C ve Swift) başlattığında, uygulama kullanıcının çevrimiçi olduğunu varsayar. Sonraki bölümde, böylece kullanıcılar çevrimdışı olsalar bile düzenleyebilirsiniz uygulamayı güncelleştirir.

## <a name="review-core-data"></a>Çekirdek veri modeli gözden geçirin
Çekirdek veri çevrimdışı deposu kullandığınızda, veri modelinizi belirli tabloları ve alanları tanımlamanız gerekir. Örnek uygulaması zaten doğru biçime sahip bir veri modeli içerir. Bu bölümde, nasıl kullanıldıkları göstermek için bu tablolar yol.

Açık **QSDataModel.xcdatamodeld**. Dört tablonun tanımlanmış--SDK tarafından kullanılan üç ve yapılacaklar için kullanılan bir kendilerini öğeleri:
  * MS_TableOperations: sunucusuyla eşitlenmesi gereken öğeleri izler.
  * MS_TableOperationErrors: Çevrimdışı eşitleme sırasında gerçekleşen hataları izler.
  * MS_TableConfig: Parçaları tüm çekme işlemleri için son eşitleme işlemi için zaman son güncelleştirildi.
  * Todoıtem: Yapılacaklar öğelerini depolar. Sistem sütunları **createdAt**, **updatedAt**, ve **sürüm** isteğe bağlı sistem özelliklerdir.

> [!NOTE]
> Mobile Apps SDK'sı ile başlayan sütun adları ayırır "**``**". Bu ön ek sistem sütunlar dışındaki herhangi bir şeyle kullanmayın. Aksi takdirde, uzak arka uç kullandığınızda, sütun adlarının değiştirilir.
>
>

Çevrimdışı eşitleme özelliğini kullandığınızda, üç Sistem tabloları ve veri tablosu tanımlayın.

### <a name="system-tables"></a>Sistem tabloları

**MS_TableOperations**  

![MS_TableOperations tablo öznitelikleri][defining-core-data-tableoperations-entity]

| Öznitelik | Tür |
| --- | --- |
| id | Tamsayı 64 |
| öğe kimliği | Dize |
| properties | İkili veriler |
| tablo | Dize |
| tableKind | Tamsayı 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tablo öznitelikleri][defining-core-data-tableoperationerrors-entity]

| Öznitelik | Tür |
| --- | --- |
| id |Dize |
| Operationıd |Tamsayı 64 |
| properties |İkili veriler |
| tableKind |Tamsayı 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Öznitelik | Tür |
| --- | --- |
| id |Dize |
| anahtar |Dize |
| keyType |Tamsayı 64 |
| tablo |Dize |
| değer |Dize |

### <a name="data-table"></a>Veri tablosu

**Todoıtem**

| Öznitelik | Tür | Not |
| --- | --- | --- |
| id | Dize, gerekli olarak işaretlenmiş |Uzak Depolama birincil anahtar |
| Tamamlayın | Boole | Yapılacak iş öğesi alanı |
| Metin |Dize |Yapılacak iş öğesi alanı |
| CreatedAt | Tarih | (isteğe bağlı) Eşlendiği **createdAt** sistem özelliği |
| updatedAt | Tarih | (isteğe bağlı) Eşlendiği **updatedAt** sistem özelliği |
| sürüm | Dize | (isteğe bağlı) Çakışmaları, maps sürüme algılamak için kullanılan |

## <a name="setup-sync"></a>Uygulama eşitleme davranışını değiştirme
Bu bölümde, uygulama başlatma ya da zaman eklemek ve öğeleri güncelleştirme üzerinde eşitlemez şekilde uygulama değiştirin. Yalnızca Yenile hareketi düğmesini gerçekleştirildiğinde eşitlenir.

**Objective-C**:

1. İçinde **QSTodoListViewController.m**, değiştirme **viewDidLoad** yöntemi çağrısı kaldırmak için `[self refresh]` yöntemi sonunda. Şimdi, uygulama başlatılırken sunucusuyla veri eşitlenmedi. Bunun yerine, Yerel Depodaki içeriğini eşitlenip.
2. İçinde **QSTodoService.m**, tanımını değiştirmek `addItem` böylece öğeyi eklendikten sonra eşitleme değil. Kaldırma `self syncData` engelleme ve şununla değiştirin:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Tanımını değiştirmek `completeItem` daha önce belirtildiği gibi. Bloğunu kaldırmak `self syncData` ve şununla değiştirin:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**SWIFT**:

İçinde `viewDidLoad`, **ToDoTableViewController.swift**, uygulama başlangıç eşitlemeyi durdurma için burada gösterilen iki satırları açıklama. Bu yazma zaman birisi ekler veya öğeyi tamamlandıktan Swift Todo uygulaması hizmet güncelleştirmez. Uygulama başlangıç hizmette yalnızca güncelleştirir.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>Uygulamayı test etme
Bu bölümde, çevrimdışı bir senaryo benzetimini yapmak için geçersiz bir URL bağlayın. Veri öğeleri eklediğinizde, bunlar içinde tutulan yerel çekirdek verileri depolamak, ancak bunlar mobil uygulama arka ucu ile eşitlenmedi.

1. Mobil uygulama URL'sini değiştirmek **QSTodoService.m** bir geçersiz URL ve uygulamayı yeniden çalıştırın:

   **Objective-C**. QSTodoService.m içinde:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **SWIFT**. ToDoTableViewController.swift içinde:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Bazı Yapılacaklar öğelerini ekleyin. Simulator çıkın (veya zorla uygulamayı kapatmak) ve yeniden başlatın. Yaptığınız değişiklikleri kalıcı olduğunu doğrulayın.

3. Uzak içeriğini görüntülemek **Todoıtem** tablosu:
   * Node.js arka ucu için Git [Azure portal](https://portal.azure.com/) ve, mobil uygulama arka ucu içinde tıklatın **kolay tablolar** > **Todoıtem**.  
   * Bir .NET geri için sonlandırmak için SQL Server Management Studio gibi bir SQL aracı veya Fiddler veya Postman gibi bir REST istemcisi kullanın.  

4. Yeni öğelerin bulunduğunu doğrulayın *değil* edilmiş sunucuyla eşitlenir.

5. Doğru bir şekilde geri URL'sini değiştirmek **QSTodoService.m**ve uygulamayı yeniden çalıştırın.

6. Öğeleri listede aşağı çekerek yenileme hareketi gerçekleştirin.  
Devam eden değer değiştirici görüntülenir.

7. Görünüm **Todoıtem** yeniden verileri. Yeni ve değiştirilen Yapılacaklar öğelerini şimdi görüntülenmesi gerekir.

## <a name="summary"></a>Özet
Çevrimdışı eşitleme özelliğini desteklemek için kullandık `MSSyncTable` arabirim ve başlatıldı `MSClient.syncContext` yerel deposu. Bu durumda, Yerel Depodaki çekirdek veri tabanlı bir veritabanı oluştu.

Bir çekirdek veri yerel deposu kullandığınızda, birkaç tablolarla tanımlamalısınız [düzeltmek Sistem Özellikleri](#review-core-data).

Normal oluşturma, okuma, güncelleştirme ve uygulama hala bağlı, ancak yerel depo tüm işlemleri ortaya gibi mobil uygulamaları iş için (CRUD) işlemleridir silin.

Yerel depodan sunucuyla eşitlendiğinde kullandık **MSSyncTable.pullWithQuery** yöntemi.

## <a name="additional-resources"></a>Ek kaynaklar
* [mobil uygulamalarda çevrimdışı veri eşitlemeye]
* [Bulut kapsar: Azure Mobile Services'ın çevrimdışı eşitleme] \(video Mobile Services hakkında olmakla birlikte, benzer şekilde Mobile Apps çevrimdışı eşitleme çalışır.\)

<!-- URLs. -->


[iOS uygulaması oluştur]: app-service-mobile-ios-get-started.md
[mobil uygulamalarda çevrimdışı veri eşitlemeye]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Bulut kapsar: Azure Mobile Services'ın çevrimdışı eşitleme]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
