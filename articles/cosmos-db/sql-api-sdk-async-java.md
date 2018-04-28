---
title: 'Azure Cosmos DB: SQL zaman uyumsuz Java API, SDK & kaynakları | Microsoft Docs'
description: SQL zaman uyumsuz Java API ve yayın tarih, sona erme tarihlerini ve her Azure Cosmos DB SQL zaman uyumsuz Java SDK'sı sürüm arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
documentationcenter: java
author: SnehaGunda
manager: kfile
ms.assetid: a452ffa2-c15d-4b0a-a8c1-ec9b750ce52b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/20/2018
ms.author: sngun
ms.openlocfilehash: b80ad9837939af5406989d08e18f6f3d9fe3064f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
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
> 
> 

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

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Sorgu geri baskısı destek eklendi.
* Bölüm anahtarı aralığının kimliği sorgusunda desteği eklendi.
* İstek üstbilgisinde (bugfix github #24) daha büyük devamlılık belirteci izin verecek şekilde düzeltin.
* Ana iş parçacığı bittikten sonra JVM emin olmak için 4.1.22.Final için yükseltilmiş netty bağımlılık kapanır.
* Oturum belirteci ana kaynaklar okunurken geçirmemek düzeltin.
* Daha fazla örnek eklendi.
* Daha fazla Kıyaslama senaryoları eklenir.
* Sabit Java üstbilgi dosyaları uygun javadoc oluşturma için.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK engelleyici olmayan g/ç kullanarak uçtan uca desteği ile [Netty Kitaplığı](http://netty.io/) ağ geçidi modunda. 

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft sağlayacaktır bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, olduğundan bu nedenle önerilir, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz.

Cosmos devre dışı bırakılan bir SDK'sını kullanarak DB'de herhangi bir istek hizmeti tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [1.0.1](#1.0.1) |20 Nisan 2018|--- |
| [1.0.0](#1.0.0) |27 Şubat 2018|--- |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

