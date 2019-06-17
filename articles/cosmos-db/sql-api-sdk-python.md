---
title: 'Azure Cosmos DB: SQL Python API, SDK ve kaynakları'
description: Tüm SQL Python API'si ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB Python SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: reference
ms.date: 11/29/2018
ms.author: sngun
ms.openlocfilehash: 9903339cbf0958893fb0d11a8c1b6ab7d156aae7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60626800"
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB için Python SDK'sı SQL API'si için: Sürüm Notları ve kaynakları
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

| |  |
|---|---|
|**SDK'sını indirin**|[PyPI](https://pypi.org/project/azure-cosmos)|
|**API belgeleri**|[Python API başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cosmos/?view=azure-python)|
|**SDK yükleme yönergeleri**|[Python SDK'sını yükleme yönergeleri](https://github.com/Azure/azure-cosmos-python)|
|**SDK'sı için katkıda bulunan**|[GitHub](https://github.com/Azure/azure-cosmos-python)|
|**Kullanmaya başlama**|[Python SDK'sı ile çalışmaya başlama](sql-api-python-application.md)|
|**Geçerli desteklenen platform**|[Python 2.7](https://www.python.org/downloads/) ve [Python 3.5](https://www.python.org/downloads/)|

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name302302"></a><a name="3.0.2"/>3.0.2
* MultiPolygon veri türü için destek eklendi
* Hata düzeltmesi oturumunda yeniden deneme ilkesi okuyun
* Taban 64 dizeleri kod çözme sırasında hatalı doldurma sorunlarında hata düzeltmesi

### <a name="a-name301301"></a><a name="3.0.1"/>3.0.1
* Hata düzeltmesi olarak LocationCache
* Hata düzeltmesi uç noktası yeniden deneme mantığı
* Sabit belgeleri

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0
* Çok bölgeli yazma desteği.
* Namespace azure.cosmos için değiştirildi.
* Koleksiyon ve belge kavramlar yeniden adlandırılmış kapsayıcı ve öğesi, document_client cosmos_client için yeniden adlandırıldı. 

### <a name="a-name233233"></a><a name="2.3.3"/>2.3.3
* Proxy için destek eklendi
* Değişiklik akışı okuma desteği eklendi
* Koleksiyon kotası üst bilgileri için destek eklendi
* Sorun büyük oturumu için hata düzeltmesi belirteçler
* Hata düzeltmesi ReadMedia API için
* Bölüm anahtar aralığı önbelleğinde hata düzeltmesi

### <a name="a-name232232"></a><a name="2.3.2"/>2.3.2
* Varsayılan yeniden deneme bağlantı sorunları için destek eklendi.

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Güncelleştirilmiş belgeleri başvurmak yerine Azure DocumentDB, Azure Cosmos DB için.

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Bu SDK sürüm Merkezi'nden Azure Cosmos DB Emulator kullanılabilir en son sürümünü gerektirir. https://aka.ms/cosmosdb-emulator.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Hata düzeltmesi için birleşik bir sözlük.
* Kaynak bağlantısını eğik çizgi kırpma için hata düzeltmesi.
* Unicode kodlaması için testler eklendi.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için destek eklendi.
* Cosmos DB öykünücüsüne karşı çalıştırırken SSL doğrulama devre dışı bırakmak için bir seçenek eklenmiştir.
* Bağımlı istekleri modülü tam olarak 2.10.0 olmasını kısıtlaması kaldırıldı.
* Bölümlenmiş koleksiyonlardan 10,100 RU/sn 2500 RU/sn için en düşük aktarım hızını düşürdü.
* Saklı yordam yürütme sırasında betik günlük kaydını etkinleştirmek için destek eklendi.
* REST API sürümü indirgenmesine ' 2017-01-19' Bu sürümle birlikte.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Belge yorumlarını editoryal değişiklikler.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Python 3.5 için destek eklendi.
* Bir istek modülü kullanarak bağlantı havuzu için destek eklendi.
* Oturum tutarlılığı için destek eklendi.
* Bölümlenmiş koleksiyonlar için üst/ORDERBY sorgular için destek eklendi.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Eklenen yeniden deneme ilkesi desteği için daraltılmış istekler. (Daraltılmış istekler bir istek oranı çok büyük özel durum, hata kodu 429 alırsınız.) Hata kodu 429 karşılaşıldığında, varsayılan olarak, Azure Cosmos DB dokuz kez her istek için yanıt üst bilgisi retryAfter sürede uygularken yeniden dener. Yeniden denemeler arasında sunucu tarafından döndürülen retryAfter zaman yoksay istiyorsanız bir sabit yeniden deneme zaman aralığını artık RetryOptions özelliğinin bir parçası olarak ConnectionPolicy nesnede ayarlanabilir. Azure Cosmos DB artık en fazla (yeniden deneme sayısı bağımsız olarak) çalıştırıldığında ve hata kodu 429 yanıtı döndüren her istek için 30 saniye bekler. Bu süre de ConnectionPolicy nesnesinde RetryOptions özelliğinde geçersiz kılınabilir.
* Yanıt üstbilgilerini kısıtlama belirtmek için her istekte yeniden deneme sayısı ve isteği yeniden denemeler arasında beklenen toplu süresi olarak cosmos DB artık x-ms-kısıtlama-yeniden-count ve x-ms-throttle-retry-wait-time-ms döndürür.
* RetryPolicy sınıfı ve document_client sınıf üzerinde kullanıma sunulan karşılık gelen özelliği (retry_policy) kaldırıldı ve bunun yerine bazı geçersiz kılmak için kullanılan ConnectionPolicy sınıfı RetryOptions özelliği kullanıma sunan bir RetryOptions sınıfı eklendi Varsayılan yeniden deneme seçenekleri.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Çoklu bölge veritabanı hesapları için destek eklendi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için zaman Live(TTL) özelliği için destek eklendi.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Hata düzeltmeleri, özel karakterler bölüm anahtar yoluna izin vermek için sunucu tarafı bölümleme için ilgili.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Uygulanan [bölümlenmiş koleksiyonları](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Karma & aralığı Ekle Çözümleyicileri parçalara ayırma uygulamaları ile birden çok bölümde yardımcı olmak üzere bölümleme.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert uygulayın. Upsert özelliğini desteklemek için eklenen yeni UpsertXXX yöntemleri.
* Kimlik tabanlı yönlendirme uygulayın. Genel API değişiklik, tüm değişiklikleri iç.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Jeo-uzamsal dizini destekler.
* Tüm kaynaklar için kimlik özelliği doğrular. Kaynaklar için kimlikleri içeremez?, /, #, \, karakterler veya boşluk ile bitmelidir.
* Yeni üst bilgi "dizin dönüştürme ilerleme durumu" için ResourceResponse ekler.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Ara sunucu bağlantısı destekler.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, olduğundan bu nedenle önerilir, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz. 

Cosmos DB devre dışı bırakılan bir SDK'sını kullanarak yapılan tüm istekleri hizmet tarafından reddedilir.

> [!WARNING]
> Python için Azure SQL SDK'sının sürümden önceki tüm sürümler **1.0.0** kullanımdan kaldırılmıştır **29 Şubat 2016**. 
> 
> 

<br/>

| Version | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [3.0.2](#3.0.2) |15 Kasım 2018 |--- |
| [3.0.1](#3.0.1) |04 Ekim 2018 |--- |
| [2.3.3](#2.3.3) |08 Eylül 2018 |--- |
| [2.3.2](#2.3.2) |08 Mayıs 2018 |--- |
| [2.3.1](#2.3.1) |21 aralık 2017 |--- |
| [2.3.0](#2.3.0) |10 Kasım 2017 |--- |
| [2.2.1](#2.2.1) |Sep 29, 2017 |--- |
| [2.2.0](#2.2.0) |10 Mayıs 2017 |--- |
| [2.1.0](#2.1.0) |01 Mayıs 2017 |--- |
| [2.0.1](#2.0.1) |30 Ekim 2016 |--- |
| [2.0.0](#2.0.0) |29 Eylül 2016 |--- |
| [1.9.0](#1.9.0) |07 Temmuz 2016 |--- |
| [1.8.0](#1.8.0) |14 Haziran 2016 |--- |
| [1.7.0](#1.7.0) |26 Nisan 2016 |--- |
| [1.6.1](#1.6.1) |08 Nisan 2016 |--- |
| [1.6.0](#1.6.0) |29 Mart 2016 |--- |
| [1.5.0](#1.5.0) |03 Ocak 2016 |--- |
| [1.4.2](#1.4.2) |06 Ekim 2015 |--- |
| 1.4.1 |06 Ekim 2015 |--- |
| [1.2.0](#1.2.0) |06 Ağustos 2015 |--- |
| [1.1.0](#1.1.0) |09 Temmuz 2015 |--- |
| [1.0.1](#1.0.1) |25 Mayıs 2015 |--- |
| [1.0.0](#1.0.0) |07 Nisan 2015 |--- |
| 0.9.4-prelease |14 Ocak 2015 |29 Şubat 2016 |
| 0.9.3-prelease |09 aralık 2014 |29 Şubat 2016 |
| 0.9.2-prelease |25 Kasım 2014 |29 Şubat 2016 |
| 0.9.1-prelease |23 Eylül 2014'ten |29 Şubat 2016 |
| 0.9.0-prelease |21 Ağustos 2014 |29 Şubat 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası. 

