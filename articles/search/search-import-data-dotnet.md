---
title: İçinde bir Azure Search dizinine veri yükleme C# (.NET SDK'sı) - Azure Search
description: Azure Search kullanarak tam metin aranabilir dizin veri yükleneceğini öğrenin C# örnek kod ve .NET SDK'sı.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: d2d54d1425bbb67a3f5ba1b6081a9f74ff87f4d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871055"
---
# <a name="quickstart-2---load-data-to-an-azure-search-index-using-c"></a>Hızlı Başlangıç: 2 - bir Azure Search dizini kullanarak veri yüklemeC#

Bu makalede verilerin içe aktarılacağı gösterilmektedir [Azure Search dizini](search-what-is-an-index.md) kullanarak C# ve [.NET SDK'sı](https://aka.ms/search-sdk). Dizininize belgeleri göndermek, bu görevleri gerçekleştirerek gerçekleştirilir:

> [!div class="checklist"]
> * Oluşturma bir [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) arama dizinine bağlanmak için nesne.
> * Oluşturma bir [ `IndexBatch` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) eklenmesi, belgeleri içeren bir nesne değiştirilmiş veya silinmiş.
> * Çağrı `Documents.Index` metodunda `SearchIndexClient` belgeleri bir dizine yüklenecek.

## <a name="prerequisites"></a>Önkoşullar

