---
title: Azure Cosmos DB'de çok yöneticili çakışma çözümü
description: Bu makalede, çakışma kategorileri ve çakışma çözüm modları gibi son yazıcı WINS (LWW), özel – kullanıcı tanımlı yordam, özel – Azure Comsos DB çok ana zaman uyumsuz açıklanır.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: b4433beea7fd9288c8c7be33c977ae11f3c2bac1
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426355"
---
# <a name="multi-master-conflict-resolution-in-azure-cosmos-db"></a>Azure Cosmos DB'de çok yöneticili çakışma çözümü 

Azure Cosmos DB çok yöneticili genel çakışma verilerin nerede çoğaltılmış tüm bölgeler arasında tutarlı olduğundan emin olmak için çözüm modları sağlar.

## <a name="conflict-categories"></a>Çakışma kategorileri

Azure Cosmos DB verilerle çalışırken oluşabilecek çakışmaları üç kategoriye ayrılır:

* **Çakışmaları Ekle:** uygulama ile aynı benzersiz dizin (örneğin, kimliği) iki veya daha fazla belge eklediğinde, bu tür bir çakışma iki veya daha fazla bölgelerden aynı anda olur. Bu durumda, tüm yazma işlemlerini başlangıçta başarısız olabilir, ancak seçtiğiniz çakışma çözüm İlkesi bağlı olarak, özgün Kimliğine sahip tek bir belge taahhüt eder.

* **Çakışmaları değiştirin:** uygulama tek bir belge iki veya daha fazla bölgede aynı anda güncelleştirir. Bu tür bir çakışma olur.

* **Çakışmaları Sil:** uygulama bir bölgeden bir belgeyi siler ve bir veya daha fazla bölgelerden güncelleştirmeleri bu tür bir çakışma olur. 

## <a name="conflict-resolution-modes"></a>Çakışma çözümleme modları

Azure Cosmos DB tarafından sunulan üç Çakışma çözümlemesi modu mevcuttur. Çakışma çözümleme modları her koleksiyon için tanımlanır.

> [!NOTE]
> SQL API'si kullanıcıların üç farklı çakışma çözümlemesi modları arasında seçim yapabilirsiniz. Tüm diğer API modelleri için (MongoDB, Cassandra, grafik ve tablo), son yazıcı WINS kullanarak çakışmaları çözümlenir.

### <a name="last-writer-wins-lww"></a>Son yazıcı WINS (LWW)

Son yazıcı WINS varsayılan çakışma çözüm modunda olur. Bu modda, belge üzerinde bir özelliği geçirilen sayısal bir değere göre çakışmaları çözüldü.

Aşağıdaki kod parçacığı, .net SDK'sı kullanılırken son yazıcı WINS çakışma çözüm ilkesi yapılandırma örneğidir. "ConflictResolutionPath" çakışmayı çözmek için kullanılan özellik yolunu tanımlar. Bu örnekte, `/userDefinedId` çakışma çözümleme yolu ve en büyük belgeyle `userDefinedId` değeri her zaman çakışma kazanın. Son yazıcı WINS çözüm modunda kaydetmek için aşağıda gösterildiği gibi koleksiyon ConflictResolutionPolicy sağlayın.

```csharp
DocumentCollection lwwCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
   Id = "myCollection", 
   ConflictResolutionPolicy = new ConflictResolutionPolicy
   {
      Mode = ConflictResolutionMode.LastWriterWins,
      ConflictResolutionPath = "/userDefinedId"
   }
});
```

 Son yazıcı WINS çözümü modu için farklı veri çakışması kategorileri şu şekilde uygulanır:

* **Ekleme ve güncelleştirme çakışmaları:** iki veya daha fazla belge birbiriyle çakışır Ekle veya Değiştir işlemleri, çakışma çözümleme yolu en büyük değeri içeren belge birinci (diğer bir deyişle, userDefinedId) olur. Birden çok belge çakışma çözümleme yolu için aynı sayısal değere sahip ise seçilen birinci belirleyici değildir. Ancak, tüm bölgeler için tek bir kazanan yakınsanır.

* **Çakışmayı silecek:** ilgili Sil çakışmalarından varsa, silme her zaman diğer Değiştir çakışmaları çakışma çözümleme yolu değerini bakılmaksızın üzerinden WINS.

