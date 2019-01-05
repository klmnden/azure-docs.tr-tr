---
title: 'Azure Cosmos DB: SQL Async Java API, SDK ve kaynakları'
description: Tüm SQL Async Java API'si ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB SQL Async Java SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: moderakh
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 11/29/2018
ms.author: moderakh
ms.openlocfilehash: fbb1757cfb1118380e2f7d79566f6dc9832fce23
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041502"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Async Java SDK'sı SQL API'si için: Sürüm Notları ve kaynakları
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
> * [Bulkexecutor'a - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulkexecutor'a - Java](sql-api-sdk-bulk-executor-java.md)

SQL API Async Java SDK'sı desteği ile zaman uyumsuz işlemleri sağlayarak SQL API Java SDK'sından farklıdır [Netty Kitaplığı](https://netty.io/). Önceden varolan [SQL API Java SDK'sı](sql-api-sdk-java.md) zaman uyumsuz işlemleri desteklemez. 

<table>

<tr><td>**SDK'sını indirme**</td><td>[Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)</td></tr>

<tr><td>**API belgeleri**</td><td>[Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._asyncdocumentclient?view=azure-java-stable)</td></tr>

<tr><td>**SDK'sı için katkıda bulunan**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[Async Java SDK'sı ile çalışmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)</td></tr>

<tr><td>**Kod örneği**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)</td></tr>

<tr><td>**Performans ipuçları**</td><td>[GitHub Benioku](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)</td></tr>

<tr><td>**En düşük desteklenen çalışma zamanı**</td><td>[JDK 8](https://aka.ms/azure-jdks)</td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Bir kaynak sızıntısı hata düzeltildi.
* MultiPolygon desteği eklendi
* Özel üst bilgilerinde RequestOptions desteği eklendi.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Paketleme hata düzeltildi.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Yazma yeniden deneme yolunda NPE düzeltildi.
* Uç nokta yönetiminde NPE düzeltildi.
* Yükseltme savunmasız bağımlılıkları ([GitHub #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Sorun giderme için günlük Netty ağ desteği eklendi.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Çok bölgeli yazma desteği eklendi.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Proxy için destek eklendi.
* Kaynak belirteci yetkilendirme için destek eklendi.
* Büyük bölüm anahtarlarını işleme içinde bir hata düzeltildi ([GitHub #63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Belgeleri geliştirdik.
* SDK daha ayrıntılı modüllerine yeniden oluşturulamaz.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* İngilizce dışındaki diller için bir hata düzeltildi ([GitHub #51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Çakışma kaynağında ek yardımcı yöntemler.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Performansı artırmak ve lisans nedeniyle org.JSON bağımlılık jackson tarafından değiştirildi ([GitHub #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)).
* Kullanım dışı OfferV2 sınıfı kaldırıldı.
* İçerik aktarım hızı için teklif sınıfı eklendi erişimci yöntemi.
* Herhangi bir yöntem belge/kaynağında org.json türleri bir jackson nesne türü döndürmek için değiştirildi.
* jackson ObjectNode döndürülecek genişletme JsonSerializable değiştirilen sınıfları getObject(.) yöntemi yazın.
* getCollection(.) yöntemi, koleksiyon ObjectNode döndürülecek değiştirildi.
* Kaldırılan JsonSerializable kılabileceği oluşturucularla org.json.JSONObject bağımsız değişken.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented), artık iki boşluk girintileme için kullanır.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Benzersiz bir dizin ilke için destek eklendi.
* Akış seçenekleri yanıt devamlılık belirteci boyutu sınırlama için destek eklendi.
* Çapraz bölüm sorgusunun bölüm bölmedeki desteği eklendi.
* Json zaman damgası serileştirme düzeltildi ([GitHub #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)).
* Json enum serileştirme düzeltildi.
* 2 MB boyutunda Belge yönetiminde bir hata düzeltildi ([GitHub #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)).
* Bağımlılık com.fasterxml.jackson.core:jackson-databind 2.9.5 bir hata nedeniyle yükseltme ([jackson databind: GitHub #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Bir hata nedeniyle 0.8.0.17 yükseltme rxjava-ek özellikler bağımlılık ([rxjava ek özellikler: GitHub #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* Meta veri açıklamasını pom dosyasına satır içi belgeler geri kalanıyla olacak şekilde güncelleştirildi.
* Söz dizimi geliştirme ([GitHub #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([GitHub #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Sorgu geri baskısı desteği eklendi.
* Sorgu bölüm anahtar aralığı kimliği desteği eklendi.
* Daha büyük bir devamlılık belirteci (hata düzeltmesi GitHub #24) istek üst bilgisinde izin verecek şekilde düzeltin.
* Ana iş parçacığı sonlandırıldıktan sonra JVM sağlamak için 4.1.22.Final yükseltilmiş netty bağımlılık kapanır.
* Oturum belirteci ana kaynaklarını okurken geçirmekten kaçının düzeltin.
* Daha fazla örnek eklendi.
* Daha fazla Kıyaslama senaryoları eklenir.
* Uygun java belge oluşturma için sabit Java üst bilgi dosyaları.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'sı ile engelleyici olmayan g/ç kullanarak uçtan uca Destek [Netty Kitaplığı](https://netty.io/) ağ geçidi modunda. 

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Sağlar; Microsoft bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Yeni özellikleri ve işlevselliği ve iyileştirmeler yalnızca geçerli SDK'sı eklenir. Bu nedenle, her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz önerilir.

Cosmos DB devre dışı bırakılan bir SDK'sını kullanarak yapılan tüm istekleri hizmet tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.3.0](#2.3.0) |29 Kasım 2018|--- |
| [2.2.2](#2.2.2) |8 Kasım 2018|--- |
| [2.2.1](#2.2.1) |2 Kasım 2018|--- |
| [2.2.0](#2.2.0) |22 Eylül 2018'den|--- |
| [2.1.0](#2.1.0) |5 Eylül 2018'den|--- |
| [2.0.1](#2.0.1) |16 Ağustos 2018|--- |
| [2.0.0](#2.0.0) |20 Haziran 2018|--- |
| [1.0.2](#1.0.2) |18 Mayıs 2018|--- |
| [1.0.1](#1.0.1) |20 Nisan 2018|--- |
| [1.0.0](#1.0.0) |27 Şubat 2018|--- |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası.

