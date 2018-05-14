---
title: Hizmet sınırları Azure Search'te | Microsoft Docs
description: Kapasite planlama için kullanılan hizmet sınırları ve istekleri ve yanıtları Azure arama için en fazla sınırlandırır.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: heidist
ms.openlocfilehash: 9fd046efd01281de6d5b46cca37d22a48671b1b2
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="service-limits-in-azure-search"></a>Azure Search hizmet sınırları
Maksimum depolama, iş yükleri ve dizinler, belgeler, miktarda sınırlar ve bağımlı nesneler olup olmadığına göre [Azure Search sağlamak](search-create-service-portal.md) adresindeki **serbest**, **temel**, veya **Standart** fiyatlandırma katmanları.

+ **Ücretsiz** Azure aboneliğinizle birlikte gelen bir çok kiracılı paylaşılan bir hizmettir.

+ **Temel** daha küçük ölçekli üretim iş yükleri için özel bilgi işlem kaynakları sağlar.

+ **Standart** daha fazla depolama ve işleme kapasiteye sahip ayrılmış makinelerde her düzeyde çalışır. Standart gelen dört düzeyler: S1, S2, S3 ve S3 HD.

  S3 yüksek yoğunluklu (S3 HD) belirli iş yükleri için tasarlanan: [çoklu kiracı](search-modeling-multitenant-saas-applications.md) ve büyük miktarlarda küçük dizinleri (dizin başına bir milyon belge, hizmeti başına üç bin dizinler). Bu katman sağlamaz [dizin oluşturucu özelliği](search-indexer-overview.md). S3 HD üzerinde veri alımı dizin kaynağından veri göndermek için API çağrılarını kullanarak anında iletme yaklaşım yararlanın gerekir. 

