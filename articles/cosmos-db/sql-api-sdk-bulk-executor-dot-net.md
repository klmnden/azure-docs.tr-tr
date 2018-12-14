---
title: 'Azure Cosmos DB: Yürütücü .NET API, SDK ve kaynakları toplu'
description: Tüm toplu Yürütücü .NET API ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB toplu Yürütücü .NET SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: tknandu
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: c239464a37637b21504227951d917977cfea6726
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343970"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET toplu Yürütücü kitaplığı: Yükleme bilgileri 

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
> * [SQL](sql-api-query-reference.md)
> * [Toplu Yürütücü - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu Yürütücü - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Açıklama**</td><td>Toplu Yürütücü kitaplığı, istemci uygulamalarının Azure Cosmos DB hesapları toplu işlemleri sağlar. Toplu Yürütücü kitaplığı BulkImport BulkUpdate ve BulkDelete ad alanları sağlar. Toplu olarak modülü BulkImport alma belgeleri bir en iyi duruma getirilmiş şekilde sağlayacak şekilde bir koleksiyon için sağlanan aktarım hızı, azami ölçüde kullanılır. Toplu olarak modülü BulkUpdate düzeltme ekleri olarak Azure Cosmos DB kapsayıcıları mevcut verileri güncelleştirin. Bir koleksiyon için sağlanan aktarım hızı, azami ölçüde kullanılır, BulkDelete modülü en iyi duruma getirilmiş bir yolla silme belgeleri topluca ekleyebilirsiniz.</td></tr>

<tr><td>**SDK'sını indirme**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Github'da Bulkexecutor'a kitaplığı**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API belgeleri**</td><td>[.NET API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[.NET SDK'sı toplu Yürütücü kitaplığını kullanmaya başlama](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Geçerli desteklenen çerçevesi**</td><td>Microsoft .NET Framework 4.5.2, 4.6.1 ve .NET Standard 2.0 </td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* MongoBulkExecutor .NET Standard 2.0 desteği dahil olmak üzere. Bu özellik için 1.3.0 işlevsel olarak eşdeğer kılar ayrıca .NET Standard 2.0 olarak hedef çerçeveyi destekleme ile serbest bırakın.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* Eklenen .NET Standard .NET Core uygulamalarıyla çalışır Bulkexecutor'a kitaplığı yapmak 2.0 desteklenen hedef çerçeveleri biri olarak.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Aşırı bölüm anahtarı, belge kimliği tanımlama grubu silmek için kabul etmek SQL API hesabı için BulkDelete işlemi eklendi.
* Aşırı silmek için belgeler belirtme giriş sorgusunda filtre olarak kullanmanın yanı sıra bölüm anahtarı değerini belirten bölüm anahtarını içeren RequestOptions kabul etmek SQL API hesabı için BulkDelete işlemi eklendi.
* Bulkexecutor'a tarafından kullanılan kullanıcı aracısı, bir biçimlendirme sorunu nedeniyle bir sorun düzeltildi.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Bulkexecutor'a alma ve güncelleştirme depolama özel durumları atma olmadan geçerli kapasitesini aştığında esnek Cosmos DB kapsayıcısı ölçeklendirme için şeffaf bir şekilde uyarlamak için API'leri geliştirmesi yaptık.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* DocumentDB .NET SDK'sı bağımlılık sürümüne 2.1.3'kurmak indirgenmesine.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Alma işlemini sabit koleksiyonlar JSRT hata vermesini Bulkexecutor'a kaynaklanan bir sorun düzeltildi.

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

## <a name="next-steps"></a>Sonraki adımlar

Toplu Yürütücü Java Kitaplığı hakkında bilgi edinmek için şu makaleye bakın:

[Java toplu Yürütücü kitaplığı SDK ve sürüm bilgileri](sql-api-sdk-bulk-executor-java.md)
