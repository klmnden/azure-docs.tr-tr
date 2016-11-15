---
title: "REST API kullanarak Azure Search&quot;te karşıya veri yükleme | Microsoft Belgeleri"
description: "REST API kullanarak Azure Search&quot;te bir dizine nasıl veri yükleneceğini öğrenin."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 6ff31940f3a4e7557e0caf3d9d3740590be3bc04
ms.openlocfilehash: 340287e4a3331eba441bce7feb957f27aca38b2b


---
# <a name="upload-data-to-azure-search-using-the-rest-api"></a>REST API kullanarak Azure Search'e veri yükleme
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Bu makalede, bir Azure Search dizinine veri aktarmak için [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)'sinin nasıl kullanılacağı gösterilir.

Bu kılavuza başlamadan önce bir [Azure Search dizini oluşturmuş](search-what-is-an-index.md) olmanız gerekir.

REST API kullanarak dizininize belgeleri göndermek için dizininizin URL uç noktasına bir HTTP POST isteği gönderirsiniz. HTTP isteğinin gövdesi eklenecek, değiştirilecek veya silinecek belgeleri içeren bir JSON nesnesidir.

## <a name="i-identify-your-azure-search-services-admin-apikey"></a>I. Azure Search hizmet yöneticinizin api anahtarını tanımlama
REST API kullanarak hizmetinize karşı HTTP istekleri gönderirken, *her bir* API isteğinin sağladığınız Search hizmeti için oluşturulmuş api anahtarını içermesi gerekir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure Portal](https://portal.azure.com/)'da oturum açmanız gerekir
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

* Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
* *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Bir dizine veri aktarma amacıyla birincil ya da ikincil yönetici anahtarınızı kullanabilirsiniz.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Hangi dizin oluşturma eyleminin kullanılacağına karar verme
REST API kullanırken, Azure Search dizininizin uç nokta URL'sine JSON istek gövdelerine sahip HTTP POST istekleri gönderirsiniz. HTTP istek gövdenizdeki JSON nesnesi, dizininize eklemek, güncelleştirmek veya silmek istediğiniz belgeleri temsil eden JSON nesnelerini içeren "value" adlı tek bir JSON dizisi içerir.

"value" dizisindeki her bir JSON nesnesi, dizine alınacak bir belgeyi temsil eder. Bu nesnelerin her biri belgenin anahtarını içerir ve istenen dizin oluşturma eylemini (karşıya yükleme, birleştirme, silme, vb.) belirtir. Yukarıdaki eylemlerden hangisini seçtiğinize bağlı olarak, her bir belgeye yalnızca belirli alanlar dahil edilmelidir:

| @search.action | Açıklama | Her bir belge için gerekli alanlar | Notlar |
| --- | --- | --- | --- |
| `upload` |Bir `upload` eylemi, belgenin yeni olması durumunda ekleneceği ve var olması durumunda güncelleştirileceği/değiştirileceği bir "upsert" ile benzerlik gösterir. |anahtar ve tanımlamak istediğiniz diğer alanlar |Var olan bir belgeyi güncelleştirirken/değiştirirken istekte belirtilmeyen herhangi bir alan `null` olarak ayarlanır. Bu durum, alan daha önce değersiz olmayan bir değere ayarlanmış olsa dahi gerçekleşir. |
| `merge` |Var olan belgeyi belirtilen alanlarla güncelleştirir. Belge dizinde mevcut değilse birleştirme işlemi başarısız olur. |anahtar ve tanımlamak istediğiniz diğer alanlar |Birleştirmede belirttiğiniz herhangi bir alan belgede var olan alanın yerini alır. Buna `Collection(Edm.String)` türünde alanlar dahildir. Örneğin, belge `["budget"]` değerine sahip bir `tags` alanını içeriyorsa ve `tags` için `["economy", "pool"]` değeriyle bir birleştirme yürütürseniz `tags` alanının son değeri `["economy", "pool"]` olur. `["budget", "economy", "pool"]` olmayacaktır. |
| `mergeOrUpload` |Belirtilen anahtara sahip bir belge dizinde zaten mevcutsa bu eylem `merge` gibi davranır. Belge mevcut değilse yeni bir belgeyle `upload` gibi davranır. |anahtar ve tanımlamak istediğiniz diğer alanlar |- |
| `delete` |Belirtilen belgeyi dizinden kaldırır. |yalnızca anahtar |Anahtar alanı dışında belirttiğiniz tüm alanlar yoksayılır. Bir belgeden tek bir alanı kaldırmak istiyorsanız bunun yerine `merge` kullanıp alanı açık bir şekilde null olarak ayarlamanız yeterlidir. |

## <a name="iii-construct-your-http-request-and-request-body"></a>III. HTTP isteğinizi ve istek gövdenizi oluşturma
Artık dizin eylemleriniz için gerekli alan değerlerini topladığınıza göre, verilerinizi içeri aktarmak için asıl HTTP isteğini ve JSON istek gövdesini oluşturmaya hazırsınız.

#### <a name="request-and-request-headers"></a>İstek ve İstek Üst Bilgileri
URL'de hizmet adınızın ve dizin adının (bu durumda "hotels") yanı sıra düzgün API sürümünü (bu belgenin yayımlandığı sırada geçerli API sürümü `2015-02-28`) de sağlamanız gerekir. `Content-Type` ve `api-key` istek üst bilgilerini tanımlamanız gerekir. İkincisi için hizmetinizin yönetici anahtarlarından birini kullanın.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>İstek Gövdesi
```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

Bu durumda arama eylemlerimiz olarak `upload`, `mergeOrUpload` ve `delete` kullanıyoruz.

Bu "hotels" dizini örneğinin, birçok belgeyle önceden doldurulduğunu varsayın. `mergeOrUpload` kullanırken olası tüm belge alanlarını belirtmek zorunda kalmadığımıza ve `delete` kullanırken yalnızca belge anahtarını (`hotelId`) belirttiğimize dikkat edin.

Ayrıca, tek bir dizin oluşturma isteğine yalnızca en fazla 1000 belge (veya 16 MB) dahil edebileceğinizi unutmayın.

## <a name="iv-understand-your-http-response-code"></a>IV. HTTP yanıt kodunuzu anlama
#### <a name="200"></a>200
Başarılı bir dizin oluşturma isteği gönderdikten sonra `200 OK` durum koduna sahip bir HTTP yanıtı alırsınız. HTTP yanıtının JSON gövdesi aşağıdaki gibidir:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
En az bir öğenin dizine alınması başarısız olduğunda `207` durum kodu döndürülür. HTTP yanıtının JSON gövdesi, başarısız belge/belgeler hakkında bilgiler içerir.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> Bu durum genellikle, arama hizmetinizdeki yükün, dizin oluşturma isteklerinin `503` yanıtları döndürmeye başlayacağı bir noktaya eriştiği anlamına gelir. Bu durumda, istemci kodunuzun geri alınmasını ve yeniden denemeden önce beklenmesini kesinlikle öneririz. Böylece kurtulması için sisteme biraz zaman tanınmış ve gelecekteki isteklerin başarılı olma şansı artırılmış olur. İsteklerinizi hızla yeniden denemeniz bu durumu yalnızca uzatır.
> 
> 

#### <a name="429"></a>429
Dizin başına belge sayısı kotanızı aştığınızda `429` durum kodu döndürülür.

#### <a name="503"></a>503
İstekteki öğelerin hiçbiri başarılı bir şekilde dizine alınamazsa `503` durum kodu döndürülür. Bu hata, sistemin aşırı yüklü olduğu ve isteğinizin şu anda işlenemediği anlamına gelir.

> [!NOTE]
> Bu durumda, istemci kodunuzun geri alınmasını ve yeniden denemeden önce beklenmesini kesinlikle öneririz. Böylece kurtulması için sisteme biraz zaman tanınmış ve gelecekteki isteklerin başarılı olma şansı artırılmış olur. İsteklerinizi hızla yeniden denemeniz bu durumu yalnızca uzatır.
> 
> 

Belge eylemleri ve başarı/hata yanıtları hakkında daha fazla bilgi için lütfen bkz. [Belge Ekleme, Güncelleştirme veya Silme](https://msdn.microsoft.com/library/azure/dn798930.aspx). Hata durumunda döndürülebilen diğer HTTP durum kodları hakkında daha fazla bilgi için bkz. [HTTP durum kodları (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Sonraki
Azure Search dizininizi doldurduktan sonra, belgeleri aramak için sorgu göndermeye başlamaya hazır olursunuz. Ayrıntılı bilgi için bkz. [Azure Search Dizininizi Sorgulama](search-query-overview.md).




<!--HONumber=Nov16_HO2-->


