---
title: Dizin Oluşturucu durumunu ve sonuçları - Azure Search izleme
description: Durum, ilerleme ve REST API'si veya .NET SDK kullanarak Azure portalındaki Azure Search dizin oluşturucularında sonuçlarını izleyin.
ms.date: 06/28/2019
author: RobDixon22
manager: HeidiSteen
ms.author: v-rodixo
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 07b4fe2ef830c3ce09b655cf4b433d14923229a9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486291"
---
# <a name="how-to-monitor-azure-search-indexer-status-and-results"></a>Azure Search dizin oluşturucu durumunu ve sonuçlarını izleme

Azure arama, durum ve izleme her Indexer'ın geçerli ve geçmiş çalıştırma hakkında bilgi sağlar.

Dizin Oluşturucu izleme istediğinizde yararlı olur:

* İzleme sırasında devam eden bir dizin oluşturucu ilerlemesini çalıştırın.
* Devam eden veya önceki dizin oluşturucuyu çalıştırma sonuçlarını gözden geçirin.
* Üst düzey bir dizin oluşturucu tanımlamak hatalarını ve hata veya uyarı hakkında tek tek belgeler dizine ekleniyor.

## <a name="find-indexer-status-and-history-details"></a>Dizin Oluşturucu durumu ve geçmişi ayrıntılarını bulun

Dizin Oluşturucu izleme bilgilerini dahil olmak üzere çeşitli yollarla erişebilirsiniz:

