---
title: Azure Event Grid blob depolama olay şeması
description: Blob depolama olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: bed6c3f1efcb2d0ef34e827ddb2b521f8c038940
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445776"
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Blob Depolama için Azure Event Grid olay şeması

Bu makale, blob depolama olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Örnek betikler ve öğreticiler listesi için bkz: [depolama olay kaynağı](event-sources.md#storage).

## <a name="list-of-events-for-blob-rest-apis"></a>Olayların bir listesi için Blob REST API'leri

Bir istemci oluşturur, değiştirir veya Blob REST API'lerini çağırarak bir blobu siler. Bu olay tetiklenir.

 |Olay adı |Açıklama|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** |Bir blob oluşturulan veya değiştirilen tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `PutBlob`, `PutBlockList`, veya `CopyBlob` Blob REST API'SİNDE kullanılabilen işlemleri.   |
 |**Microsoft.Storage.BlobDeleted** |Bir blob silindiğinde tetiklenir. <br>Özellikle, istemcilerin çağırdığınızda bu olay tetiklenir `DeleteBlob` Blob REST API'SİNDE kullanılabilir olan işlem. |

> [!NOTE]
> Emin olmak istiyorsanız **Microsoft.Storage.BlobCreated** yalnızca bir blok blobu tamamen tamamlandığında olay tetiklenir, olay için filtre `CopyBlob`, `PutBlob`, ve `PutBlockList` REST API'sini çağırır. Şu API çağrıları tetikleyici **Microsoft.Storage.BlobCreated** yalnızca veri bir blok Blobuna tam olarak kaydedildikten sonra olay. Filtre oluşturma konusunda bilgi almak için bkz: [olayları Event Grid için filtre](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

## <a name="list-of-the-events-for-azure-data-lake-storage-gen-2-rest-apis"></a>Olayların bir listesi için Azure Data Lake depolama Gen 2 REST API'leri

Bu olayları bir hiyerarşik ad alanı depolama hesabındaki etkinleştirirseniz ve istemcilerin Azure Data Lake depolama Gen2 REST API'lerini çağırma tetiklenir.

> [!NOTE]
> Bu olaylar, genel Önizleme aşamasındadır ve yalnızca kullanımına açıktır **Batı ABD 2** ve **Batı Orta ABD** bölgeleri.

 |Olay adı|Açıklama|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** | Bir blob oluşturulan veya değiştirilen tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `CreateFile` ve `FlushWithClose` Azure Data Lake depolama Gen2 REST API'SİNDE kullanılabilen işlemleri. |
 |**Microsoft.Storage.BlobDeleted** |Bir blob silindiğinde tetiklenir. <br>İstemciler çağırdığınızda özellikle, bu olay de tetiklenir `DeleteFile` Azure Data Lake depolama Gen2 REST API kullanılabilir olan işlem. |
 |**Microsoft.Storage.BlobRenamed**|Bir blob yeniden adlandırıldığında tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `RenameFile` Azure Data Lake depolama Gen2 REST API kullanılabilir olan işlem.|
 |**Microsoft.Storage.DirectoryCreated**|Bir dizin oluşturulduğunda tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `CreateDirectory` Azure Data Lake depolama Gen2 REST API kullanılabilir olan işlem.|
 |**Microsoft.Storage.DirectoryRenamed**|Bir dizini yeniden adlandırıldığında tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `RenameDirectory` Azure Data Lake depolama Gen2 REST API kullanılabilir olan işlem.|
 |**Microsoft.Storage.DirectoryDeleted**|Bir dizin silindiğinde tetiklenir. <br>Özellikle, istemciler kullandığınızda bu olay tetiklenir `DeleteDirectory` Azure Data Lake depolama Gen2 REST API kullanılabilir olan işlem.|

> [!NOTE]
> Emin olmak istiyorsanız **Microsoft.Storage.BlobCreated** yalnızca bir blok blobu tamamen tamamlandığında olay tetiklenir, olay için filtre `FlushWithClose` REST API çağrısı. Bu API çağrısı Tetikleyicileri **Microsoft.Storage.BlobCreated** yalnızca veri bir blok Blobuna tam olarak kaydedildikten sonra olay. Filtre oluşturma konusunda bilgi almak için bkz: [olayları Event Grid için filtre](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

<a id="example-event" />

## <a name="the-contents-of-an-event-response"></a>Bir olay yanıtının içeriği

Olay tetiklendiğinde Event Grid hizmeti bu olay hakkında veri abone uç noktasına gönderir.

Bu bölüm, bu verileri her blob depolama olayı nasıl gibidir örneği içerir.

### <a name="microsoftstorageblobcreated-event"></a>Microsoft.Storage.BlobCreated olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobCreated olay (Data Lake depolama Gen2)

Blob depolama hesabına bir hiyerarşik ad alanı varsa, verileri bu dışında önceki örneğe benzer değişiklikler:

* `dataVersion` Anahtar değerine ayarlanmış `2`.

* `data.api` Anahtarı dizeye ayarlanır `CreateFile` veya `FlushWithClose`.

* `contentOffset` Anahtarı veri kümesinde yer almaktadır.

> [!NOTE]
> Uygulamaları kullanıyorsanız `PutBlockList` hesabına, verileri yeni bir blob yüklemek için işlem, bu değişiklikleri içermesi gerekmez.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Microsoft.Storage.BlobDeleted olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobDeleted olay (Data Lake depolama Gen2)

Blob depolama hesabına bir hiyerarşik ad alanı varsa, verileri bu dışında önceki örneğe benzer değişiklikler:

* `dataVersion` Anahtar değerine ayarlanmış `2`.

* `data.api` Anahtarı dizeye ayarlanır `DeleteFile`.

* `url` Anahtarı içeren yolun `dfs.core.windows.net`.

> [!NOTE]
> Uygulamaları kullanıyorsanız `DeleteBlob` hesaptan, veriler bir blob silme işlemi, bu değişiklikleri içeren olmaz.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobrenamed-event"></a>Microsoft.Storage.BlobRenamed olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "eventType": "Microsoft.Storage.BlobRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Microsoft.Storage.DirectoryCreated olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "eventType": "Microsoft.Storage.DirectoryCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Microsoft.Storage.DirectoryRenamed olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "eventType": "Microsoft.Storage.DirectoryRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Microsoft.Storage.DirectoryDeleted olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "eventType": "Microsoft.Storage.DirectoryDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | BLOB Depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| api | string | Olayı tetikleyen işlemi. |
| clientRequestId | string | sağlanan istemci İstek Kimliği ' % s'depolama API işlemi için. Bu kimlik günlüklerinde "client-request-id" alanını kullanarak Azure depolama tanılama günlüklerine ilişkilendirmek için kullanılabilir ve istemci isteklerinde "x-ms-istemci-request-id" üst bilgisi kullanılarak sağlanabilir. Bkz: [günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| requestId | string | Depolama API işlemi için istek hizmet tarafından oluşturulan kimliği. Azure tanılama günlüklerinde "istek kimliği üstbilgisi" alanını kullanarak kaydeder ve başlatmasını API çağrısı 'x-ms-isteği-id' üst bilgisinde döndürülen depolama ilişkilendirmek için kullanılabilir. Bkz: [günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| eTag | string | Koşullu işlemleri gerçekleştirmek için kullanabileceğiniz bir değer. |
| contentType | string | Blob'u için belirtilen içerik türü. |
| contentLength | integer | Blob bayt cinsinden boyutu. |
| blobType | string | Blob türü. Geçerli değerler "BlockBlob" veya "PageBlob" olmalı. |
| contentOffset | number | Bir yazma işlemi olarak olay tetiklemeyi uygulama dosyasını yazmayarak burada tamamlandı noktasında alınan bayt uzaklığı. <br>Yalnızca hiyerarşik ad alanı olan blob depolama hesapları üzerinde harekete geçirilen olayları için görünür.|
| destinationUrl |string | İşlem tamamlandıktan sonra mevcut dosyanın URL'si. Örneğin, bir dosyayı yeniden adlandırılırsa `destinationUrl` özelliği, yeni dosya adı URL'sini içerir. <br>Yalnızca hiyerarşik ad alanı olan blob depolama hesapları üzerinde harekete geçirilen olayları için görünür.|
| sourceUrl |string | İşlemi önce var olan dosyanın URL'si. Örneğin, bir dosyayı yeniden adlandırılırsa `sourceUrl` özgün dosya adını yeniden adlandırma işlemi öncesinde URL'sini içerir. <br>Yalnızca hiyerarşik ad alanı olan blob depolama hesapları üzerinde harekete geçirilen olayları için görünür. |
| url | string | Blob yolu. <br>Bir Blob REST API istemcisi kullanır ardından url bu yapıya sahiptir:  *\<depolama hesabı adı\>.blob.core.windows.net/\<kapsayıcı adı\>/\<dosya adı \>* . <br>Bir Data Lake depolama REST API istemcisi kullanır ardından url bu yapıya sahiptir:  *\<depolama hesabı adı\>.dfs.core.windows.net/\<dosya sistemi adı\> / \<dosya adı\>* .
|
| recursive| string| `True` tüm alt dizinler işlemi gerçekleştirmek için; Aksi takdirde `False`. <br>Yalnızca hiyerarşik ad alanı olan blob depolama hesapları üzerinde harekete geçirilen olayları için görünür. |
| sequencer | string | Herhangi bir belirli blob adı için olayların mantıksal sırasını temsil eden bir donuk bir dize değeri.  Kullanıcılar, standart dize karşılaştırması, aynı blob adı iki olayların göreli sırasını anlamak için kullanabilirsiniz. |
| storageDiagnostics | object | Bazen Azure depolama hizmeti tarafından bulunan Tanılama verileri. Mevcut olduğunda olay tüketicileri tarafından yoksayılacak. |

|Özellik|Tür|Açıklama|
 |-------------------|------------------------|-----------------------------------------------------------------------|

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
* Blob Depolama olaylarını çalışmak için bir giriş için bkz [rota Blob Depolama olayları - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
