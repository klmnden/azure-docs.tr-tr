---
title: 'Azure Cosmos DB: SQL Node.js API, SDK & kaynakları | Microsoft Docs'
description: SQL Node.js API ve yayın tarih, sona erme tarihlerini ve her Azure Cosmos DB Node.js SDK'sı sürüm arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
author: rnagpal
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: reference
ms.date: 5/3/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 94d60325fa53668628ddefa68e8ce313c4fa75c6
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797854"
---
# <a name="azure-cosmos-db-nodejs-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB Node.js SDK: sürüm notları ve kaynakları
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

<table>

<tr><td>**SDK'sını indirin**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td>**API belgeleri**</td><td>[Node.js API başvuru belgeleri](https://docs.microsoft.com/javascript/api/documentdb/?view=azure-node-latest)</td></tr>

<tr><td>**SDK yükleme yönergeleri**</td><td>[Yükleme yönergeleri](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td>**SDK katkıda bulunan**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td>**Örnekler**</td><td>[Node.js kod örnekleri](sql-api-nodejs-samples.md)</td></tr>

<tr><td>**Başlangıç eğitmeni**</td><td>[Node.js SDK'sı ile çalışmaya başlama](sql-api-nodejs-get-started.md)</td></tr>

<tr><td>**Web uygulaması Öğreticisi**</td><td>[Azure Cosmos DB kullanarak bir Node.js web uygulaması oluşturma](sql-api-nodejs-application.md)</td></tr>

<tr><td>**Geçerli desteklenen platformlar**</td><td> 
[Node.js v6.x](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları

### <a name="1.14.4"/>1.14.4</a>
* Sabit npm belgeleri.

### <a name="1.14.3"/>1.14.3</a>
* Varsayılan deneme bağlantı sorunları için destek eklendi.
* Koleksiyon değişiklik okumak için destek eklendi akış.
* Zaman zaman "okuma oturumun kullanılamıyor" neden sabit oturum tutarlılığı hata.
* Sorgu ölçümleri desteği eklendi.
* Değiştirilen http aracısının en fazla bağlantı sayısı.

### <a name="1.14.2"/>1.14.2</a>
* Güncelleştirilmiş belgeleri başvuru Azure DocumentDB yerine Azure Cosmos DB.
* ConnectionPolicy proxyUrl ayarında desteği eklendi.

### <a name="1.14.1"/>1.14.1</a>
* Büyük küçük harfe duyarlı dosya sistemleri için ikincil düzeltme.

### <a name="1.14.0"/>1.14.0</a>
* Oturum tutarlılığı için destek ekler.
* Bu SDK sürümü Merkezi'nden Azure Cosmos DB öykünücüsü kullanılabilir en son sürümünü gerektirir https://aka.ms/cosmosdb-emulator.

### <a name="1.13.0"/>1.13.0</a>
* Bölüm sorguları proofed Böl.
* Baştaki ve sondaki eğik çizgi (ve karşılık gelen testleri) içeren kaynak bağlantı için desteklenen ekler.

### <a name="1.12.2"/>1.12.2</a>
*   Sabit npm belgeleri.

### <a name="1.12.1"/>1.12.1</a>
* Bir hata, burada ilgili belgelerini özel Unicode karakterler (LS, PS) b executeStoredProcedure içinde sabit.
* Bölüm anahtarı Unicode karakterler içeren belgeleri işlemedeki hatanın düzeltildiğini.
* Sabit koleksiyonlar ile ad ortamı oluşturma desteği. Github sorunu #114.
* İzni yetkilendirme belirtecini sabit desteği. Github sorunu #178.

### <a name="1.12.0"/>1.12.0</a>
* Yeni bir desteği eklendi [tutarlılık düzeyi](consistency-levels.md) ConsistentPrefix çağrılır.
* UriFactory desteği eklendi.
* Unicode desteği hatanın düzeltildiğini. GitHub sorunu #171.

### <a name="1.11.0"/>1.11.0</a>
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) desteği eklendi.
* Çapraz bölüm sorgular için paralellik derecesi denetleme seçenek eklenmiştir.
* SSL doğrulama Azure Cosmos DB öykünücüsüne karşı çalıştırırken devre dışı bırakmak için seçenek eklenmiştir.
* En düşük işleme 2500 RU/s 10,100 RU/s bölümlenmiş koleksiyonlar üzerinde düşürdü.
* Tek bölümlü bir koleksiyon için devamlılık belirteci hata sabit. Github sorunu #107.
* 0 tek param işlemedeki executeStoredProcedure hata sabit. Github sorunu #155.

### <a name="1.10.2"/>1.10.2</a>
* SDK sürümü dahil etmek için sabit user-agent üstbilgisi.
* Küçük kod temizleme.

### <a name="1.10.1"/>1.10.1</a>
* SSL doğrulama SDK emulator(hostname=localhost) hedeflemek için kullanırken devre dışı bırakılıyor.
* Saklı yordam yürütme sırasında betik günlüğünü etkinleştirme için destek eklendi.

### <a name="1.10.0"/>1.10.0</a>
* Bölüm paralel sorgular çapraz eklenen desteği.
* ÜST/ORDER BY sorguları bölümlenmiş koleksiyonlar için desteği eklendi.

### <a name="1.9.0"/>1.9.0</a>
* Daraltılmış istekleri için eklenen yeniden deneme ilkesi desteği. (Daraltılmış isteklerini bir istek oranı çok büyük özel durumu, hata kodu 429 alır.) Hata kodu 429 karşılaşıldığında, varsayılan olarak, Azure Cosmos DB dokuz kez her istek için yanıt üst bilgisi retryAfter zamanında uygularken yeniden dener. Yeniden denemeler arasında sunucu tarafından döndürülen retryAfter zaman yoksay istiyorsanız sabit yeniden deneme zaman aralığını şimdi RetryOptions özelliğinin bir parçası olarak ConnectionPolicy nesne üzerinde ayarlanabilir. Azure Cosmos DB şimdi en fazla (bağımsız olarak yeniden deneme sayısı) kısıtlanan ve 429 hata koduyla yanıt döndüren her istek için 30 saniye bekler. Bu süre de ConnectionPolicy nesnesindeki RetryOptions özelliği geçersiz kılınabilir.
* Yanıt üstbilgilerini kısıtlama belirtmek için her istekte yeniden deneme sayısı ve isteği yeniden denemeler arasında beklenen toplam süre gibi cosmos DB x-ms-kısıtlama-yeniden deneme-sayısı ve x-ms-throttle-retry-wait-time-ms şimdi döndürür.
* Bazı varsayılan yeniden deneme seçeneklerini geçersiz kılmak için kullanılan ConnectionPolicy sınıfı RetryOptions özellikte gösterme RetryOptions sınıfı eklendi.

### <a name="1.8.0"/>1.8.0</a>
* Bölgeli veritabanı hesaplarını desteği eklendi.

### <a name="1.7.0"/>1.7.0</a>
* Belgeler için zaman için Live(TTL) özelliği için destek eklendi.

### <a name="1.6.0"/>1.6.0</a>
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md).

### <a name="1.5.6"/>1.5.6</a>
* Burada, hatalı concat sonuçları nedeniyle bağlantıları döndürdü olmayan sabit RangePartitionResolver.resolveForRead hata.

### <a name="1.5.5"/>1.5.5</a>
* HashParitionResolver resolveForRead() sabit: ne zaman sağlanan hiçbir bölüm anahtarı atma tüm kayıtlı bağlantıların listesini döndürme yerine bir özel durum.

### <a name="1.5.4"/>1.5.4</a>
* Sorunu giderir [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -ayrılmış HTTPS aracısı: Azure Cosmos DB amacıyla genel Aracısı değiştirme kaçının. Ayrılmış bir aracı tüm lib'ın istekleri için kullanın.

### <a name="1.5.3"/>1.5.3</a>
* Sorunu giderir [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - düzgün işleyecek medya kimlikleri tire.

### <a name="1.5.2"/>1.5.2</a>
* Sorunu giderir [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter dinleyicisi sızıntısı uyarı.

### <a name="1.5.1"/>1.5.1</a>
* Sorunu giderir [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -klasör karma büyük küçük harfe duyarlı sistemler için karma yeniden adlandırın.

### <a name="1.5.0"/>1.5.0</a>
* Karma & aralığı bölüm çözümleyiciler ekleyerek parçalama destek uygular.

### <a name="1.4.0"/>1.4.0</a>
* Upsert uygulayın. DocumentClient yeni upsertXXX yöntemleri.

### <a name="1.3.0"/>1.3.0</a>
* Sürüm numaraları diğer SDK ile hizalama getirilecek atlandı.

### <a name="1.2.2"/>1.2.2</a>
* Bölünmüş Q yeni bir havuz için sarmalayıcı taahhüt eder.
* Paket dosyası npm kayıt defteri için güncelleştirin.

### <a name="1.2.1"/>1.2.1</a>
* Implements tabanlı yönlendirme kimliği.
* Sorunu giderir [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -geçerli özellik yöntemi current() ile çakışıyor.

### <a name="1.2.0"/>1.2.0</a>
* Jeo-uzamsal dizin desteği eklendi.
* ID özelliği tüm kaynaklar için doğrular. Kaynaklar için kimlikleri içeremez?, /, #, &#47; &#47;, karakter veya boşluk ile bitmelidir.
* Yeni Üstbilgi "dizini dönüştürme ilerleme durumu" için ResourceResponse ekler.

### <a name="1.1.0"/>1.1.0</a>
* V2 dizin oluşturma ilkesini uygular.

### <a name="1.0.3"/>1.0.3</a>
* Sorun [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - uygulanan eslint grunt çekirdek yapılandırmalarını ve SDK veriyoruz.

### <a name="1.0.2"/>1.0.2</a>
* Sorun [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -öneriler sarmalayıcı hata üstbilgiyle içermez.

### <a name="1.0.1"/>1.0.1</a>
* Sorgu readConflicts, readConflictAsync ve queryConflicts ekleyerek çakışmaları için uygulanan yeteneği.
* Güncelleştirilmiş API belgeleri.
* Sorun [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync hata.

### <a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihlerini
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, bu nedenle, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz önerilir.

Kullanımdan Kaldırılan SDK Cosmos DB kullanarak herhangi bir istek hizmeti tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [1.14.4](#1.14.4) |03 May 2018 |--- |
| [1.14.3](#1.14.3) |03 May 2018 |--- |
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
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