* [Azure portalında](#portal)
* Kullanarak [REST API](#restapi)
* Kullanarak [.NET SDK'sı](#dotnetsdk)

(Biçimleri farklı veri erişimi yöntemi kullanılan temel rağmen) izleme bilgilerini kullanılabilir dizin oluşturucu tüm şunları içerir:

* Dizin oluşturucunun kendisi hakkında durum bilgileri
* En son hakkında bilgi Oluşturucusu durumunu, başlangıç ve bitiş zamanlarını ve ayrıntılı hatalar ve uyarılar dahil olmak üzere, çalıştırın.
* Geçmiş dizin oluşturucu çalıştırıldığında ve durumlar, sonuçları, hataları ve Uyarıları listesi.

Büyük miktarda veri işleyen dizin oluşturucular çalıştırmak uzun sürebilir. Örneğin, milyonlarca kaynak belge işleme dizin oluşturucular, 24 saat çalıştırma ve hemen yeniden başlatın. Yüksek hacimli dizin oluşturucular için durumu her zaman diyebilirsiniz **sürüyor** portalında. Bir dizin oluşturucu bile çalışırken, devam eden ilerlemeyi ve önceki çalıştırmaları hakkında ayrıntı bulabilirsiniz.

<a name="portal"></a>

## <a name="monitor-indexers-in-the-portal"></a>İzleyici dizin oluşturucular portalda

Tüm, dizin oluşturucularda geçerli durumunu görebilirsiniz **dizin oluşturucular** arama hizmeti genel bakış sayfanıza listesi.

   ![Dizin oluşturucuların listesini](media/search-monitor-indexers/indexers-list.png "Dizin oluşturucuların listesini")

Ne zaman bir dizin oluşturucu yürütülmekte listesi gösterir durumu **sürüyor**ve **Docs başarılı** değeri şu ana kadar işlenen belge sayısı gösterilir. Portalda dizin oluşturucu durum değerleri güncelleştirme birkaç dakika sürebilir ve belge sayılarını.

Bir dizin oluşturucu, en son çalıştırma başarılı gösterir olan **başarı**. Tek tek belgeler hataları olsa bile hatalarının sayısını oluşturucunun altındaysa dizin oluşturucunun çalıştırılması başarılı olabileceğini **başarısız öğe maksimum sayısı** ayarı.

En son bir hata ile sona erdi çalıştırırsanız, durumunun **başarısız**. Durumu **sıfırlama** oluşturucunun değişiklik izleme durumu sıfırlandı anlamına gelir.

Hakkında daha fazla ayrıntı oluşturucunun geçerli ve yeni görmek için listedeki bir dizin oluşturucu tıklayarak çalıştırır.

   ![Dizin Oluşturucu özeti ve yürütme geçmişini](media/search-monitor-indexers/indexer-summary.png "dizin oluşturucu özeti ve yürütme geçmişi")

**Dizin oluşturucu özeti** grafik, en son çalışır'işlenen belge sayısı bir grafiği görüntüler.

**Yürütme ayrıntıları** 50 en son yürütme sonuçlarının listesini gösterir.

Çalıştırılan ilgili ayrıntıları görmek için listenin bir yürütme sonuca tıklayın. Bu, başlangıç ve bitiş zamanlarını ve hataları ve oluşan uyarıları içerir.

   ![Dizin Oluşturucu yürütme ayrıntıları](media/search-monitor-indexers/indexer-execution.png "dizin oluşturucusu yürütme ayrıntıları")

Çalıştırma sırasında belgeye özgü sorunlar varsa, hataları ve Uyarıları alanları listelenir.

   ![Dizin Oluşturucu ayrıntıları hatalarla](media/search-monitor-indexers/indexer-execution-error.png "hatalarla dizin oluşturucu ayrıntıları")

Uyarılar, dizin oluşturucular bazı türleri yaygın olarak bulunur ve her zaman bir sorun göstermez. Örneğin, görüntü veya PDF dosyalarını işlemek için herhangi bir metin içermeyen bilişsel Hizmetler'i kullanma dizin oluşturucular uyarıları bildirebilirsiniz.

Dizin Oluşturucu hataları ve Uyarıları araştırma hakkında daha fazla bilgi için bkz. [Azure Search'te yaygın dizin oluşturucu sorunları giderme](search-indexer-troubleshooting.md).

<a name="restapi"></a>

## <a name="monitor-indexers-using-the-rest-api"></a>REST API kullanarak dizin oluşturucular izleyin

Bir dizin oluşturucu kullanarak, durum ve yürütme geçmişini alabilirsiniz [dizin oluşturucu durumunu Al komutu](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

Yanıt, genel dizin oluşturucu durumu, son (veya devam eden) dizin oluşturucuyu çağırmayı ve son dizin oluşturucu çağrılarını geçmişini içerir.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Kadar geriye doğru kronolojik sırayla (en son gerçekleşen en başta) sıralanır 50 en son çalıştığında, yürütme geçmişini içerir.

İki farklı durum değerleri olduğuna dikkat edin. Dizinleyici için üst düzey durumudur. Bir dizin oluşturucu durumunu **çalıştıran** dizin oluşturucu doğru ayarlandığından ve şu anda kullanılabilir değil onun çalıştırılacak kullanıcının anlamına gelir çalışıyor.

Her dizin oluşturucunun ayrıca o yürütmenin devam eden olup olmadığını belirtir, kendi durum vardır (**çalıştıran**), veya zaten tamamlandı bir **başarı**, **transientFailure**, veya **persistentFailure** durumu. 

Değişiklik izleme durumunu yenilemek için bir dizin oluşturucu sıfırladığınızda ayrı yürütme geçmişi girişi ile eklendiğinde bir **sıfırlama** durumu.

Durum kodları ve dizin oluşturucu izleme verileri hakkında daha fazla ayrıntı için [GetIndexerStatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).

<a name="dotnetsdk"></a>

## <a name="monitor-indexers-using-the-net-sdk"></a>.NET SDK kullanarak dizin oluşturucular izleyin

Azure Search .NET SDK'sını kullanarak bir dizin oluşturucu zamanlaması tanımlayabilirsiniz. Bunu yapmak için dahil **zamanlama** oluşturulurken veya güncelleştirilirken bir dizin oluşturucu özelliği.

Aşağıdaki C# örnek, bir oluşturucunun durumu hakkındaki bilgileri yazar ve konsola sonuçlarını, en son (veya devam eden) çalıştırın.

```csharp
static void CheckIndexerStatus(Indexer indexer, SearchServiceClient searchService)
{
    try
    {
        IndexerExecutionInfo execInfo = searchService.Indexers.GetStatus(indexer.Name);

        Console.WriteLine("Indexer has run {0} times.", execInfo.ExecutionHistory.Count);
        Console.WriteLine("Indexer Status: " + execInfo.Status.ToString());

        IndexerExecutionResult result = execInfo.LastResult;

        Console.WriteLine("Latest run");
        Console.WriteLine("  Run Status: {0}", result.Status.ToString());
        Console.WriteLine("  Total Documents: {0}, Failed: {1}", result.ItemCount, result.FailedItemCount);

        TimeSpan elapsed = result.EndTime.Value - result.StartTime.Value;
        Console.WriteLine("  StartTime: {0:T}, EndTime: {1:T}, Elapsed: {2:t}", result.StartTime.Value, result.EndTime.Value, elapsed);

        string errorMsg = (result.ErrorMessage == null) ? "none" : result.ErrorMessage;
        Console.WriteLine("  ErrorMessage: {0}", errorMsg);
        Console.WriteLine("  Document Errors: {0}, Warnings: {1}\n", result.Errors.Count, result.Warnings.Count);
    }
    catch (Exception e)
    {
        // Handle exception
    }
}
```

Konsolunda çıktısı aşağıdakine benzer görünecektir:

    Indexer has run 18 times.
    Indexer Status: Running
    Latest run
      Run Status: Success
      Total Documents: 7, Failed: 0
      StartTime: 10:02:46 PM, EndTime: 10:02:47 PM, Elapsed: 00:00:01.0990000
      ErrorMessage: none
      Document Errors: 0, Warnings: 0

İki farklı durum değerleri olduğuna dikkat edin. Üst düzey durum dizinleyicinin durumudur. Bir dizin oluşturucu durumunu **çalıştıran** dizin oluşturucu doğru ayarlandığından anlamına gelir ve yürütme, ancak bu BT yürütülmekte için kullanılabilir.

Her dizin oluşturucunun ayrıca ilgili yürütme devam ediyor için kendi durum vardır (**çalıştıran**), veya ile zaten tamamlandığından bir **başarı** veya **TransientError** durumu. 

Değişiklik izleme durumunu yenilemek için bir dizin oluşturucu sıfırlandığında, ayrı bir geçmiş girişi ile eklendiğinde bir **sıfırlama** durumu.

Durum kodları ve dizin oluşturucu izleme bilgileri hakkında daha fazla ayrıntı için [GetIndexerStatus](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) REST API'de.

Belgeye özgü hatalar veya uyarılar hakkında ayrıntılar listeleri numaralandırarak alınabilir `IndexerExecutionResult.Errors` ve `IndexerExecutionResult.Warnings`.

Dizin oluşturucular izlemek için kullanılan .NET SDK sınıfları hakkında daha fazla bilgi için bkz. [IndexerExecutionInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutioninfo?view=azure-dotnet) ve [IndexerExecutionResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet).