[Bir Azure Search dizini oluşturma](search-create-index-dotnet.md) ve `SearchServiceClient` gösterildiği gibi nesne ["istemci oluşturma"](search-create-index-dotnet.md#CreateSearchServiceClient).


## <a name="create-a-client"></a>Bir istemci oluşturma
Veri almak için örneği gerekir. `SearchIndexClient` sınıfı. Kullanma dahil olmak üzere, bu sınıf oluşturmak için birçok yaklaşım vardır `SearchServiceClient` zaten oluşturulmuş bir örnek. 

Aşağıdaki örnekte gösterildiği gibi kullanabileceğiniz `SearchServiceClient` örneği ve çağrı kendi `Indexes.GetClient` yöntemi. Bu kod parçacığı alır bir `SearchIndexClient` "hotels" adlı dizin için bir `SearchServiceClient` adlı `serviceClient`.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

`SearchIndexClient`, `Documents` özelliğine sahiptir. Bu özellik, belgeleri dizininize eklemek, bunları değiştirmek, silmek veya sorgulamak için ihtiyacınız olan tüm yöntemleri sağlar.

> [!NOTE]
> Tipik arama uygulamasında, sorgulama ve dizin oluşturma ayrı ayrı işlenir. Sırada `Indexes.GetClient` gibi nesneleri yeniden kullanabileceğiniz kullanışlı çünkü `SearchCredentials`, daha güçlü bir yaklaşım oluşturulmasını ilgilendirir `SearchIndexClient` doğrudan bir yönetici anahtarı yerine sorgu anahtarında geçirebilmeniz. Bu uygulama ile tutarlıdır [en az ayrıcalık ilkesini](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ve uygulamanızı daha güvenli olmasına yardımcı olur. Oluşturmak bir `SearchIndexClient` sonraki alıştırmada. Anahtarları hakkında daha fazla bilgi için bkz. [oluşturun ve Azure Search hizmeti için api anahtarlarını yönetebilirsiniz](search-security-api-keys.md).
> 
> 

<a name="construct-indexbatch"></a>

## <a name="construct-indexbatch"></a>IndexBatch oluşturun

.NET SDK kullanarak veri içeri aktarmak için yedekleme verilerinizi paketini bir `IndexBatch` nesne. Bir `IndexBatch` koleksiyonunu yalıtır `IndexAction` nesneleri, her biri içeren belge ve hangi eylemin (karşıya yükleme, birleştirme, silme ve mergeOrUpload) ilgili belge üzerinde gerçekleştirmek için Azure Search söyleyen bir özellik. Dizin oluşturma eylemler hakkında daha fazla bilgi için bkz. [eylemleri dizin oluşturma: karşıya yükleme, birleştirme, mergeOrUpload, silme](search-what-is-data-import.md#indexing-actions).

Hangi eylemleri gerçekleştireceğinizi belgelerinizi bildiğiniz varsayıldığında, oluşturmak hazır olursunuz `IndexBatch`. Aşağıdaki örnek, bir toplu işlemin birkaç farklı eylemle nasıl oluşturulacağını gösterir. Örnek olarak adlandırılan özel bir sınıf kullandığımıza `Hotel` "hotels" dizinindeki bir belgeye eşler.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Bu durumda, `IndexAction` sınıfında çağrılan yöntemler tarafından belirtildiği üzere, arama eylemlerimiz olarak `Upload`, `MergeOrUpload` ve `Delete` kullanıyoruz.

Bu "hotels" dizini örneğinin, birçok belgeyle önceden doldurulduğunu varsayın. `MergeOrUpload` kullanırken olası tüm belge alanlarını belirtmek zorunda kalmadığımıza ve `Delete` kullanırken yalnızca belge anahtarını (`HotelId`) belirttiğimize dikkat edin.

Ayrıca, tek bir dizin oluşturma isteğine yalnızca en fazla 1000 belge dahil edebileceğinizi unutmayın.

> [!NOTE]
> Bu örnekte, farklı belgelere farklı eylemler uyguluyoruz. Toplu işlemdeki tüm belgelerde aynı eylemleri gerçekleştirmek istiyorsanız `IndexBatch.New` çağırmak yerine `IndexBatch` öğesinin diğer statik yöntemlerini kullanabilirsiniz. Örneğin; `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` ve `IndexBatch.Delete` çağırarak toplu işlemler oluşturabilirsiniz. Bu yöntemler, `IndexAction` nesneleri yerine bir belge koleksiyonu (bu örnekte `Hotel` türünde nesneler) alır.
> 
> 

## <a name="call-documentsindex"></a>Çağrı Documents.Index
Artık başlatılan bir `IndexBatch` nesneniz olduğuna göre `SearchIndexClient` nesneniz üzerinden `Documents.Index` çağrısı yaparak bunu dizininize gönderebilirsiniz. Aşağıdaki örnekte, nasıl `Index` çağrılacağının yanı sıra gerçekleştirmeniz gereken bazı ek adımlar gösterilmektedir:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

`try`/`catch` öğesinin, `Index` yöntemine yönelik çağrıyı çevrelediğine dikkat edin. Catch bloğu, dizin oluşturma için önemli bir hata durumunu işler. Azure Search hizmetiniz toplu işlemdeki belgelerin bazılarına dizin oluşturmada başarısız olursa `Documents.Index` tarafından bir `IndexBatchException` oluşturulur. Bu durum, hizmetiniz ağır yük altındayken belgelere dizin oluşturuyorsanız oluşabilir. **Bu durumu, kodunuzda açık şekilde işlemenizi kesinlikle öneririz.** Başarısız olan belgelere dizin oluşturmayı geciktirip sonra yeniden deneyebilir veya günlük tutup örneğin devam ettiği şekilde devam edebilir veya uygulamanızın veri tutarlılığı gereksinimlerine bağlı olarak başka bir şey yapabilirsiniz.

Son olarak, yukarıdaki örnekteki kod iki saniye gecikir. Azure Search hizmetinizde dizin oluşturma uyumsuz şekilde meydana gelir; bu nedenle belgelerin aramada kullanılabilir olduğundan emin olmak için örnek uygulamanızın kısa bir süre beklemesi gerekir. Bu gibi gecikmeler genellikle yalnızca gösterilerde, testlerde ve örnek uygulamalarda gereklidir.

Belge işleme hakkında daha fazla bilgi için bkz. [".NET SDK belgeleri nasıl işler"](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).


## <a name="next-steps"></a>Sonraki adımlar
Azure Search dizininizi doldurduktan sonra sonraki adım, belgeleri aramak için sorguları veriyor. 

> [!div class="nextstepaction"]
> [İçinde bir Azure Search dizinini sorgulamaC#](search-query-dotnet.md)
