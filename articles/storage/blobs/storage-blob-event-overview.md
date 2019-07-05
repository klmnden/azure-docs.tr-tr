---
title: Azure Blob Depolama olaylarına tepki | Microsoft Docs
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
services: storage,event-grid
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: c0655d02fd5d0d64c22db286236b2a26f9e70619
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444675"
---
# <a name="reacting-to-blob-storage-events"></a>BLOB Depolama olaylarına tepki verme

Azure Depolama olaylarını uygulamaları modern sunucusuz mimarileri kullanarak oluşturma ve blobları silme gibi olaylarına tepki verin. Bunu karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan yapar.

Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) aboneleri gibi Azure işlevleri, Azure Logic Apps ve hatta kendi özel http dinleyicisi ve yalnızca kullandığınız kadarı için ödeme yaparsınız.

BLOB Depolama olaylarını güvenilir bir şekilde zengin yeniden deneme ilkelerini ve teslim edilemeyen bir deneyimle uygulamalarınızda güvenilir teslim hizmetleri sağlayan Event Grid hizmetine gönderilir.

Ortak Blob Depolama olayı senaryolar resim veya video işleme, arama dizini oluşturma veya tüm dosya odaklı iş akışı içerir. Zaman uyumsuz dosya yüklemeleri olayları için harika bir uygun olan. Seyrek görülen değişiklikler, ancak senaryonuza anında yanıt verme hızını gerektirir, olay tabanlı mimari özellikle etkili olabilir.

Bunu şimdi denemek istiyorsanız, bu hızlı başlangıçta makalelerden herhangi birine bakın:

|Bu aracı kullanmak istiyorsanız:    |Bu makaleye bakın: |
|--|-|
|Azure Portal    |[Hızlı Başlangıç: Azure portalı ile web uç noktası için BLOB Depolama olaylarını yönlendirme](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[Hızlı Başlangıç: PowerShell ile web uç noktası için Depolama olaylarını yönlendirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Hızlı Başlangıç: Azure CLI ile web uç noktası için Depolama olaylarını yönlendirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="the-event-model"></a>Olay modeli

Event grid'i kullanır [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Bu görüntü, olay yayımcıları ve olay abonelikleri olay işleyicileri arasındaki ilişkiyi gösterir.

![Event Grid modeli](./media/storage-blob-event-overview/event-grid-functional-model.png)

İlk olarak, bir uç nokta bir olaya abone olun. Ardından, bir olayı tetiklendiğinde, Event Grid hizmet uç noktasına bu olay hakkında veri gönderir.

Bkz: [Blob Depolama olayları şema](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) makale görüntülemek için:

> [!div class="checklist"]
> * Blob Depolama olaylarını ve nasıl her olayın tetiklendiği tam listesi.
> * Event Grid veri örneği, bu olayların her biri için gönderir.
> * Verilerin görünen her anahtar değer çifti amacı.

## <a name="filtering-events"></a>Olayları filtreleme

BLOB olay abonelikleri olayın türüne ve kapsayıcı adı ve blob adı, oluşturulduğu veya silindiği nesne tarafından göre filtrelenebilir.  Filtre uygulanabilir olay abonelikleri ya da sırasında [oluşturma](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) olay aboneliğinin veya [sonraki bir zamanda](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest). Konu filtreleri temel Event Grid çalışma "ile başlayan" ve "böylece abone ile eşleşen bir konu ile olayları teslim ile eşleşir, ends".

Uygulama Filtreleri hakkında daha fazla bilgi için bkz. [olayları Event Grid için filtre](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

Blob Depolama olaylarını konusu biçimini kullanır:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Bir depolama hesabı için tüm olaylara eşleştirmek için boş konu filtreleri bırakabilirsiniz.

BLOB kapsayıcıları bir ön eki paylaşımı kümesi içinde oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containerprefix
```

Belirli bir kapsayıcıda oluşturulan BLOB'ları olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/
```

BLOB'ları belirli kapsayıcısına bir blob adı ön eki paylaşımı oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectBeginsWith` gibi Filtrele:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

BLOB'ları belirli kapsayıcısına bir blob soneki paylaşımı oluşturulan olaylardan eşleştirmek için kullanmak bir `subjectEndsWith` Filtresi ".log" veya ".jpg" gibi. Daha fazla bilgi için [Event Grid kavramları](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Olayları kullanan uygulamalar

Blob Depolama olayları işleyen uygulamaları birkaç önerilen uygulamaları izlemelisiniz:
> [!div class="checklist"]
> * Birden çok abonelik aynı olay işleyicisi için rota olayları yapılandırılması gibi belirli bir kaynaktan gelen olayları olduğu varsayılır değil ancak beklediğiniz depolama hesabından geldiğinden emin olmak için iletisinin konu denetlemek için önemlidir.
> * Benzer şekilde, eventType tek bir işlem için hazır ve aldığınız tüm olayları beklediğiniz türleri olacağını varsaymayın olup olmadığını denetleyin.
> * Bazı gecikmeden sonra ve iletileri sıralamaya gelen nesnelerle ilgili bilgilerinizi hala güncel olup olmadığını anlamak için etag alanları kullanın.  Ayrıca, herhangi bir nesne üzerinde olayların sırası anlamak için sıralayıcı'yı alanlarını kullanın.
> * Ne tür işlem bir bloba izin anlamak için blobType alanı kullanın ve hangi istemci kitaplığı, türleri bloba erişmek için kullanmanız gerekir. Geçerli değerler ya da `BlockBlob` veya `PageBlob`. 
> * Url alanla kullanmak `CloudBlockBlob` ve `CloudAppendBlob` bloba erişmek için oluşturucu.
> * Alanları anlamadığınız yoksayın. Bu yöntem, dayanıklı, gelecekte eklenebilir yeni özellikler kalmasına yardımcı olur.
> * Emin olmak istiyorsanız **Microsoft.Storage.BlobCreated** yalnızca bir blok blobu tamamen tamamlandığında olay tetiklenir, olay için filtre `CopyBlob`, `PutBlob`, `PutBlockList` veya `FlushWithClose` REST API çağrıları. Şu API çağrıları tetikleyici **Microsoft.Storage.BlobCreated** yalnızca veri bir blok Blobuna tam olarak kaydedildikten sonra olay. Filtre oluşturma konusunda bilgi almak için bkz: [olayları Event Grid için filtre](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).


## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinmek ve Blob Depolama olaylarını deneyin:

- [Event Grid Hakkında](../../event-grid/overview.md)
- [BLOB Depolama olaylarını bir özel web uç noktasına yönlendirme](storage-blob-event-quickstart.md)
