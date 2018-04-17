---
title: Azure Blob Depolama olaylarına tepki | Microsoft Docs
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
services: storage,event-grid
keywords: ''
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 2762466c0130ead36372a93f4c3b852cb378a02a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="reacting-to-blob-storage-events"></a>BLOB Depolama olaylarına tepki

Azure depolama olaylarında oluşturulması ve modern sunucusuz mimarileri kullanarak blob'lara silinmesini tepki vermek uygulamaların olanak tanır. Bunu karmaşık kodu veya verimsiz ve pahalı yoklama Hizmetleri gerek kalmadan yapar.  Bunun yerine, olayları gönderilen [Azure olay kılavuz](https://azure.microsoft.com/services/event-grid/) gibi abonelere [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi özel http dinleyicisi ve yalnızca Kullanım için ödeme yaparsınız. 

Resim veya video işleme, arama dizini oluşturma veya tüm dosya odaklı iş akışı ortak Blob Depolama olay senaryolar içerir.  Zaman uyumsuz dosya yüklemeleriyle olayları için harika bir Sığdırma ' dir.  Sık değişir, ancak hemen yanıtlama senaryonuz gerektirir, olay tabanlı mimari özellikle etkili olabilir.

Kullanılabilirlik depolama olayları için olay kılavuza bağlı [kullanılabilirlik](../../event-grid/overview.md) ve olay kılavuz yaptığı gibi diğer bölgelerde kullanılabilir hale gelecektir. Bir göz atalım [rota Blob Depolama olayları özel bir web uç noktası - CLI](storage-blob-event-quickstart.md) veya [rota Blob Depolama olayları özel bir web uç noktası - PowerShell](storage-blob-event-quickstart-powershell.md) kısa bir örnek için. 

