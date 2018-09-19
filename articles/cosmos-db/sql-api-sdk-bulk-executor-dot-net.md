---
title: 'Azure Cosmos DB: Toplu Yürütücü .NET API, SDK ve kaynakları | Microsoft Docs'
description: Tüm toplu Yürütücü .NET API ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB toplu Yürütücü .NET SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/14/2018
ms.author: ramkris
ms.openlocfilehash: ffd8f438429cd8769ac0dbff7f489327166e0000
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294467"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET toplu Yürütücü kitaplığı: karşıdan yükleme bilgileri 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Toplu Yürütücü - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu Yürütücü - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Açıklama**</td><td>Azure Cosmos DB hesaplarında toplu işlemleri gerçekleştirmek istemci uygulamaları toplu Yürütücü kitaplık sağlar. Toplu Yürütücü kitaplığı BulkImport ve BulkUpdate BulkDelete ad alanları sağlar. Toplu olarak modülü BulkImport alma belgeleri bir en iyi duruma getirilmiş şekilde sağlayacak şekilde bir koleksiyon için sağlanan aktarım hızı, azami ölçüde kullanılır. Toplu olarak modülü BulkUpdate düzeltme ekleri olarak Azure Cosmos DB kapsayıcıları mevcut verileri güncelleştirin. Bir koleksiyon için sağlanan aktarım hızı, azami ölçüde kullanılır, BulkDelete modülü en iyi duruma getirilmiş bir yolla silme belgeleri topluca ekleyebilirsiniz.</td></tr>

<tr><td>**SDK'sını indirme**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Github'da Bulkexecutor'a kitaplığı**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API belgeleri**</td><td>[.NET API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[.NET SDK'sı toplu Yürütücü kitaplığını kullanmaya başlama](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Geçerli desteklenen çerçevesi**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)(sürüm > 2.0.0 =)</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)(sürüm > 9.0.1 =)
</li></ul></td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Azure Cosmos DB SQL API hesabı için BulkDelete işlemi için destek eklendi.
* Azure Cosmos DB MongoDB API hesabı için BulkImport işlemi için destek eklendi.
* DocumentDB .NET SDK'sı sürüm 2.0.0 bağımlılığı'kurmak indirgenmesine. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Azure Cosmos DB Gremlin API hesapları için BulkImport işlemi için destek eklendi.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Azure Cosmos DB SQL API hesabı için BulkImport işleme küçük hata düzeltmesi.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Azure Cosmos DB SQL API hesabı BulkImport ve BulkUpdate işlemlerinde desteği eklendi.
