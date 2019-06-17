---
title: 'Azure Cosmos DB: SQL Node.js API, SDK ve kaynakları'
description: Tüm SQL Node.js API'si ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB Node.js SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: reference
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: 1cb6889305e5f6bce5728039712a1834dc2e9353
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60626749"
---
# <a name="azure-cosmos-db-nodejs-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB Node.js SDK'sı SQL API'si için: Sürüm Notları ve kaynakları
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

|Resource  |Bağlantı  |
|---------|---------|
|SDK'yı indir  |   [NPM](https://www.npmjs.com/package/@azure/cosmos) 
|API Belgeleri  |  [JavaScript SDK başvuru belgeleri](https://docs.microsoft.com/javascript/api/%40azure/cosmos/?view=azure-node-latest)
|SDK yükleme yönergeleri  |  [Yükleme yönergeleri](https://github.com/Azure/azure-cosmos-js#installation)
|SDK'sı için katkıda bulunan | [GitHub](https://github.com/Azure/azure-cosmos-js/tree/master)
| Örnekler | [Node.js kod örneği](sql-api-nodejs-samples.md)
| Başlangıç Öğreticisi | [JavaScript SDK'sı ile çalışmaya başlama](sql-api-nodejs-get-started.md)
| Web uygulaması Öğreticisi | [Azure Cosmos DB kullanarak bir Node.js web uygulaması oluşturma](sql-api-nodejs-application.md)
| Geçerli desteklenen platform | [Node.js v6.x](https://nodejs.org/en/blog/release/v6.10.3/) : SDK'sı sürüm 2.0.0 ve üzeri gerekli.<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> [Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> [Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 

## <a name="release-notes"></a>Sürüm notları

### <a name="2.0.5"/>2.0.5</a>
* Aracı türü düğümü için bir arabirim ekler. Typescript artık kullanıcınız yüklemek @types/node bağımlılık olarak
* Tercih edilen konumlar artık düzgün şekilde dikkate
* Geliştirici belgelerine katkıda bulunan geliştirmeleri
* Çeşitli yazım düzeltmeleri

### <a name="2.0.4"/>2.0.4</a>
* Düzeltmeleri 2.0.3 sürümünü içinde tanıtılan tanımı sorun türü

### <a name="2.0.3"/>2.0.3</a>
* Kaldırma `big-integer` bağımlılık
* Başvuru yönergeleri AsyncIterable türünün geçin. Typescript kullanıcıları artık kendi "LIB" ayarı özelleştirmeniz gerekir.
* Yazım düzeltmeleri

### <a name="2.0.2"/>2.0.2</a>
* Benioku bağlantıları Düzelt

### <a name="2.0.1"/>2.0.1</a>
* Yeniden deneme arabirim uygulaması Düzelt

### <a name="2.0.0"/>2.0.0</a>
* JavaScript SDK 2.0.0 sürümünün GA
* Çok bölgeli yazma desteği eklendi.

### <a name="2.0.0-3"/>2.0.0-3</a>
* RC1'de 2.0.0 sürümünün JavaScript SDK'ın genel önizlemesi için.
* Üst düzey CosmosClient ve yöntemleri ile yeni nesne modeli, veritabanı ve kapsayıcı öğesi ilgili sınıflar arasında bölün. 
* Destek [taahhüt](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). 
* SDK'sı için TypeScript dönüştürülür.

### <a name="1.14.4"/>1.14.4</a>
* npm belgeleri düzeltildi.

### <a name="1.14.3"/>1.14.3</a>
* Varsayılan yeniden deneme bağlantı sorunları için destek eklendi.
* Koleksiyon değişiklik okumak için destek eklendi akış.
* Aralıklı olarak "okuma oturumun kullanılamıyor" neden sabit oturum tutarlılık hata.
* Sorgu ölçümler için destek eklendi.
* Değiştirilen http aracısının en fazla bağlantı sayısı.

### <a name="1.14.2"/>1.14.2</a>
* Güncelleştirilmiş belgeleri başvurmak yerine Azure DocumentDB, Azure Cosmos DB için.
* ConnectionPolicy proxyUrl ayarı desteği eklendi.

### <a name="1.14.1"/>1.14.1</a>
* Büyük küçük harfe duyarlı dosya sistemleri için küçük düzeltme.

### <a name="1.14.0"/>1.14.0</a>
* Oturum tutarlılığı için destek ekler.
* Bu SDK sürüm Merkezi'nden Azure Cosmos DB Emulator kullanılabilir en son sürümünü gerektirir. https://aka.ms/cosmosdb-emulator.

### <a name="1.13.0"/>1.13.0</a>
* Bölüm sorguları haricindeki bölme.
* Baştaki ve sondaki eğik çizgi (ve ilgili testler) kaynak bağlantısı için desteklenen ekler.

### <a name="1.12.2"/>1.12.2</a>
*   npm belgeleri düzeltildi.

### <a name="1.12.1"/>1.12.1</a>
* Bir hata, burada özel Unicode karakterler (LS, PS) ilgili belgelerini b içinde executeStoredProcedure düzeltildi.
* Bölüm anahtarı Unicode karakter belgelerle işlemedeki hata düzeltildi.
* Sabit koleksiyonlar ile adı medya oluşturma desteği. GitHub sorunu #114.
* Yetkilendirme belirteci izni sabit desteği. GitHub sorunu #178.

### <a name="1.12.0"/>1.12.0</a>
* Yeni bir desteği eklendi [tutarlılık düzeyi](consistency-levels.md) ConsistentPrefix çağrılır.
* UriFactory desteği eklendi.
* Unicode desteği düzeltildi. GitHub sorunu #171.

### <a name="1.11.0"/>1.11.0</a>
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için destek eklendi.
* Çapraz bölüm sorgular için paralellik derecesini denetleme seçeneği eklendi.
* Azure Cosmos DB öykünücüsüne karşı çalıştırırken SSL doğrulama devre dışı bırakmaya yönelik seçenek eklenmiştir.
* Bölümlenmiş koleksiyonlardan 10,100 RU/sn 2500 RU/sn için en düşük aktarım hızını düşürdü.
* Tek bölümlü bir koleksiyon için devamlılık belirteci hata düzeltildi. GitHub sorunu #107.
* Tek parametre 0 işlemedeki executeStoredProcedure hata düzeltildi. GitHub sorunu #155.

### <a name="1.10.2"/>1.10.2</a>
* SDK sürümü dahil etmek için sabit bir kullanıcı aracısı üstbilgisi.
* Küçük kod temizleme.

### <a name="1.10.1"/>1.10.1</a>
* SSL doğrulama emulator(hostname=localhost) hedeflemek için SDK'sı kullanırken devre dışı bırakılıyor.
* Saklı yordam yürütme sırasında betik günlük kaydını etkinleştirmek için destek eklendi.

### <a name="1.10.0"/>1.10.0</a>
* Çapraz bölüm paralel sorgular için eklenen destek.
* ÜST/ORDER BY sorguları bölümlenmiş koleksiyonlar için destek eklendi.

### <a name="1.9.0"/>1.9.0</a>
* Eklenen yeniden deneme ilkesi desteği için daraltılmış istekler. (Daraltılmış istekler bir istek oranı çok büyük özel durum, hata kodu 429 alırsınız.) Hata kodu 429 karşılaşıldığında, varsayılan olarak, Azure Cosmos DB dokuz kez her istek için yanıt üst bilgisi retryAfter sürede uygularken yeniden dener. Yeniden denemeler arasında sunucu tarafından döndürülen retryAfter zaman yoksay istiyorsanız bir sabit yeniden deneme zaman aralığını artık RetryOptions özelliğinin bir parçası olarak ConnectionPolicy nesnede ayarlanabilir. Azure Cosmos DB artık en fazla (yeniden deneme sayısı bağımsız olarak) çalıştırıldığında ve hata kodu 429 yanıtı döndüren her istek için 30 saniye bekler. Bu süre de ConnectionPolicy nesnesinde RetryOptions özelliğinde geçersiz kılınabilir.
* Yanıt üstbilgilerini kısıtlama belirtmek için her istekte yeniden deneme sayısı ve isteği yeniden denemeler arasında beklenen toplu süresi olarak cosmos DB artık x-ms-kısıtlama-yeniden-count ve x-ms-throttle-retry-wait-time-ms döndürür.
* Bazı varsayılan yeniden deneme seçeneklerini geçersiz kılmak için kullanılan ConnectionPolicy sınıfı RetryOptions özelliği gösterme RetryOptions sınıfı eklendi.

### <a name="1.8.0"/>1.8.0</a>
* Çoklu bölge veritabanı hesapları için destek eklendi.

### <a name="1.7.0"/>1.7.0</a>
* Belgeler için zaman Live(TTL) özelliği için destek eklendi.

### <a name="1.6.0"/>1.6.0</a>
* Uygulanan [bölümlenmiş koleksiyonları](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Burada bir hatalı concat sonuçlarının nedeniyle bağlantıları döndürmeden değil RangePartitionResolver.resolveForRead hata düzeltildi.

### <a name="1.5.5"/>1.5.5</a>
* Fixed hashPartitionResolver resolveForRead(): Ne zaman sağlanan hiçbir bölüm anahtarı, kayıtlı tüm bağlantıların listesini döndürmek yerine özel durumu oluşturmaya.

### <a name="1.5.4"/>1.5.4</a>
* Sorunu giderir [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -adanmış HTTPS aracı: Azure Cosmos DB amacıyla genel aracı değiştirme kaçının. Adanmış bir aracı tüm lib'ın istekleri için kullanın.

### <a name="1.5.3"/>1.5.3</a>
* Sorunu giderir [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - düzgün bir şekilde işlemek ortam kimlikleri tirelerin.

### <a name="1.5.2"/>1.5.2</a>
* Sorunu giderir [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter dinleyici sızıntı uyarı.

### <a name="1.5.1"/>1.5.1</a>
* Sorunu giderir [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -klasör karma karma büyük/küçük harfe sistemler için yeniden adlandırın.

### <a name="1.5.0"/>1.5.0</a>
* Karma & aralığı bölüm Çözümleyicileri ekleyerek parçalama destek uygular.

### <a name="1.4.0"/>1.4.0</a>
* Upsert uygulayın. DocumentClient yeni upsertXXX yöntemleri.

### <a name="1.3.0"/>1.3.0</a>
* Sürüm numaraları diğer SDK'lar ile uyumlu duruma getirmek için atlandı.

### <a name="1.2.2"/>1.2.2</a>
* Bölünmüş Q yeni bir havuz için sarmalayıcı sağlar.
* Npm kayıt defteri için paket dosyasını güncelleştirin.

### <a name="1.2.1"/>1.2.1</a>
* Implements tabanlı yönlendirme kimliği.
* Sorunu giderir [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -current özelliğinin yöntemi Current() değeri öğesi ile çakışıyor.

### <a name="1.2.0"/>1.2.0</a>
* Jeo-uzamsal dizin için destek eklendi.
* Tüm kaynaklar için kimlik özelliği doğrular. Kaynaklar için kimlikleri içeremez?, /, #, &#47; &#47;, karakterler veya boşluk ile bitmelidir.
* Yeni üst bilgi "dizin dönüştürme ilerleme durumu" için ResourceResponse ekler.

### <a name="1.1.0"/>1.1.0</a>
* V2 dizin oluşturma ilkesini uygular.

### <a name="1.0.3"/>1.0.3</a>
* Sorunu [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - uygulanan eslint'i grunt çekirdek yapılandırmalarını ve SDK'sı taahhüt.

### <a name="1.0.2"/>1.0.2</a>
* Sorunu [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -hata başlığı gösterir sarmalayıcı dahil değildir.

### <a name="1.0.1"/>1.0.1</a>
* Sorgu readConflicts readConflictAsync ve queryConflicts ekleyerek çakışmaları için uygulanan yeteneği.
* Güncelleştirilmiş API belgeleri.
* Sorunu [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync hata.

### <a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Yayın ve sona erme tarihlerini
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, bu nedenle, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz önerilir.

Devre dışı bırakılan bir SDK'sı Cosmos DB kullanarak tüm istekleri hizmet tarafından reddedilir.

<br/>

| Version | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.0.0-3 (RC)](#2.0.0-3) |2 Ağustos 2018 |--- |
| [1.14.4](#1.14.4) |03 Mayıs 2018 |--- |
| [1.14.3](#1.14.3) |03 Mayıs 2018 |--- |
| [1.14.2](#1.14.2) |21 aralık 2017 |--- |
| [1.14.1](#1.14.1) |10 Kasım 2017 |--- |
| [1.14.0](#1.14.0) |9 Kasım 2017 |--- |
| [1.13.0](#1.13.0) |11 Ekim 2017 |--- |
| [1.12.2](#1.12.2) |10 Ağustos 2017 |--- |
| [1.12.1](#1.12.1) |10 Ağustos 2017 |--- |
| [1.12.0](#1.12.0) |10 Mayıs 2017 |--- |
| [1.11.0](#1.11.0) |16 Mart 2017 |--- |
| [1.10.2](#1.10.2) |27 Ocak 2017 |--- |
| [1.10.1](#1.10.1) |22 aralık 2016 |--- |
| [1.10.0](#1.10.0) |03 Ekim 2016 |--- |
| [1.9.0](#1.9.0) |07 Temmuz 2016 |--- |
| [1.8.0](#1.8.0) |14 Haziran 2016 |--- |
| [1.7.0](#1.7.0) |26 Nisan 2016 |--- |
| [1.6.0](#1.6.0) |29 Mart 2016 |--- |
| [1.5.6](#1.5.6) |08 Mart 2016 |--- |
| [1.5.5](#1.5.5) |02 Şubat 2016 |--- |
| [1.5.4](#1.5.4) |01 Şubat 2016 |--- |
| [1.5.2](#1.5.2) |26 Ocak 2016 |--- |
| [1.5.2](#1.5.2) |22 Ocak 2016 |--- |
| [1.5.1](#1.5.1) |4 Ocak 2016 |--- |
| [1.5.0](#1.5.0) |31 Aralık 2015 |--- |
| [1.4.0](#1.4.0) |06 Ekim 2015 |--- |
| [1.3.0](#1.3.0) |06 Ekim 2015 |--- |
| [1.2.2](#1.2.2) |10 Eylül 2015 |--- |
| [1.2.1](#1.2.1) |15 Ağustos 2015 |--- |
| [1.2.0](#1.2.0) |05 Ağustos 2015 |--- |
| [1.1.0](#1.1.0) |09 Temmuz 2015 |--- |
| [1.0.3](#1.0.3) |04 Haziran 2015 |--- |
| [1.0.2](#1.0.2) |23 Mayıs 2015 |--- |
| [1.0.1](#1.0.1) |15 Mayıs 2015 |--- |
| [1.0.0](#1.0.0) |08 Nisan 2015 |--- |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası.