![Olay kılavuz modeli](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Blob Storage hesapları
BLOB Depolama olayları kullanılabilir [Blob storage hesapları](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ve [genel amaçlı v2 depolama hesapları](../common/storage-account-options.md#general-purpose-v2). **Genel amaçlı v2 (GPv2)** BLOB'lar, dosyalar, kuyruklar ve tablolar dahil olmak üzere tüm depolama hizmetleri için tüm özellikleri destekleyen depolama hesaplarıdır. A **Blob storage hesabı** , yapılandırılmamış verilerinizi bloblar (nesneler) olarak Azure Storage depolamak için bir özel depolama hesabı. BLOB storage hesapları, genel amaçlı depolama hesapları gibi ve % 100 API tutarlığı dahil günümüzde blok bloblar için kullanılır ve ilave blobları tüm harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır. Yalnızca blok veya engelleme blobunun gerektiği uygulamalar için Blob Storage hesaplarının kullanılmasını öneririz. 

## <a name="available-blob-storage-events"></a>Kullanılabilir Blob Depolama olayları
Olay kılavuz kullanan [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için.  BLOB Depolama olay abonelikleri iki tür olay içerebilir:  

> |Olay adı|Açıklama|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Bir blob oluşturulduğunda veya aracılığıyla yerini harekete `PutBlob`, `PutBlockList`, veya `CopyBlob` işlemleri|
> |`Microsoft.Storage.BlobDeleted`|Bir blob üzerinden silindiğinde harekete bir `DeleteBlob` işlemi|

## <a name="event-schema"></a>Olay şeması
BLOB Depolama olayları, verilerde yapılan değişiklikler yanıtlamaları gereken tüm bilgileri içerir.  EventType özelliği "Microsoft.Storage" ile başladığından bir Blob Depolama olayı tanımlayabilirsiniz.  
Olay kılavuz olay özelliklerini kullanımı hakkında ek bilgi bölümlerinde [olay kılavuz olay şema](../../event-grid/event-schema.md).  

> |Özellik|Tür|Açıklama|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |Konu|string|Olay yayar depolama hesabı tam Azure Resource Manager kimliği.|
> |Konu|string|Depolama hesapları, hizmetleri ve kapsayıcıları için Azure RBAC açıklamak için kullanırız aynı Genişletilmiş Azure Resource Manager biçimini kullanarak konu olay nesne göreli kaynak yolu.  Bu biçim bir harf korumalıdır blob adı içerir.|
> |EventTime|string|Olay oluşturuldu, ISO 8601 biçiminde tarih/saat|
> |Olay türü|string|"Microsoft.Storage.BlobCreated" veya "Microsoft.Storage.BlobDeleted"|
> |Kimlik|string|Bu benzersiz tanımlayıcı olay|
> |dataVersion|string|Veri nesnesi şema sürümü.|
> |metadataVersion|string|Üst düzey özellikleri şema sürümü.|
> |veriler|object|Blob depolama özgü olay verilerini toplama|
> |data.contentType|string|Content-Type üstbilgisi blobundan döndürülür blob'u, içerik türü|
> |data.contentLength|number|Content-Length üstbilgisi blobundan döndürülür gibi bayt sayısını temsil eden bir tamsayı olduğu gibi blob'unun boyutu.  İle BlobCreated olay, ancak değil BlobDeleted gönderdi.|
> |Data.URL|string|Olay konusu nesnenin URL'si|
> |data.eTag|string|Bu olay şu olduğunda nesne etag.  BlobDeleted olay için kullanılamaz.|
> |Data.api|string|Bu olay tetiklenir API işlemi adıdır.  BlobCreated olaylar için bu değer "PutBlob", "PutBlockList" veya "CopyBlob" olur.  BlobDeleted olaylar için bu değer "DeleteBlob" olur.  Bu değerleri Azure Storage tanılama günlüklerine var olan aynı API adlardır.  Bkz: [işlemler ve durum iletilerini günlüğe](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.Sequencer|string|Herhangi bir belirli blob adını olayların mantıksal sırası temsil eden bir donuk dize değeri.  Kullanıcılar, iki olay aynı blob adı göreli dizisini anlamak için standart dize karşılaştırma kullanabilir.|
> |data.requestId|string|Depolama API işlemi için hizmeti oluşturulan isteği kimliği.  Azure tanılama günlüklerine "istek kimliği üstbilgisi" alanını kullanarak kaydeder ve yapma işlemi başlatmasını API çağrısı 'x-ms-request-id' başlığı döndürülür depolama ilişkilendirmek için kullanılabilir. Bkz: [oturum biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|string|Depolama API işlemi için sağlanan istemci isteği kimliği.  "X-ms-istemci-request-id" başlığı kullanarak istemci isteklerinde sağlanan ve Azure Storage tanılama günlüklerine günlükleri "client-request-id" alanında kullanarak ilişkilendirmek için kullanılır. Bkz: [oturum biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.storageDiagnostics|object|Bazen Azure depolama hizmeti tarafından eklenen tanılama verilerini.  Varsa, olay tüketicileri tarafından sayılır.|
|data.blobType|string|Blob türü. Geçerli değerler "BlockBlob" veya "PageBlob" dir.| 

BlobCreated olay bir örneği burada verilmiştir:
```json
[{
  "topic": "/subscriptions/319a9601-1ec0-0000-aebc-8fe82724c81e/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file1.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T01:57:26.005121Z",
  "id": "602a88ef-0001-00e6-1233-1646070610ea",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "799304a4-bbc5-45b6-9849-ec2c66be800a",
    "requestId": "602a88ef-0001-00e6-1233-164607000000",
    "eTag": "0x8D4E44A24ABE7F1",
    "contentType": "text/plain",
    "contentLength": 447,
    "blobType": "BlockBlob",
    "url": "https://myaccount.blob.core.windows.net/testcontainer/file1.txt",
    "sequencer": "00000000000000EB000000000000C65A",
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

Daha fazla bilgi için bkz: [Blob Depolama olayları şema](../../event-grid/event-schema-blob-storage.md).

## <a name="filtering-events"></a>Olayları filtreleme
BLOB olay abonelikleri olay türünü ve oluşturulan veya silinen nesnenin blob adını ve kapsayıcı adı tarafından göre filtre uygulanabilir.  Filtreler uygulanabilir olay abonelikleri ya da sırasında [oluşturma](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) olay aboneliğin veya [sonraki bir zamanda](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update). Konu filtrelere göre olay kılavuz iş "ile başlayan" ve "eşleşen bir konu olan olaylar için abone teslim edilir böylece ile eşleşirse, sona erer". 

Blob Depolama olayların konu biçimi kullanır:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Bir depolama hesabı için tüm olayları eşleştirmek için konu filtreleri boş bırakabilirsiniz.

Bir önek paylaşımı kapsayıcıları kümesinde oluşturulan BLOB'lar olaylarından eşleşecek şekilde kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containerprefix
```

Belirli kapsayıcısında oluşturulan BLOB'lar olaylarından eşleşecek şekilde kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/
```

Bir blob adı ön eki paylaşımı belirli kapsayıcısında oluşturulan BLOB'lar olaylarından eşleşecek şekilde kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Bir blob soneki paylaşımı belirli kapsayıcısında oluşturulan BLOB'lar olaylarından eşleşecek şekilde kullanmak bir `subjectEndsWith` ".log" veya ".jpg" gibi filtresi

Daha fazla bilgi için bkz: [olay kılavuz kavramları](../../event-grid/concepts.md#filters).

## <a name="practices-for-consuming-events"></a>Olayları tüketimi için uygulamalar
Blob Depolama olayları işlemek uygulamaları birkaç önerilen uygulamaları izlemelidir:
> [!div class="checklist"]
> * Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılabilir gibi olayları belirli bir kaynaktan varsayalım değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için ileti konusu denetlemek için önemlidir.
> * Benzer şekilde, eventType bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsayar değil olup olmadığını denetleyin.
> * Bazı gecikmeden sonra ve iletiler bozuk geldiğinde gibi nesneler hakkındaki bilgilerinizi hala güncel olup olmadığını anlamak için etag alanları kullanın.  Ayrıca, herhangi bir nesne üzerinde olayların sırası anlamak için sıralayıcı alanları kullanın.
> * Blob üzerindeki ne tür bir işlem izin verilen anlamak için blobType alanını kullanın ve hangi istemci kitaplığı türleri blob erişmek için kullanmanız gerekir. Geçerli değerler ya da `BlockBlob` veya `PageBlob`. 
> * Url alanla kullanmak `CloudBlockBlob` ve `CloudAppendBlob` blob erişmek için oluşturucular.
> * Anlamadığınız alanları yoksayın.  Bu yöntem, esnek gelecekte eklenebilecek yeni özelliklere tutmaya yardımcı olur.


## <a name="next-steps"></a>Sonraki adımlar

Olay kılavuz hakkında daha fazla bilgi ve Blob Depolama olayları bir deneyin:

- [Event Grid Hakkında](../../event-grid/overview.md)
- [BLOB Depolama olayları özel web uç noktasına rota](storage-blob-event-quickstart.md)