### <a name="custom--user-defined-procedure"></a>Özel – kullanıcı tanımlı yordam

Bu modda, kullanıcı denetimleri, bir kullanıcı tanımlı yordam (UDP) koleksiyonuna kaydederek çözümleme çakışıyor. Bu UDP özgül bir imzası vardır. Bu seçeneği belirleyin, ancak bir UDP kaydı başarısız olursa veya UDP çalışma zamanında bir özel durum oluşturursa, çakışmaları nerede bunlar ayrı olarak çözülebilir akışı çakışmaları yazılır.

Özel bir – kullanıcı tanımlı yordam çakışma çözüm modunda kaydetmek için aşağıda gösterildiği gibi koleksiyon ConflictResolutionPolicy sağlayın.

```csharp
DocumentCollection udpCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
  Id = "myCollection",
  ConflictResolutionPolicy = new ConflictResolutionPolicy
  {
     Mode = ConflictResolutionMode.Custom,
     ConflictResolutionProcedure = UriFactory.CreateStoredProcedureUri("myDatabase","myCollection","myUdpStoredProcedure").ToString()
  }
});
```

Ardından, kullanıcı tanımlı yordamın, aşağıda gösterildiği gibi koleksiyona ekleyin.

```csharp
StoredProcedure myUdpStoredProc = new StoredProcedure
{
   Id = "myUdpStoredProcedure",
   Body = myUdpStoredProcText
};
await myClient.UpsertStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("myDatabase", "myCollection"), myUdpStoredProc);
```

Koleksiyonuna kayıtlı saklı yordamı bir özel imza içeriyor. Bu örnekte, bir özellik UDP kullanan `UserDefinedId` çakışmaları çözümlemek için. Çakışma belgenin en büyük değer kazanır.

```javascript
function myUdpStoredProcedure(incomingDocument, existingDocument, isDeleteConflict, conflictingDocuments){
    var collection = getContext().getCollection();

    if (!incomingDocument) {
        if (existingDocument) {

            collection.deleteDocument(existingDocument._self, {}, function(err, responseOptions) {
                if (err) throw err;
            });
        }
    } else if (isDeleteConflict) {
        // delete always wins.
    } else {
        var documentToUse = incomingDocument;

        if (existingDocument) {
            if (documentToUse.userDefinedId < existingDocument.userDefinedId) {
                documentToUse = existingDocument;
            }
        }

        var i;
        for (i = 0; i < conflictingDocuments.length; i++) {
            if (documentToUse.userDefinedId < conflictingDocuments[i].userDefinedId) {
                documentToUse = conflictingDocuments[i];
            }
        }

        tryDelete(conflictingDocuments, incomingDocument, existingDocument, documentToUse);
    }

    function tryDelete(documents, incoming, existing, documentToInsert) {
        if (documents.length > 0) {
            collection.deleteDocument(documents[0]._self, {}, function(err, responseOptions) {
                if (err) throw err;

                documents.shift();
                tryDelete(documents, incoming, existing, documentToInsert);
            });
        } else if (existing) {
            collection.replaceDocument(existing._self, documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        } else {
            collection.createDocument(collection.getSelfLink(), documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        }
    }
}

```

Yordamı, dört parametrelere sahiptir:

* **incomingDocument:** belge çakışan sürümünü belirtir. Çakışma, ekleme, değiştirme veya silme çakışması olabilir. Bir silme çakışması için bu içeriği olmayan bir delete görüntüsü (kaldırma) olacaktır.

* **existingDocument:** aynı belgenin işlenmiş sürüm "yazılımlar" incomingDocument değeri belirtir. INSERT null ve çakışma silmek için bu parametreyi ayarlayın.

* **isDeleteConflict:** gelen belgeyi daha önce silinmiş bir belge ile çakışan olup olmadığını belirten bir Boole bayrağı. Bu parametre için true, existingDocument ayarlandığında aynı zamanda bu kadar null olacaktır.

* **conflictingDocuments:** incomingDocument kimlik sütunu veya diğer benzersiz dizin alanları ile çakışan veritabanındaki tüm belgelerin işlenmiş sürüm koleksiyonunu belirtir. Bu belgeler için incomingDocument karşılaştırıldığında farklı "yazılımlar" değerine sahip olacaktır.

