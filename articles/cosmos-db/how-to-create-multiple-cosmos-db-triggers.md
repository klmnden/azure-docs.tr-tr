---
title: Birden çok bağımsız Azure Cosmos DB Tetikleyicileri oluşturma
description: Birden çok bağımsız Azure Cosmos DB olaya dayalı mimariler Azure işlevleri'ni oluşturmak için tetikleyici yapılandırmayı öğrenin.
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 4/15/2019
ms.author: maquaran
ms.openlocfilehash: 7a47a928e97d52535a6d3baa808f1fcb81d9bb55
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59700159"
---
# <a name="create-multiple-azure-cosmos-db-triggers"></a>Birden çok Azure Cosmos DB Tetikleyicileri oluşturma

Bu makalede, birden çok Cosmos DB paralel olarak çalışır ve bağımsız olarak değişikliklerine tepki tetikleyici nasıl yapılandırılacağını açıklar.

![Azure Cosmos DB tetikleyicisi ile çalışma ve kiralarını kapsayıcıyı paylaşan sunucusuz olay tabanlı İşlevler](./media/change-feed-functions/multi-trigger.png)

## <a name="event-based-architecture-requirements"></a>Olay tabanlı mimari gereksinimleri

Sunucusuz mimarileri ile derleme yaparken [Azure işlevleri](../azure-functions/functions-overview.md), sahip [önerilen](../azure-functions/functions-best-practices.md#avoid-long-running-functions) yerine büyük uzun süre çalışan işlevler birlikte çalışan küçük işlevi kümeleri oluşturmak için.

Kullanarak olay temelli sunucusuz akışlar derleme sırasında [Azure Cosmos DB tetikleyicisi](./change-feed-functions.md), belirli bir yeni bir olay olduğunda birden çok şey yapmak istediğiniz senaryoya çalıştıracaksınız [Azure Cosmos kapsayıcı](./databases-containers-items.md#azure-cosmos-containers). Tetiklemek istediğiniz eylemleri, birbirinden bağımsız için ideal çözüm olacaktır **eylem başına bir Cosmos DB tetikleyicisi oluşturma** yapmak, tüm değişikliklerin aynı Azure Cosmos kapsayıcısı üzerinde dinleme istiyorsunuz.

## <a name="optimizing-containers-for-multiple-triggers"></a>Kapsayıcılar için birden çok tetikleyici en iyi duruma getirme

Verilen *gereksinimleri* Cosmos DB tetikleyicisi, ihtiyacımız olarak da adlandırılır, durumunu depolamak için ikinci bir kapsayıcı *kiraları kapsayıcı*. Bu, her bir Azure işlevi için ayrı kiraları kapsayıcı gerektiğini anlama geliyor?

Burada, iki seçeneğiniz vardır:

* Oluşturma **bir işlev başına kapsayıcı kiralar**: Kullanmakta olduğunuz sürece, bu yaklaşım ek maliyetler çevirebilir bir [paylaşılan aktarım hızı veritabanı](./set-throughput.md#set-throughput-on-a-database). Kapsayıcı düzeyinde en düşük aktarım hızı 400 olduğunu unutmayın [istek birimi](./request-units.md), ve kiraları kapsayıcı söz konusu olduğunda, yalnızca yüklenmekte olan durumunu korumak ve ilerleme durumunu kontrol noktasına kullanılır.
* Sahip **bir kira kapsayıcı ve paylaşın** tüm işlevler için: Birden çok Azure işlevleri aynı sağlanan aktarım hızı paylaşıp olanak tanıdığından bu ikinci seçeneği kapsayıcı, sağlanan istek birimi daha iyi kullanımı sağlar.

Bu makalenin amacı, ikinci seçenek gerçekleştirmek için size rehberlik etmektir.

## <a name="configuring-a-shared-leases-container"></a>Paylaşılan kiraları kapsayıcı yapılandırma

Paylaşılan kiraları kapsayıcı yapılandırmak için almanız gereken, Tetikleyicileri yalnızca ek yapılandırma eklemektir `LeaseCollectionPrefix` [özniteliği](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---c-attributes) kullanıyorsanız C# veya `leaseCollectionPrefix` [özniteliği](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---javascript-example)JavaScript kullanıyorsa. Öznitelik değeri, hangi, belirli bir tetikleyici bir mantıksal tanımlayıcısı olmalıdır.

Örneğin, üç Tetikleyiciler varsa: e-postaları, gerçekleştirilmiş bir görünüm oluşturmak için bir toplama yapan, diğeri değişiklikleri başka bir depolama alanına gönderen gönderen bir sonra analiz etmek için atayabilir `LeaseCollectionPrefix` birinci için "e-postaların" " İkinci ve üçüncü bir "analiz" gerçekleştirilmiş".

Üç tetikler önemli bir parçası olan **aynı kiraları kapsayıcı yapılandırması kullanabilirsiniz** (hesap, veritabanı ve kapsayıcı adı).

Kullanarak bir çok basit kod örnekleri `LeaseCollectionPrefix` özniteliğini C#, şuna benzer:

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

[FunctionName("SendEmails")]
public static void SendEmails([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "emails")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}

[FunctionName("MaterializedViews")]
public static void MaterializedViews([CosmosDBTrigger(
    databaseName: "ToDoItems",
    collectionName: "Items",
    ConnectionStringSetting = "CosmosDBConnection",
    LeaseCollectionName = "leases",
    LeaseCollectionPrefix = "materialized")]IReadOnlyList<Document> documents,
    ILogger log)
{
    ...
}
```

Ve JavaScript için yapılandırma üzerinde uygulanabilir `function.json` dosyası ile `leaseCollectionPrefix` özniteliği:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "emails"
},
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "CosmosDBConnection",
    "databaseName": "ToDoItems",
    "collectionName": "Items",
    "leaseCollectionPrefix": "materialized"
}
```

> [!NOTE]
> Paylaşılan kiraları kapsayıcınızın sağlanan istek birimi üzerinde her zaman izleyin. Bunu paylaşan her tetikleyici için kullanmakta olduğunuz Azure işlevleri sayısı arttıkça, sağlanan aktarım hızı artırmak ihtiyacınız olabilecek şekilde aktarım hızı ortalama tüketimi artacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* Tam yapılandırma için bkz: [Azure Cosmos DB tetikleyicisi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration)
* Genişletilmiş denetleyin [örnekleri listesi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---example) tüm diller için.
* Azure Cosmos DB ile Azure işlevleri ile sunucusuz tarifleri ziyaret [GitHub deposu](https://github.com/ealsur/serverless-recipes/tree/master/cosmosdbtriggerscenarios) daha fazla örnek için.