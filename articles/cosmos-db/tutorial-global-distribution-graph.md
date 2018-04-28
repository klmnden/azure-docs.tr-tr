---
title: Graph API’si için Azure Cosmos DB genel dağıtım öğreticisi | Microsoft Docs
description: Graph API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin.
services: cosmos-db
keywords: genel dağıtım, grafik, gremlin
documentationcenter: ''
author: luisbosquez
manager: kfile
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 273b5aeafbf67016259da787f4dfef078ec0a669
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Graph API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makalede, Azure portalını kullanarak Azure Cosmos DB genel dağıtımını ayarlama ve sonra Graph API’sini kullanarak bağlanma işlemlerinin nasıl yapılacağı gösterilmektedir.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [Graph API’lerini](graph-introduction.md) kullanarak genel dağıtımı yapılandırma

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>.NET SDK’sını kullanan Graph API’sini kullanarak tercih edilen bir bölgeye bağlanma

Graph API’si SQL API’sinin üzerine bir uzantı kitaplığı olarak sunulur.

[Genel dağıtımdan](distribute-data-globally.md) yararlanmak için istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin tercihe göre sıralanmış listesini belirtebilir. Bağlantı ilkesi ayarlanarak bu yapılabilir. Azure Cosmos DB hesap yapılandırmasına, geçerli bölgesel kullanılabilirliğe ve belirtilen tercih listesine göre yazma ve okuma işlemlerini gerçekleştirmek için SDK tarafından en iyi uç nokta seçilir.

SDK’ları kullanılarak bağlantı başlatırken bu tercih listesi belirtilir. SDK’lar, Azure bölgelerinin sıralı listesinde isteğe bağlı "PreferredLocations" parametresini kabul eder.

* **Yazmalar**: SDK, tüm yazma işlemlerini otomatik olarak geçerli yazma bölgesine gönderir.
* **Okumalar**: Tüm okuma işlemleri, PreferredLocations listesindeki ilk kullanılabilir bölgeye gönderilir. İstek başarısız olursa, istemci listedeki sonraki bölgeyi dener ve bu şekilde devam eder. SDK’lar yalnızca PreferredLocations listesinde belirtilen bölgelerden okuma işlemi yapmaya çalışır. Bu nedenle, örneğin, Cosmos DB hesabı dört bölgede kullanılabiliyorsa ancak istemci, PreferredLocations için yalnızca iki yazma bölgesi belirtiyorsa, yük devretme durumunda bile yazma bölgesinden okuma işlemi yapılmaz.

Uygulama, SDK 1.8 ve üzeri sürümlerde kullanılabilir olan iki WriteEndpoint ve ReadEndpoint özelliğini denetleyerek SDK tarafından seçilen geçerli yazma uç noktasını ve okuma uç noktasını doğrulayabilir. PreferredLocations özelliği ayarlanmazsa, tüm istekler geçerli yazma bölgesinden işlenir.

### <a name="using-the-sdk"></a>SDK’yı kullanarak

Örneğin, .NET SDK’sında, `DocumentClient` oluşturucusu için `ConnectionPolicy` parametresi `PreferredLocations` adlı bir özelliğe sahiptir. Bu özellik bir bölge adları listesine ayarlanabilir. [Azure Bölgeleri][regions] için görünen adlar `PreferredLocations` öğesinin bir parçası olarak belirtilebilir.

> [!NOTE]
> Uç noktaların URL’leri, uzun ömürlü sabitler olarak değerlendirilmemelidir. Bu noktada hizmet bunları güncelleştirebilir. SDK bu değişikliği otomatik olarak işler.
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

Hepsi bu kadar. Böylece bu öğretici tamamlanmış olur. [Azure Cosmos DB’deki tutarlılık düzeyleri](consistency-levels.md) bölümünü okuyarak genel olarak çoğaltılan hesabınızın tutarlılığının nasıl yönetileceğini öğrenebilirsiniz. Ayrıca genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri genel olarak dağıtma](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * SQL API’lerini kullanarak genel dağıtımı yapılandırma

Artık Azure Cosmos DB yerel öykünücüsünü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Öykünücü ile yerel olarak geliştirme](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

