---
title: 'Azure Cosmos DB: SQL Async Java API, SDK ve kaynakları'
description: Tüm SQL Async Java API'si ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB SQL Async Java SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: moderakh
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 07/01/2019
ms.author: moderakh
ms.openlocfilehash: 3cafa4d5aecaa4c8f3863c3269ec02793340e3e6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509272"
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

| |  |
|---|---|
| **SDK'sını indirme** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) |
|**API belgeleri** |[Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient?view=azure-java-stable) | 
|**SDK'sı için katkıda bulunan** | [GitHub](https://github.com/Azure/azure-cosmosdb-java) | 
|**Kullanmaya başlama** | [Async Java SDK'sı ile çalışmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) | 
|**Kod örneği** | [GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)| 
| **Performans ipuçları**| [GitHub Benioku](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)| 
| **En düşük desteklenen çalışma zamanı**|[JDK 8](https://aka.ms/azure-jdks) | 

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name250250"></a><a name="2.5.0"/>2.5.0
* TCP modunu artık varsayılan olarak
* Çapraz bölüm sorgu ölçümleri, artık tüm bölümleri döndürür
* Genel tanımlayıcı artık düzgün çalışıyor
* Sorgular için yük devretme düzgün şekilde çok ana için yeniden deneme sayısı
* Güvenlik düzeltmeleri bağımlılık tümsekler

### <a name="a-name245245"></a><a name="2.4.5"/>2.4.5
* Hata düzeltmesi karma V2 desteği

### <a name="a-name243243"></a><a name="2.4.3"/>2.4.3
* Client#close() üzerinde kaynak sızıntıları için hata düzeltmesi ([github #88](https://github.com/Azure/azure-cosmosdb-java/issues/88)).

### <a name="a-name242242"></a><a name="2.4.2"/>2.4.2
* Eklenen devamlılık belirteci desteği için çapraz bölüm sorgular.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1
* Doğrudan modda bazı hatalar düzeltildi.
* Gelişmiş günlük kaydı doğrudan modunda.
* Geliştirilmiş bağlantı yönetimi.

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0
* Doğrudan modu bağlantı Available(GA) genel kullanıma sunulmuştur. Doğrudan modu bağlantısı kullanan bir örnek için bkz. [azure cosmosdb java](https://github.com/Azure/azure-cosmosdb-java) GitHub deposu.
* QueryMetrics desteği eklendi.
* Java.util.Collection sipariş java.util.List yerine kabul etmek önemli kabul API'leri değiştirildi. Artık ConnectionPolicy#getPreferredLocations() JsonSerialization ve PartitionKey(.) listesini kabul eder.

### <a name="a-name240-beta-1240-beta-1"></a><a name="2.4.0-beta-1"/>2.4.0-Beta-1
* Doğrudan modu bağlantısı için destek eklendi.
* Java.util.Collection sipariş java.util.List yerine kabul etmek önemli kabul API'leri değiştirildi.
  Artık ConnectionPolicy#getPreferredLocations() JsonSerialization ve PartitionKey(.) listesini kabul eder.
* Ağ geçidi modunda belge sorgu için oturumu düzeltildi.
* Yükseltme bağımlılıkları (netty 0.4.20 [github #79](https://github.com/Azure/azure-cosmosdb-java/issues/79), RxJava 1.3.8).

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Çok büyük sorgu yanıtları işleme giderir.
* Kaynak belirteci işleme istemci başlatılırken giderir ([github #78](https://github.com/Azure/azure-cosmosdb-java/issues/78)).
* Yükseltme savunmasız bağımlılık jackson veri bağlama ([github #77](https://github.com/Azure/azure-cosmosdb-java/pull/77)).

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
| [2.4.3](#2.4.3) |5 Mart 2019|--- |
| [2.4.2](#2.4.2) |1 Mart'ta 2019|--- |
| [2.4.1](#2.4.1) |20 Şubat 2019|--- |
| [2.4.0](#2.4.0) |8 Şubat 2019|--- |
| [2.4.0-Beta-1](#2.4.0-beta-1) |4 Şubat 2019|--- |
| [2.3.1](#2.3.1) |15 Ocak 2019|--- |
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