Kullanıcı tanımlı yordamın, Cosmos DB bölüm anahtarı tam erişimi olduğundan ve çakışmaları çözümlemek için tüm depolama işlemleri gerçekleştirebilir. Kullanıcı tanımlı yordamın çakışma sürümü kaydetmek değil, sistem çakışma kaldıracağız ve existingDocument kaydedilmiş kalır. Kullanıcı tanımlı yordamın başarısız olursa veya yok, Azure Cosmos DB tüm çakışma halinde burada da gösterildiği gibi zaman uyumsuz olarak işleyebileceğiniz akış salt okunur çakışmaları ekler [zaman uyumsuz Çakışma çözümlemesi modu](#custom--asynchronous). 

### <a name="custom--asynchronous"></a>Özel – zaman uyumsuz  

Bu modda, Azure Cosmos DB kaydedilmiş tüm çakışmalar (ekleme, değiştirme ve silme) dahil değildir ve kullanıcının uygulamayla ertelenmiş çözümlemesi için akış salt okunur çakışmaları kaydeder. Uygulama zaman uyumsuz olarak Çakışma çözümlemesi ve herhangi bir mantık kullanın veya bir dış kaynak, uygulama veya hizmet çakışmayı çözmek için bakın.

Özel bir – zaman uyumsuz çakışma çözüm modunda kaydetmek için aşağıda gösterildiği gibi koleksiyon ConflictResolutionPolicy sağlayın.

```csharp
DocumentCollection manualCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
    Id = "myCollection",
    ConflictResolutionPolicy = new ConflictResolutionPolicy
    {
        Mode = ConflictResolutionMode.Custom
    }
});
```

Okuma ve çakışmaları çakışmaları akıştaki işlemek için aşağıda gösterilen kodu uygulayın. Veri akışı çakışmaları içinde depolanan bazı depolama maliyeti ekler. Bu nedenle, bunlar işlendikten sonra akış çakışmalar depolanan verileri silmek için önerilir.

```csharp
FeedResponse<Conflict> response = await myClient.ReadConflictFeedAsync(myCollectionUri);
  foreach (Conflict conflict in response)
  {
      switch(conflict.OperationKind)
      {
         case OperationKind.Create:
         //Process Insert Conflicts.
         …
         case OperationKind.Replace:
         //Process Replace Conflicts
         …
         case OperationKind.Delete:
         //Process Delete Conflicts
         …
      }
  //Optionally delete the conflict after processed
  await myClient.DeleteConflictAsync(conflict.SelfLink);
  }
```

> [!NOTE]
> Çakışmaları akış değişiklik Cosmos DB'de akışı gibi aşağı akış işleme için bildirimleri göndermek için bir dinleyici içermez. Çakışmaları akış yoklama ve çakışmaları var olup olmadığını belirlemek için mantığı uygulaması gerekir.

## <a name="code-samples"></a>Kod örnekleri

Aşağıda listelenen API'ler için çakışma çözümü gösteren örnek uygulamalar mevcuttur. Her örnek, bir kapsayıcı içindeki çakışmaları oluşturur ve ardından her bir desteklenen Çakışma çözümlemesi modu için çakışmalarını gösterir.

|API modeli  | SDK |Örnek |
|---------|---------|---------|
|SQL API'Sİ    | .NET    |[Azure-cosmosdb-dotnet/samples/MultiMaster /](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/samples/MultiMaster)  |
|SQL API'Sİ    | Node    |[Azure-cosmos-js/samples/MultiRegionWrite /](https://github.com/Azure/azure-cosmos-js/tree/master/samples/MultiRegionWrite)  |
|SQL API'Sİ    | Java    |[Azure-cosmosdb-java-örnekler/src/main](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/main)  |
|MongoDB  | .NET    |[Azure-cosmos-DB-mongodb-dotnet-multi-Master](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-multi-master)   |
|Tablo API'si  | .NET    |[Azure-cosmos-DB-Table-dotnet-multi-Master](https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-multi-master)       |
|Gremlin API | .NET | [Azure-cosmos-DB-gremlin-dotnet-multi-Master](https://github.com/Azure-Samples/azure-cosmos-db-gremlin-dotnet-multi-master)|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Cosmos DB için çakışma kategorileri ve çakışma çözüm modları hakkında öğrendiniz. Ardından, aşağıdaki kaynaklara göz atın:

* [Çok yöneticili MongoDB istemcileri kullanın](multi-master-oss-nosql.md)
