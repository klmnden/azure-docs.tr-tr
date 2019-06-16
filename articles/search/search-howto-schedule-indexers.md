---
title: Dizin oluşturucular - Azure Search zamanlama
description: Azure Search dizin oluşturucularında dizin içeriği düzenli aralıklarla veya belirli zamanlarda zamanlayın.
ms.date: 05/31/2019
author: RobDixon22
manager: HeidiSteen
ms.author: v-rodixo
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 764fca8d3cb4cd9c40d7880043637f89ef1a8578
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755388"
---
# <a name="how-to-schedule-indexers-for-azure-search"></a>Dizin oluşturucular için Azure Search zamanlama
Oluşturulduktan hemen sonra bir dizin oluşturucu genellikle bir kez çalışır. Yeniden portalı, REST API'si veya .NET SDK kullanarak isteğe bağlı olarak çalıştırabilirsiniz. Ayrıca, düzenli bir zamanlamaya göre çalıştırmak için bir dizin oluşturucusunu da yapılandırabilirsiniz.

Dizin Oluşturucu zamanlaması yararlı olduğu bazı durumlar:

* Kaynak veriler zamanla değişir ve otomatik olarak değiştirilen verileri işlemek için Azure Search dizin oluşturucularında istiyorsunuz.
* Birden çok veri kaynağından dizin doldurulur ve dizin oluşturucular çakışmalarını azaltmak için farklı zamanlarda çalışmak emin olmak istiyoruz.
* Kaynak veriler çok büyükse ve zaman içinde işleme dizin oluşturucu yayılan istiyorsunuz. Büyük hacimli verileri dizinleme hakkında daha fazla bilgi için bkz. [büyük veri kümeleri Azure Search dizini oluşturmak nasıl](search-howto-large-index.md).

Zamanlayıcı, Azure Search'ün yerleşik bir özelliğidir. Search dizin oluşturucularında denetlemek için bir dış Zamanlayıcı kullanamazsınız.

## <a name="define-schedule-properties"></a>Zamanlama özelliklerini tanımlar

Bir dizin oluşturucu zamanlaması iki özelliğe sahiptir:
* **Aralığı**, dizin oluşturucusu yürütme zamanlanmış hangi arasında süreyi tanımlar. İzin verilen en küçük aralığı 5 dakikadır ve en büyük 24 saattir.
* **Başlama zamanı (UTC)** , hangi dizin oluşturucunun çalıştırılması gerekir ilk kez gösterir.

