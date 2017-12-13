---
title: "Grafik API'si için Azure Cosmos DB genel dağıtım Öğreticisi | Microsoft Docs"
description: "Grafik API'sini kullanarak Azure Cosmos DB genel dağıtım Kurulum öğrenin."
services: cosmos-db
keywords: "genel dağıtım, grafik, gremlin"
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 12e1ab5f57d217537ba14183500efb099985ff1e
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Grafik API'sini kullanarak Azure Cosmos DB genel dağıtım ayarlama

Bu makalede, Kurulum Azure Cosmos DB genel dağıtım için Azure Portalı'nı kullanın ve ardından (Önizleme) grafik API'sini kullanarak bağlanmak nasıl gösterir.

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * Genel dağıtım kullanarak yapılandırma [Graph API](graph-introduction.md) (Önizleme)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>.NET SDK kullanarak grafik API'sini kullanarak bir tercih edilen bölge bağlanma

Grafik API'si SQL API üstünde bir uzantı kitaplığı olarak sunulur.

Anlamıyla yararlanabilmek için [genel dağıtım](distribute-data-globally.md), istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin sıralı tercih listesi belirtebilirsiniz. Bu bağlantı İlkesi ayarlayarak yapılabilir. Azure Cosmos DB hesabı yapılandırması, geçerli bölge kullanılabilirliği ve belirtilen tercih listesi bağlı olarak, en iyi endpoint yazma gerçekleştirmek ve okuma işlemleri için SDK tarafından seçilir.

Bu tercih listesi SDK'ları kullanarak bağlantı başlatırken belirtilir. SDK'ları isteğe bağlı bir parametre "PreferredLocations" kabul Azure bölgeleri diğer bir deyişle sıralı bir listesi.

* **Yazar**: SDK, geçerli tüm yazma işlemlerini yazma bölge otomatik olarak gönderir.
* **Okur**: tüm okuma PreferredLocations listedeki ilk kullanılabilir bölge gönderilir. İstek başarısız olursa, istemci listeyi sonraki bölgeyi başarısız ve benzeri. SDK'ları yalnızca PreferredLocations içinde belirtilen bölgeler okuma dener. Bu nedenle, örneğin, Cosmos DB hesabı üç bölgelerde kullanılabilir, ancak PreferredLocations için iki yazma olmayan bölgeleri yalnızca istemci belirtir, sonra okuma yazma bölge, yük devretme durumunda bile dışında sunulacak.

Uygulama, geçerli yazma uç noktası doğrulayın ve WriteEndpoint ve ReadEndpoint, SDK sürümü 1.8 ve üzeri kullanılabilir iki özelliklerini denetleyerek SDK tarafından seçilen endpoint okuyun. PreferredLocations özellik ayarlanmamışsa, tüm istekleri geçerli yazma bölgesinden sunulacak.

### <a name="using-the-sdk"></a>SDK'sını kullanarak

Örneğin, .NET SDK ' `ConnectionPolicy` parametresi için `DocumentClient` Oluşturucusu adlı bir özellik olan `PreferredLocations`. Bu özellik, bölge adları listesi için ayarlanabilir. Görünen adları için [Azure bölgeleri] [ regions] parçası olarak belirtilen `PreferredLocations`.

> [!NOTE]
> Uç noktalar için URL'leri uzun süreli sabitleri düşünülmemelidir. Hizmet, bunlar herhangi bir noktada güncelleştirebilir. SDK, bu değişiklik otomatik olarak yönetir.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

Bu, bu öğreticinin tamamlanan kadar. Genel olarak çoğaltılmış hesabınızı tutarlılığını okuyarak yönetmek nasıl öğrenebilirsiniz [Azure Cosmos veritabanı tutarlılık düzeylerini](consistency-levels.md). Ve Azure Cosmos DB'de nasıl genel veritabanı çoğaltma hakkında daha fazla bilgi çalıştığı için bkz: [Azure Cosmos DB genel verilerle dağıtmak](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * SQL API'lerini kullanarak genel dağıtım yapılandırma

Artık Azure Cosmos DB yerel öykünücüsü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğretici devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Yerel olarak öykünücü ile geliştirme](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

