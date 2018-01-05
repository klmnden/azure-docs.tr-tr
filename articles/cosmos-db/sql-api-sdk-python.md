---
title: "Azure Cosmos DB: SQL Python API, SDK & kaynakları | Microsoft Docs"
description: "SQL Python API ve yayın tarih, sona erme tarihlerini ve her Azure Cosmos DB Python SDK'sı sürüm arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 1/4/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6801c5b62be08e4dcb32ad342b15e9ad3f3e20a8
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB Python SDK: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik besleme](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

<table>

<tr><td>**SDK'sını indirin**</td><td>[Pypı](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**API belgeleri**</td><td>[Python API başvuru belgeleri](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**SDK yükleme yönergeleri**</td><td>[Python SDK yükleme yönergeleri](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**SDK katkıda bulunan**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[Python SDK'sı ile çalışmaya başlama](sql-api-python-application.md)</td></tr>

<tr><td>**Geçerli desteklenen platformlar**</td><td>[Python 2.7](https://www.python.org/downloads/) ve [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları
### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Güncelleştirilmiş belgeleri başvuru Azure DocumentDB yerine Azure Cosmos DB.

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Bu SDK sürümü https://aka.ms/cosmosdb-emulator Merkezi'nden Azure Cosmos DB öykünücüsü kullanılabilir en son sürümünü gerektirir.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Birleşik sözlüğü için hata düzeltmesi.
* Eğik çizgi kaynak bağlantıda kırpma için hata düzeltmesi.
* Unicode kodlama için testler eklendi.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) desteği eklendi.
* SSL doğrulama Cosmos DB öykünücüsüne karşı çalıştırırken devre dışı bırakmak için bir seçenek eklenmiştir.
* Tam olarak 2.10.0 olması için bağımlı istekleri modülü kısıtlama kaldırıldı.
* En düşük işleme 2500 RU/s 10,100 RU/s bölümlenmiş koleksiyonlar üzerinde düşürdü.
* Saklı yordam yürütme sırasında betik günlüğünü etkinleştirme için destek eklendi.
* REST API sürümü için 2017 ' indirgenmesine-01-19' Bu sürümde.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Belge açıklamaları için düzenleme değişiklikleri.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Python 3.5 desteği eklendi.
* İstekleri modülünü kullanarak bağlantı havuzu için destek eklendi.
* Oturum tutarlılığı desteği eklendi.
* Bölümlenmiş koleksiyonlar için üst/ORDERBY sorgular için destek eklendi.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Daraltılmış istekleri için eklenen yeniden deneme ilkesi desteği. (Daraltılmış isteklerini bir istek oranı çok büyük özel durumu, hata kodu 429 alır.) Hata kodu 429 karşılaşıldığında, varsayılan olarak, Azure Cosmos DB dokuz kez her istek için yanıt üst bilgisi retryAfter zamanında uygularken yeniden dener. Yeniden denemeler arasında sunucu tarafından döndürülen retryAfter zaman yoksay istiyorsanız sabit yeniden deneme zaman aralığını şimdi RetryOptions özelliğinin bir parçası olarak ConnectionPolicy nesne üzerinde ayarlanabilir. Azure Cosmos DB şimdi en fazla (bağımsız olarak yeniden deneme sayısı) kısıtlanan ve 429 hata koduyla yanıt döndüren her istek için 30 saniye bekler. Bu süre de ConnectionPolicy nesnesindeki RetryOptions özelliği geçersiz kılınabilir.
* Yanıt üstbilgilerini kısıtlama belirtmek için her istekte yeniden deneme sayısı ve isteği yeniden denemeler arasında beklenen toplam süre gibi cosmos DB x-ms-kısıtlama-yeniden deneme-sayısı ve x-ms-throttle-retry-wait-time-ms şimdi döndürür.
* Bunun yerine bazı geçersiz kılmak için kullanılan ConnectionPolicy sınıfı RetryOptions özellikte gösterme RetryOptions sınıfı sunulan ve RetryPolicy sınıfı ve document_client sınıf üzerinde gösterilen karşılık gelen özelliği (retry_policy) kaldırıldı Varsayılan yeniden deneme seçeneklerini.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Bölgeli veritabanı hesaplarını desteği eklendi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için zaman için Live(TTL) özelliği için destek eklendi.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Hata düzeltmeleri özel karakterler bölüm anahtar yoluna izin vermek için sunucu tarafı bölümleme için ilgili.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Karma & aralığı Ekle Bölüm parçalama uygulamalarla arasında birden çok bölüm yardımcı olmak için Çözümleyicileri.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Upsert uygulayın. Upsert özelliğini desteklemek için eklenen yeni UpsertXXX yöntemleri.
* Temel kimlik yönlendirme uygulayın. Genel API değişiklik, tüm değişiklikleri iç.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Jeo-uzamsal dizin destekler.
* ID özelliği tüm kaynaklar için doğrular. Kaynaklar için kimlikleri içeremez?, /, #, \, karakter veya boşluk ile bitmelidir.
* Yeni Üstbilgi "dizini dönüştürme ilerleme durumu" için ResourceResponse ekler.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Proxy bağlantı destekler.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, olduğundan bu nedenle önerilir, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz. 

Cosmos devre dışı bırakılan bir SDK'sını kullanarak DB'de herhangi bir istek hizmeti tarafından reddedilir.

> [!WARNING]
> Python için Azure SQL SDK'sı tüm sürümleri sürümden önceki **1.0.0** üzerinde Çekildi **29 Şubat 2016**. 
> 
> 

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.3.1](#2.3.1) |21 aralık 2017 |--- |
| [2.3.0](#2.3.0) |10 Kasım 2017 |--- |
| [2.2.1](#2.2.1) |Eylül 29, 2017 |--- |
| [2.2.0](#2.2.0) |10 Mayıs 2017 |--- |
| [2.1.0](#2.1.0) |01 May 2017 |--- |
| [2.0.1](#2.0.1) |30 Ekim 2016 |--- |
| [2.0.0](#2.0.0) |29 Eylül 2016 |--- |
| [1.9.0](#1.9.0) |07 Temmuz 2016 |--- |
| [1.8.0](#1.8.0) |14 Haziran 2016 |--- |
| [1.7.0](#1.7.0) |26 Nisan 2016 |--- |
| [1.6.1](#1.6.1) |08 Nisan 2016 |--- |
| [1.6.0](#1.6.0) |29 Mart 2016 |--- |
| [1.5.0](#1.5.0) |03 Ocak 2016 |--- |
| [1.4.2](#1.4.2) |06 Ekim 2015 |--- |
| [1.4.1](#1.4.1) |06 Ekim 2015 |--- |
| [1.2.0](#1.2.0) |06 Ağustos 2015 |--- |
| [1.1.0](#1.1.0) |09 Temmuz 2015 |--- |
| [1.0.1](#1.0.1) |25 Mayıs 2015 |--- |
| [1.0.0](#1.0.0) |07 Nisan 2015 |--- |
| 0.9.4-prelease |14 Ocak 2015 |29 Şubat 2016 |
| 0.9.3-prelease |09 aralık 2014 |29 Şubat 2016 |
| 0.9.2-prelease |25 Kasım 2014 |29 Şubat 2016 |
| 0.9.1-prelease |23 Eylül 2014 |29 Şubat 2016 |
| 0.9.0-prelease |21 Ağustos 2014 |29 Şubat 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası. 