> [!NOTE]
> Bir hizmeti belirli bir katman sağlanır. Kapasite sağlamak için katmanları atlama (hiçbir yerinde yükseltme yoktur) yeni bir hizmet sağlama içerir. Daha fazla bilgi için bkz: [bir SKU katmanı seçin veya](search-sku-tier.md). Zaten sağlanan hizmet kapasitesiyle ayarlama hakkında daha fazla bilgi için bkz: [ölçeklendirme sorgu ve dizin oluşturma iş yükleri için kaynak düzeylerini](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Depolama sınırları
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Dizin sınırları

| Kaynak | Ücretsiz | Temel&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD |
| -------- | ---- | ------------------- | --- | --- | --- | --- |
| En fazla dizin |3 |5 veya 15 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |
| Dizin başına en fazla alan |1000 |100 |1000 |1000 |1000 |1000 |
| Dizin başına en fazla Puanlama profilleri |100 |100 |100 |100 |100 |100 |
| Profil başına en fazla işlevleri |8 |8 |8 |8 |8 |8 |

<sup>1</sup> geç 2017 15 dizinleri, veri kaynakları ve dizin oluşturucular artan bir sınırı oluşturduktan sonra oluşturulan temel Hizmetleri. Daha önce oluşturduğunuz Hizmetleri 5 sahiptir. Temel katman yalnızca SKU dizin başına 100 alanlarının alt sınırına sahip olur.

## <a name="document-limits"></a>Belge sınırları 

Çoğu bölgede Azure fiyatlandırma katmanlarına (temel, S1, S2, S3, S3 HD) arama tüm hizmetler için sınırsız belge sayısını oluşturmuş Kasım/aralık 2017 sonra. Bu bölüm, burada sınırları uygulamak bölgeler ve hizmet etkilenir olup olmadığını belirleme tanımlar. 

Hizmetinizi belge limitleri olup olmadığını belirlemek için hizmetinizin genel bakış sayfasında kullanımı kutucuğu denetleyin. Belge sayıları olan sınırsız ya da konu katmanını temel alan bir sınırı.

  ![Kullanımı kutucuğu](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-and-services-having-document-limits"></a>Bölgeler ve belge limitleri sahip Hizmetleri

Sınırları sahip Hizmetleri geç 2017 önce ya da oluşturulan veya Azure arama hizmetleri barındırmak için kapasite alt kümeleri kullanarak veri merkezlerindeki çalışmıyor. Etkilenen veri merkezleri aşağıdaki bölgelerde şunlardır:

+ Avustralya Doğu
+ Doğu Asya
+ Orta Hindistan
+ Japonya Batı
+ Batı Orta ABD

Hizmetlerde belge limitleri tabi aşağıdaki maksimum sınırları Uygula:

|  Ücretsiz | Temel | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10,000 |1 milyon |Bölüm başına 15 milyon veya hizmet başına 180 milyon |Bölüm başına 60 milyon veya hizmet başına 720 milyon |Bölüm başına 120 milyon veya hizmet başına 1.4 milyar |Dizin başına 1 milyon veya bölüm başına 200 milyon |

> [!Note] 
> Geç 2017 sonra oluşturulan S3 yüksek yoğunluklu Hizmetleri için 200 milyon belgenin bölüm başına 1 milyondan fazla belge başına dizin sınırı kalır ancak kaldırılmıştır.


### <a name="document-size-limits-per-api-call"></a>API çağrısı başına belge boyutu sınırları

Bir dizin API çağrılırken en fazla belge yaklaşık 16 MB boyutudur.

Belge, aslında bir dizin API istek gövdesi boyutu sınırı boyutudur. Dizin API için aynı anda birden çok belge toplu geçirebilirsiniz olduğundan, boyut sınırını gerçekçi kaç belgeleri toplu işlemde bağlıdır. Bir toplu işin için tek bir belgenin, JSON 16 MB maksimum belge boyutudur.

Belge boyutu tutun, sorgulanabilir olmayan verileri istekten dışlamak unutmayın. Görüntüleri ve diğer ikili veriler doğrudan sorgulanabilir değildir ve dizinde saklanan döndürmemelidir. Arama sonuçlarında sorgulanabilir olmayan verilerini tümleştirmek için bir URL referansını kaynağa depolar yapılamayan bir alan tanımlayın.

## <a name="indexer-limits"></a>Dizin Oluşturucu sınırları

Geç 2017 sonra oluşturulan temel Hizmetleri 15 dizinleri, veri kaynakları, skillsets ve dizin oluşturucular daha yüksek bir sınıra sahiptir.

| Kaynak | Ücretsiz&nbsp;<sup>1</sup> | Temel&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|
| -------- | ----------------- | ----------------- | --- | --- | --- | --- |
| En fazla dizin oluşturucu |3 |5 veya 15|50 |200 |200 |Yok |
| En fazla veri kaynağı |3 |5 veya 15 |50 |200 |200 |Yok |
| En fazla skillsets |3 |5 veya 15 |50 |200 |200 |Yok |
| Çağrı başına en fazla dizin yükleme |10.000 belgeleri |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |Yok |
| En fazla çalışma süresini | 1-3 dakika |24 saat |24 saat |24 saat |24 saat |Yok  |
| BLOB dizin oluşturucu: en fazla blob boyutu, MB |16 |16 |128 |256 |256 |Yok  |
| BLOB dizin oluşturucu: blob üzerinden ayıkladığınız içeriği en fazla karakter |32,000 |64,000 |4 milyon |4 milyon |4 milyon |Yok |

<sup>1</sup> ücretsiz hizmetlere sahip dizin oluşturucu maksimum yürütme süresi 3 dakika blob kaynaklarının ve diğer tüm veri kaynakları için 1 dakika.

<sup>2</sup> geç 2017 15 dizinleri, veri kaynakları ve dizin oluşturucular artan bir sınırı oluşturduktan sonra oluşturulan temel Hizmetleri. Daha önce oluşturduğunuz Hizmetleri 5 sahiptir.

<sup>3</sup> S3 HD Hizmetleri dizin oluşturucu desteği dahil değildir.

## <a name="queries-per-second-qps"></a>Sorgular / saniye (QPS)

QPS tahminleri bağımsız olarak her müşteri tarafından geliştirilmiş olmalıdır. Dizin boyutu ve karmaşıklığı, sorgu boyutu ve karmaşıklığı ve trafik miktarı QPS, birincil determinantlar var. Etkenlerden bilinmeyen olduğunda anlamlı tahminleri sunmak için bir yolu yoktur.

Tahminler özel kaynakları (temel ve standart katmanları) üzerinde çalışan hizmetleri üzerinde hesaplandığında daha tahmin edilebilir. Daha fazla parametre üzerinde denetime sahip olduğundan daha fazla QPS yakından tahmin edebilirsiniz. Yaklaşım tahmin etme hakkında yönergeler için bkz [Azure Search performans ve en iyi duruma getirme](search-performance-optimization.md).

## <a name="api-request-limits"></a>API istek sınırları
* İstek başına 16 MB maksimum <sup>1</sup>
* 8 KB URL uzunluğu üst sınırı
* Dizinin toplu iş başına en fazla 1000 belge karşıya yükler, birleştirir veya siler
* $Orderby tümcesinde en çok 32 alanları
* En fazla arama terimi 32.766 bayt sayısı (2 bayt eksi 32 KB) UTF-8 ile kodlanmış metnin boyutudur

<sup>1</sup> olarak Azure arama, bir istek gövdesi içeriği tek tek alanların veya teorik sınırları sınırlı olduğu değil koleksiyonları pratik bir sınırı etkileyici 16 MB'lık üst sınır tabidir (bkz: [desteklenen veri türleri](https://msdn.microsoft.com/library/azure/dn798938.aspx) alan birleşim ve kısıtlamaları hakkında daha fazla bilgi için).

## <a name="api-response-limits"></a>API yanıtını sınırları
* Döndürülen arama sonuçlarını sayfa başına en fazla 1000 belge
* Önermek API istek döndürülen en fazla 100 önerileri

## <a name="api-key-limits"></a>API anahtarı sınırları
Api anahtarlarından hizmetini kimlik doğrulaması için kullanılır. İki tür vardır. Yönetici anahtarları istek başlığında belirtilen ve hizmetine tam okuma-yazma erişimi verin. Sorgu anahtarları URL'SİNDE belirtilen ve genellikle istemci uygulamaları için Dağıtılmış salt okunur.

* En fazla hizmeti başına 2 yönetici anahtarları
* En fazla hizmeti başına 50 sorgu anahtarları