Dizin Oluşturucu ilk oluştururken veya oluşturucunun özellikleri daha sonra güncelleştirerek, bir zamanlama belirtebilirsiniz. Dizin Oluşturucu zamanlamaları kullanarak ayarlanabilir [portalı](#portal), [REST API](#restApi), veya [.NET SDK'sı](#dotNetSdk).

Bir dizin oluşturucu yalnızca tek bir yürütme, aynı anda çalıştırabilirsiniz. Bir dizin oluşturucu sonraki yürütme zamanlandığında zaten çalışıyorsa bu yürütme sonraki zamanlanan saatine kadar ertelenir.

Bu daha somut anlaşılması için bir örnek düşünelim. Olan bir dizin oluşturucu zamanlaması yapılandırıyoruz varsayalım bir **aralığı** , saatlik ve **başlattığınızda** , 1 Haziran 2019 saati 8:00:00: 00 UTC. Dizin oluşturucunun çalıştırılması bir saatten daha uzun sürerse ne oluşabilir aşağıda verilmiştir:

* 1 Haziran 2019 8:00:00 UTC adresindeki geçici veya bu ilk dizin oluşturucu yürütmeyi başlatır. Bu yürütme 20 dakika (veya dilediğiniz zaman küçüktür 1 saat) geçen varsayılır.
* İkinci yürütme sırasında veya 1 Haziran 2019'da 9: 00'da başlar UTC. Bu yürütme - bir saatten – 70 dakika sürer ve 10:10:00 UTC kadar tamamlamayacaktır olduğunu varsayalım.
* Üçüncü yürütme 10:00:00 UTC'de başlatmak üzere zamanlandı ancak o anda önceki yürütme hala çalışıyor. Bu zamanlanmış yürütme ardından atlandı. Sonraki yürütme dizin oluşturucunun 11: 00'da UTC kadar başlatılmaz.

<a name="portal"></a>

## <a name="define-a-schedule-in-the-portal"></a>Portalda bir zamanlamayı tanımlayın

Verileri İçeri Aktarma Sihirbazı'nı Portalı'nda bir dizin oluşturucu zamanlaması oluşturma zamanında tanımlamanızı sağlar. Varsayılan zamanlama ayarı **saatlik**, dizin oluşturucu anlamına gelir ve oluşturulur ve yeniden her saat daha sonra çalışan sonra bir kez çalışır.

Zamanlama ayarı değiştirebilirsiniz **sonra** otomatik olarak yeniden çalıştırmak için dizin oluşturucu istemiyorsanız veya **günlük** günde bir kez çalıştırılacak. Ayarlayın **özel** farklı bir zaman aralığı veya belirli bir gelecek başlangıç saatini belirtmek istiyorsanız.

Zamanlamayı ayarladığınızda **özel**, alanlar görünür belirtmenize olanak **aralığı** ve **başlangıç saati (UTC)** . İzin verilen aralığı 5 dakika ile en uzun olan en kısa süre 1440 dakika (24 saat) ' dir.

   ![Verileri İçeri Aktarma Sihirbazı'nda ayarı dizin oluşturucu zamanlaması](media/search-howto-schedule-indexers/schedule-import-data.png "verileri İçeri Aktarma Sihirbazı'nda ayarı dizin oluşturucu zamanlaması")

Bir dizin oluşturucu oluşturulduktan sonra oluşturucunun Düzen panelini kullanarak zamanlama ayarlarını değiştirebilirsiniz. Zamanlama verilerini İçeri Aktar Sihirbazı'nı tutarların alanlardır.

   ![Dizin Oluşturucu Düzen panelinde zamanlama ayarı](media/search-howto-schedule-indexers/schedule-edit.png "dizin oluşturucu Düzen panelinde zamanlamayı ayarlama")

<a name="restApi"></a>

## <a name="define-a-schedule-using-the-rest-api"></a>REST API kullanarak zamanlama tanımla

REST API kullanarak bir dizin oluşturucu zamanlaması tanımlayabilirsiniz. Bunu yapmak için dahil **zamanlama** oluştururken veya dizin oluşturucunun güncelleştirme özelliği. Aşağıdaki örnekte mevcut bir dizin oluşturucu güncelleştirmek için PUT İsteği gösterilmektedir:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aralığı** parametresi gereklidir. İki ardışık dizin oluşturucusu yürütme başlangıcı arasındaki zaman aralığını gösterir. İzin verilen en küçük aralığı 5 dakikadır; uzun bir gündür. Bir XSD "dayTimeDuration" değeri biçimlendirilmelidir (sınırlı bir alt kümesini bir [ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değeri). Bu desen: `P(nD)(T(nH)(nM))`. Örnekler: `PT15M` her 15 dakika boyunca `PT2H` her 2 saatte için.

İsteğe bağlı **startTime** zamanlanan yürütme ne zaman başlaması gerektiğini belirtir. Atlanırsa, geçerli UTC zamanı kullanılır. Bu süre içinde ilk çalıştırma durumu zamanlandığı dizin oluşturucuyu özgün beri sürekli olarak çalışıyorsa geçmişte olabilir **startTime**.

Çalıştırma dizin oluşturucu çağrısı kullanarak istediğiniz zaman isteğe bağlı bir dizin oluşturucu da çalıştırabilirsiniz. Dizin oluşturucular çalışan ve dizin oluşturucu zamanlamaları ayarlama hakkında daha fazla bilgi için bkz. [dizin oluşturucuyu Çalıştır](https://docs.microsoft.com/rest/api/searchservice/run-indexer), [alma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/get-indexer), ve [güncelleştirme dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/update-indexer) REST API Başvurusu.

<a name="dotNetSdk"></a>

## <a name="define-a-schedule-using-the-net-sdk"></a>.NET SDK kullanarak zamanlama tanımla

Azure Search .NET SDK'sını kullanarak bir dizin oluşturucu zamanlaması tanımlayabilirsiniz. Bunu yapmak için dahil **zamanlama** oluşturulurken veya güncelleştirilirken bir dizin oluşturucu özelliği.

Aşağıdaki C# örneği, önceden tanımlanmış veri kaynağı ve dizini kullanarak, bir dizin oluşturucu oluşturur ve sonra 30 dakika bugünden itibaren her gün çalışacak şekilde zamanlaması ayarlar:

```
    Indexer indexer = new Indexer(
        name: "azure-sql-indexer",
        dataSourceName: dataSource.Name,
        targetIndexName: index.Name,
        schedule: new IndexingSchedule(
                        TimeSpan.FromDays(1), 
                        new DateTimeOffset(DateTime.UtcNow.AddMinutes(30))
                    )
        );
    await searchService.Indexers.CreateOrUpdateAsync(indexer);
```
Varsa **zamanlama** parametresi atlanırsa, oluşturulduktan hemen sonra Dizin Oluşturucu yalnızca bir kez çalışır.

**StartTime** geçmişteki bir zamana parametresi ayarlanabilir. Dizin Oluşturucu beri sürekli olarak çalışıyorsa bu durumda, ilk yürütme zamanlandı verilen **startTime**.

Zamanlama kullanılarak tanımlanan [IndexingSchedule](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingschedule?view=azure-dotnet) sınıfı. **IndexingSchedule** Oluşturucusu gerektirir bir **aralığı** parametresi kullanılarak belirtilen bir **TimeSpan** nesne. İzin verilen en küçük aralık değeri 5 dakikadır ve en büyük 24 saattir. İkinci **startTime** parametresi olarak belirtilen bir **DateTimeOffset** nesne, isteğe bağlıdır.

.NET SDK kullanarak denetim dizin oluşturucu işlemleri sağlar [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) sınıf ve onun [dizin oluşturucular](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexers) yöntemleri uygulayan özelliği **IIndexersOperations**arabirimi. 

Bir dizin oluşturucu kullanarak herhangi bir zamanda isteğe bağlı olarak çalıştırabileceğiniz [çalıştırma](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.run), [RunAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.runasync), veya [RunWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations.runwithhttpmessagesasync) yöntemleri.

Oluşturma hakkında daha fazla bilgi için bkz: güncelleştirme ve dizin oluşturucular, çalışan [IIindexersOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations?view=azure-dotnet).
