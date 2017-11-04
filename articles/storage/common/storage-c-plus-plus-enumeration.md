---
title: "C++ için depolama istemci kitaplığı ile Azure Storage kaynaklarını listesi | Microsoft Docs"
description: "Kapsayıcı, BLOB, kuyruklar, tabloları ve varlıkları numaralandırmak için C++ için Microsoft Azure depolama istemci Kitaplığı'ndaki listenin API'leri kullanmayı öğrenin."
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: 9844412739f4f6f95416f81347f0f2eeeca62bea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="list-azure-storage-resources-in-c"></a>C++'ta Azure Storage kaynakları listeler
Liste, Azure Storage ile birçok geliştirme senaryolarını anahtarına işlemleridir. Bu makalede, Azure listenin C++ için Microsoft Azure depolama istemci Kitaplığı'nda sağlanan API'leri kullanarak depolama en verimli bir şekilde derlemesindeki açıklar.

> [!NOTE]
> Bu kılavuz hedefleyen C++ sürümü için Azure Storage istemci kitaplığı aracılığıyla kullanıma 2.x [NuGet](http://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

Depolama istemcisi kitaplığı çeşitli Azure Storage liste veya sorgu nesneleri için yöntemler sağlar. Bu makalede aşağıdaki senaryolar ele alır:

* Bir hesap listesi kapsayıcıları
* Liste BLOB bir kapsayıcı veya sanal blob dizini
* Bir hesap listesi kuyruklar
* Bir hesap listede tablolar
* Bir tablodaki sorgu varlıklar

Bu yöntemlerin her biri için farklı senaryolar farklı aşırı yüklemeleri kullanarak gösterilir.

## <a name="asynchronous-versus-synchronous"></a>Zaman uyumlu ve zaman uyumsuz
C++ için depolama istemci kitaplığı üzerine inşa edildiğinden [C++ REST Kitaplığı](https://github.com/Microsoft/cpprestsdk), kendiliğinden zaman uyumsuz işlemleri kullanarak destekliyoruz [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Örneğin:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Zaman uyumlu işlemler karşılık gelen zaman uyumsuz işlemleri kaydır:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Birden çok iş parçacıklı uygulamalar veya hizmetler ile çalışıyorsanız, zaman uyumsuz API'lerini doğrudan bir iş parçacığı oluşturma yerine eşitleme, performansı önemli ölçüde etkiler API'leri çağırmak için kullanmanızı öneririz.

## <a name="segmented-listing"></a>Bölümlenmiş listeleme
Bulut depolama ölçeğini bölümlenmiş listeleme gerektirir. Örneğin, bir Azure blob kapsayıcısındaki bir milyon BLOB veya bir Azure tablosu bir milyar varlıklarda üzerinden olabilir. Bunlar teorik numaraları, ancak gerçek müşteri kullanım durumları olup olmadığı.

Bu nedenle, tek bir yanıt tüm nesneleri listelemek için zordur. Bunun yerine, disk belleği kullanarak nesneleri listeleyebilirsiniz. Her API listeleme sahip bir *kesimli* aşırı yükleme.

Bölümlenmiş listeleme işlemi yanıtı içerir:

* <i>_segment</i>, tek bir çağrı listeleme API için döndürülen sonuç kümesi içerir.
* *continuation_token*, sonraki sonuç sayfasını alabilmek için sonraki çağrı geçirildi. Devamlılık belirteci dönmek için daha fazla sonuç olduğunda null olur.

Örneğin, bir kapsayıcıdaki tüm blobları listelemek için tipik bir çağrı aşağıdaki kod parçacığını gibi görünebilir. Kod kullanılabilir bizim [örnekleri](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

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
        process_diretory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

Bir sayfa döndürülen sonuç sayısı parametresiyle denetlenebilir Not *max_results* örneğin her API yüklemesini içinde:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Belirtmezseniz, *max_results* parametresi, varsayılan en büyük değer en fazla 5000 sonuçlarını tek bir sayfayla döndürülür.

Ayrıca, Azure Table storage bir sorgu kayıt yok veya değerinden daha az sayıda kayıt döndürebilir unutmayın *max_results* devamlılık belirteci boş olsa bile, belirtilen parametre. Nedenlerinden biri, sorgu beş saniye içinde tamamlanamadı olabilir. Devamlılık belirteci boş değil sürece, sorgu devam etmesi gerektiğini ve kodunuzu segment sonuçları boyutunu varsayımında bulunmamalıdır.

Çoğu senaryo için önerilen kodlama düzeni listeleme, liste veya sorgulama ve hizmetin her istek nasıl yanıt vereceğini açık ilerlemesini sağlayan kesimli. Özellikle C++ uygulamalar veya hizmetler için alt düzey denetim listesi ilerleme denetim bellek ve performans yardımcı olabilir.

## <a name="greedy-listing"></a>Doyumsuz listeleme
C++ için depolama istemci Kitaplığı'nın önceki sürümlerini (sürümleri 0.5.0 Önizleme ve önceki sürümler) bölümlenmiş listesine API'leri tablolar ve Kuyruklar, aşağıdaki örnekte olduğu gibi dahil:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Bu yöntemleri bölümlenmiş API'leri sarmalayıcılar olarak oluşturulmuştur. Her yanıtı bölümlenmiş dökümün için kod sonuçları bir vektörüne eklenen ve tam kapsayıcıları taranan sonra tüm sonuç döndürmedi.

Az sayıda nesne depolama hesabı veya tablo içerdiğinde, bu yaklaşım çalışabilir. Ancak, tüm sonuçları bellekte kalan çünkü bir artış ile nesnelerin sayısı, gerekli bellek sınırı arttırabilir. Bir listeleme işlemi sırasında ilerleme durumunu hakkında hiçbir bilgi arayan sahip bir çok uzun zaman alabilir.

Bu doyumsuz listeleme SDK API'leri C#, Java veya JavaScript Node.js ortamında mevcut değil. Doyumsuz bu API'leri kullanarak olası sorunları önlemek için biz bunları 0.6.0 sürümde kaldırılan Önizleme.

Kodunuzu bu doyumsuz API çağırıyorsa:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Ardından kodunuzu bölümlenmiş listenin API'leri kullanmak için değiştirmeniz gerekir:

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

Belirterek *max_results* parametresi kesiminin, Bakiye isteklerinin sayısı ve uygulamanız için başarım düşünceleri karşılamak için bellek kullanımını arasında.

Bölümlenmiş listeleme API'leri kullanıyorsanız, ancak bir "Hızlı" stilde yerel bir koleksiyondaki verileri depolamak, ayrıca, aynı zamanda dikkatle ölçekte yerel bir koleksiyondaki veri depolama işlemek için kodunuzu yeniden düzenlemeniz öneririz.

## <a name="lazy-listing"></a>Yavaş listeleme
Doyumsuz listeleme olası sorunları ortaya kapsayıcısında çok fazla nesne değilse bu kullanışlı olsa da.

C# veya Oracle Java SDK'ları da kullanıyorsanız, bir yavaş, gerekirse burada belirli uzaklığındaki verileri yalnızca alınmadığı listeleme stili sunan numaralandırılabilir programlama modeli, tanımanız gerekir. C++'da, yineleyici tabanlı şablonunu de benzer bir yaklaşım sağlar.

Tipik yavaş listeleme API'sini kullanarak **list_blobs** örnek olarak, şöyle görünür:

```cpp
list_blob_item_iterator list_blobs() const;
```

Yavaş listeleme desen kullanan bir tipik kod parçacığını şuna benzeyebilir:

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

Yavaş listeleme yalnızca zaman uyumlu modda kullanılabilir olduğunu unutmayın.

Doyumsuz listesi ile karşılaştırıldığında, yavaş listeleme verileri yalnızca gerekli olduğunda getirir. Yalnızca sonraki yineleyici sonraki segmentin taşındığında perde arkasında, verileri Azure depolama biriminden getirir. Bu nedenle, bellek kullanımı ile sınırlanmış bir boyutu denetlenir ve hızlı bir işlemdir.

Yavaş listeleme API'leri dahil edilir depolama istemci Kitaplığı'nda C++ için sürüm 2.2.0.

## <a name="conclusion"></a>Sonuç
Bu makalede, C++ için depolama istemci Kitaplığı'nda çeşitli nesneler için API listeleme için farklı aşırı açıklanmıştır. Özetlemek için:

* Zaman uyumsuz API'leri birden çok iş parçacığı senaryolarda önerilir.
* Bölümlenmiş listeleme çoğu senaryolar için önerilir.
* Yavaş listeleme kullanışlı bir sarmalayıcı zaman uyumlu senaryolarda olarak kitaplıkta sağlanır.
* Doyumsuz listeleme tavsiye edilmez ve Kitaplığı'ndan kaldırıldı.

## <a name="next-steps"></a>Sonraki adımlar
C++ için Azure Storage istemci Kitaplığı hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [C++ içinden BLOB Storage kullanma](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Tablo depolama C++ içinden kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [C++ içinden kuyruk depolama kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [C++ API belgeleri için Azure Storage istemci kitaplığı.](http://azure.github.io/azure-storage-cpp/)
* [Azure Depolama Ekibi Blog’u](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)

