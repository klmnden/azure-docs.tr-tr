---
title: C++ için depolama istemcisi kitaplığı ile Azure depolama kaynaklarını liste | Microsoft Docs
description: Listenin API'leri C++ için Microsoft Azure depolama istemci kitaplığı kapsayıcıları, bloblar, kuyruklar, tablolar ve varlıklar numaralandırmak için nasıl kullanılacağını öğrenin.
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: mhopkins
ms.reviewer: dineshm
ms.subservice: common
ms.openlocfilehash: edf50b97ff25a67b41bad266df9236145f288409
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146872"
---
# <a name="list-azure-storage-resources-in-c"></a>Azure depolama kaynaklarını C++ dilinde listesi
Liste, Azure depolama ile birçok geliştirme senaryoları için anahtar işlemlerdir. Bu makalede, en verimli şekilde listenin C++ için Microsoft Azure depolama istemci Kitaplığı'ndaki sağlanan API'leri kullanarak Azure Depolama'daki nesnelerini listeleme açıklar.

> [!NOTE]
> Bu kılavuzda hedefleyen C++ sürümü için Azure depolama istemci kitaplığı aracılığıyla kullanılabilir olan 2.x [NuGet](https://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

Depolama istemcisi kitaplığı çeşitli Azure storage'da listesi veya sorgusuna nesnelere yöntemler sağlar. Bu makalede aşağıdaki senaryoları ele alır:

* Bir hesabındaki kapsayıcıları listesi
* Bir kapsayıcı veya blob sanal dizin içindeki blobları listeleme
* Bir hesapta kuyrukları listeleme
* Bir hesapta listede tablolar
* Bir tablodaki varlıkları sorgulayın

Bu yöntemlerin her biri farklı aşırı yüklemeler farklı senaryolar için kullanma gösterilmektedir.

## <a name="asynchronous-versus-synchronous"></a>Zaman uyumlu ve zaman uyumsuz
C++ için depolama istemci kitaplığı üzerine inşa edildiğinden [C++ REST Kitaplığı](https://github.com/Microsoft/cpprestsdk), kendiliğinden zaman uyumsuz işlemleri kullanarak destekliyoruz [pplx::task](https://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Örneğin:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Zaman uyumlu işlemler karşılık gelen zaman uyumsuz işlemler kaydır:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Birden çok iş parçacıklı uygulamalar veya hizmetler ile çalışıyorsanız, zaman uyumsuz API'leri doğrudan bir iş parçacığı oluşturmak yerine eşitleme performansınızı önemli ölçüde etkiler API'leri çağırmak için kullanmanızı öneririz.

## <a name="segmented-listing"></a>Bölümlenmiş listeleme
Bulut depolama ölçek segmentli listeleme gerektirir. Örneğin, bir Azure blob kapsayıcısındaki bloblar bir milyon veya bir Azure tablosu bir milyardan fazla varlıklarda üzerinden olabilir. Bunlar teorik sayılar, ancak gerçek müşteri kullanım durumları değildir.

Bu nedenle, tek bir yanıt tüm nesneleri listelemek için zordur. Bunun yerine, disk belleği kullanarak nesneleri listeleyebilirsiniz. Her biri listeyi API'leri bir *bölümlenmiş* aşırı yükleme.

Bölümlenmiş listeleme işlemi için yanıt içerir:

* <i>_segment</i>, tek bir çağrı listesi API'si için döndürülen sonuç kümesini içerir.
* *continuation_token*, sonraki sonuç sayfasını almak için sonraki çağrı geçirilir. Daha fazla sonuç döndürmek için olduğunda, devamlılık belirteci null olur.

Örneğin, bir kapsayıcıdaki tüm blobları listelemek için tipik bir çağrı, aşağıdaki kod parçacığı gibi görünebilir. Kodu bizim [örnekleri](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

```cpp
// List blobs in the blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

Bir sayfa döndürülen sonuç sayısı parametresi tarafından denetlenebilir Not *max_results* örneğin her API aşırı yüklemesini içinde:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Siz belirtmezseniz *max_results* parametre, varsayılan en büyük değer en fazla 5000 sonuçlarının tek sayfa olarak döndürülür.

Ayrıca Azure tablo Depolama'yı kullanan bir sorgu kayıt yok veya değerinden daha az sayıda kayıt döndürebilir unutmayın *max_results* devamlılık belirteci boş olsa bile, belirttiğiniz parametresi. Nedenlerinden biri, sorgu beş saniye içinde tamamlanamadı olabilir. Devamlılık belirteci boş değil sürece, sorgu devam etmelidir ve kodunuzu segment sonuçları boyutunu varsayımında bulunmamalıdır.

Çoğu senaryo için önerilen kodlama düzeni listelemek, liste veya sorgulama ve hizmet her isteğin nasıl yanıt vereceğini açık ilerlemesini sağlayan bölümlenmiş. Özellikle C++ uygulamaları veya hizmetleri için alt düzey denetim listesi ilerleme denetimi bellek ve performans yardımcı olabilir.

## <a name="greedy-listing"></a>Doyumsuz listeleme
C++ için depolama istemci Kitaplığı'nın önceki sürümlerini (Önizleme sürümleri buradan 0.5.0 sürümünü ve önceki sürümleri) listesi API'leri için tablolar ve Kuyruklar, aşağıdaki örnekte olduğu gibi segmentlere ayrılmamış dahil:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Bu yöntemler bölümlenmiş API'lerinin sarmalayıcıları uygulandığına. Her yanıt segmentli dökümün için kod sonuçları bir vektör kayıtlarına eklenir ve sonra tam kapsayıcıları taranan tüm sonuç döndürmedi.

Bu yaklaşım, tablo ve depolama hesabı nesneleri az sayıda içerdiğinde işe yarayabilir. Ancak, tüm sonuçlar bellekte kalan çünkü nesnelerin sayısında bir artış ile gerekli bellek sınırı arttırabilir. Bir listeleme işlemi boyunca ilerleme durumunu hakkında hiçbir bilgi arayan vardı çok uzun zaman alabilir.

Bu SDK'sı API'leri doyumsuz listeleme C#, Java veya JavaScript Node.js ortamında mevcut değil. Doyumsuz bu API'leri kullanarak olası sorunları önlemek için bunları 0.6.0 sürümü kaldırdık Önizleme.

Kodunuzu bu doyumsuz API'ları çağırıyorsa:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Bölümlenmiş listenin API'leri kullanmak için kodunuzu değiştirmeniz gerekir:

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

Belirterek *max_results* parametresi kesiminin, Bakiye isteklerinin sayısı ve uygulamanız için performans konuları karşılamak için bellek kullanımını arasında.

Bölümlenmiş listeleme API kullanıyorsanız, ancak "greedy" stil yerel bir koleksiyondaki verileri depolamak, ayrıca, ayrıca dikkatli bir şekilde uygun ölçekte yerel bir koleksiyondaki verileri depolama işlemek için kodunuzu yeniden düzenleyin öneririz.

## <a name="lazy-listing"></a>Yavaş listeleme
Doyumsuz listeleme olası sorunları ortaya olsa da, kapsayıcıda çok fazla nesne yoksa, kullanışlıdır.

Ayrıca C# veya Oracle Java SDK'ları kullanıyorsanız, bir yavaş, gerekirse burada verileri belirli bir uzaklıkta yalnızca getirildiğini listeleme stili sunan numaralandırılabilir programlama modeli ile ilgili bilgi sahibi olması gerekir. C++'da, yineleyici tabanlı şablon da benzer bir yaklaşım sağlar.

Tipik yavaş listeleme API'sini kullanarak **list_blobs** örnek olarak, şöyle görünür:

```cpp
list_blob_item_iterator list_blobs() const;
```

Yavaş listeleme desen kullanan bir tipik kod parçacığı şuna benzeyebilir:

```cpp
// List blobs in the blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

Yavaş listesi yalnızca zaman uyumlu modda kullanılabilir olduğunu unutmayın.

Doyumsuz listeleme ile karşılaştırıldığında, yavaş listesi yalnızca gerektiğinde verileri getirir. Yalnızca sonraki yineleyiciye sonraki kesime hareket ettiğinde, perde, verileri Azure depolama biriminden getirir. Bu nedenle, bellek kullanımı sınırlanmış bir boyutu ile denetlenir ve hızlı bir işlemdir.

Yavaş listeleme API'leri dahil edilecek depolama istemci Kitaplığı'ndaki için C++ 2.2.0 sürümde.

## <a name="conclusion"></a>Sonuç
Bu makalede, C++ için depolama istemcisi kitaplığı çeşitli nesneleri için API'leri listeleme farklı aşırı yüklemeler ele almıştık. Özetlersek:

* Zaman uyumsuz API'ları birden çok iş parçacıklı senaryoları altında önerilir.
* Bölümlenmiş listeleme çoğu senaryo için önerilir.
* Yavaş listesi kitaplıkta zaman uyumlu senaryolarda uygun bir kapsayıcı olarak sağlanır.
* Doyumsuz listeleme önerilmez ve Kitaplığı'ndan kaldırıldı.

## <a name="next-steps"></a>Sonraki adımlar
C++ için Azure depolama ve istemci Kitaplığı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [BLOB depolama alanından C++ kullanma](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Tablo depolama'yı C++ kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Kuyruk Depolama'yı C++ kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [C++ API belgeleri için Azure depolama istemci kitaplığı.](https://azure.github.io/azure-storage-cpp/)
* [Azure Depolama Ekibi Blog’u](https://blogs.msdn.com/b/windowsazurestorage/)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)

