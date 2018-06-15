---
title: 'Azure Cosmos DB: SQL zaman uyumsuz Java API, SDK & kaynakları | Microsoft Docs'
description: SQL zaman uyumsuz Java API ve yayın tarih, sona erme tarihlerini ve her Azure Cosmos DB SQL zaman uyumsuz Java SDK'sı sürüm arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/18/2018
ms.author: sngun
ms.openlocfilehash: 4b12652783c94d132a5c1f4d4aa352d4e2318edf
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797677"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Cosmos DB zaman uyumsuz Java için Azure SDK SQL API: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik besleme](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

SQL API zaman uyumsuz Java SDK'sını desteği ile zaman uyumsuz işlemleri sağlayarak SQL API Java SDK'sını farklı [Netty Kitaplığı](http://netty.io/). Önceden varolan [SQL API Java SDK'sını](sql-api-sdk-java.md) zaman uyumsuz işlemleri desteklemez. 

<table>

<tr><td>**SDK yükleme**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**API belgeleri**</td><td>[Java API başvuru belgeleri](https://azure.github.io/azure-cosmosdb-java/)</td></tr>

<tr><td>**SDK katkıda bulunan**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[Zaman uyumsuz Java SDK'sı ile çalışmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**kod örneği**</td><td>[Github](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Performans ipuçları**</td><td>[Github Benioku dosyası](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**Minimum desteklenen çalışma zamanı**</td><td>[JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Benzersiz bir dizin ilke desteği eklendi.
* Yanıt devamlılık belirteci boyutu akışı seçenekleri sınırlamak için destek eklendi.
* Bölüm sorgusu çapraz bölüm bölmedeki desteği eklendi.
* Json zaman damgası seri hale getirme hatanın düzeltildiğini ([github #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Bir hata Json enum seri hale getirme sabit.
* 2 MB boyutunda belgeleri yönetme hatanın düzeltildiğini ([github #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Bağımlılık com.fasterxml.jackson.core:jackson-databind 2.9.5 bir hata nedeniyle yükseltildiğinde ([jackson databind: github #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Bir hata nedeniyle 0.8.0.17 yükseltildiğinde rxjava-ek özellikler bağımlılığını ([rxjava ek özellikler: #30 github](https://github.com/davidmoten/rxjava-extras/issues/30)).
* Meta veri açıklamasını pom dosyasında satır içi belgelerine kalanıyla olacak şekilde güncelleştirildi.
* Sözdizimi geliştirme ([github #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([github #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Sorgu geri baskısı destek eklendi.
* Bölüm anahtarı aralığının kimliği sorgusunda desteği eklendi.
* İstek üstbilgisinde (bugfix github #24) daha büyük devamlılık belirteci izin verecek şekilde düzeltin.
* Ana iş parçacığı bittikten sonra JVM emin olmak için 4.1.22.Final için yükseltilmiş netty bağımlılık kapanır.
* Oturum belirteci ana kaynaklar okunurken geçirmemek düzeltin.
* Daha fazla örnek eklendi.
* Daha fazla Kıyaslama senaryoları eklenir.
* Sabit Java üstbilgi dosyaları uygun java belge oluşturma için.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK engelleyici olmayan g/ç kullanarak uçtan uca desteği ile [Netty Kitaplığı](http://netty.io/) ağ geçidi modunda. 

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft sağlayacaktır bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yalnızca yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sına eklenir. Bu nedenle, her zaman en son SDK sürüm olabildiğince erken yükseltmeniz önerilir.

Cosmos devre dışı bırakılan bir SDK'sını kullanarak DB'de herhangi bir istek hizmeti tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [1.0.2](#1.0.2) |18 May 2018|--- |
| [1.0.1](#1.0.1) |20 Nisan 2018|--- |
| [1.0.0](#1.0.0) |27 Şubat 2018|--- |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

