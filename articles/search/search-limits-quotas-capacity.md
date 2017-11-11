---
title: "Hizmet sınırları Azure Search'te | Microsoft Docs"
description: "Kapasite planlama için kullanılan hizmet sınırları ve istekleri ve yanıtları Azure arama için en fazla sınırlandırır."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 3deb0ff81114c840798c5927ad7311d7e603813d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="service-limits-in-azure-search"></a>Azure Search hizmet sınırları
Maksimum depolama, iş yükleri ve dizinler, belgeler, miktarda sınırlar ve bağımlı nesneler olup olmadığına göre [Azure Search sağlamak](search-create-service-portal.md) adresindeki bir **serbest**, **temel**, veya **Standart** fiyatlandırma katmanı.

* **Ücretsiz** Azure aboneliğinizle birlikte gelen bir çok kiracılı paylaşılan bir hizmettir. 
* **Temel** daha küçük ölçekli üretim iş yükleri için özel bilgi işlem kaynakları sağlar.
* **Standart** daha fazla depolama ve işleme kapasiteye sahip ayrılmış makinelerde her düzeyde çalışır. Standart gelen dört düzeyler: S1, S2, S3 ve S3 yüksek yoğunluklu (S3 HD).

> [!NOTE]
> Bir hizmeti belirli bir katman sağlanır. Kapasite sağlamak için katmanları atlama (hiçbir yerinde yükseltme yoktur) yeni bir hizmet sağlama içerir. Daha fazla bilgi için bkz: [bir SKU katmanı seçin veya](search-sku-tier.md). Zaten sağlanan hizmet kapasitesiyle ayarlama hakkında daha fazla bilgi için bkz: [ölçeklendirme sorgu ve dizin oluşturma iş yükleri için kaynak düzeylerini](search-capacity-planning.md).
>

## <a name="per-subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Hizmet sınırları
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Dizin sınırları
Dizinleri sınırları ve dizin oluşturucular sınırları arasında bire bir ilişkisi yok. 200 dizinleri sınırının verildiğinde, dizin oluşturucular sınırını da aynı hizmet için 200'dür.

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Dizini: dizin başına en fazla alan |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Dizin: dizin başına profilleri Puanlama maksimum |100 |100 |100 |100 |100 |100 |
| Dizini: Profil başına en fazla işlevleri |8 |8 |8 |8 |8 |8 |
| Dizin oluşturucular: çağrı başına en fazla dizin yükleme |10.000 belgeleri |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |Maksimum belge yalnızca sınırlıdır |YOK <sup>2</sup> |
| Dizin oluşturucular: en fazla çalışma süresini | 1-3 dakika <sup>3</sup> |24 saat |24 saat |24 saat |24 saat |YOK <sup>2</sup> |
| BLOB dizin oluşturucu: en fazla blob boyutu, MB |16 |16 |128 |256 |256 |YOK <sup>2</sup> |
| BLOB dizin oluşturucu: blob üzerinden ayıkladığınız içeriği en fazla karakter |32,000 |64,000 |4 milyon |4 milyon |4 milyon |YOK <sup>2</sup> |

<sup>1</sup> temel katman yalnızca SKU dizin başına 100 alanlarının alt sınırına sahip olduğunu.

<sup>2</sup> S3 HD dizin oluşturucular şu anda desteklemiyor. Bu özellik için Acil bir gereksiniminiz varsa Azure desteğine başvurun.

<sup>3</sup> ücretsiz katmanı için maksimum yürütme süresini dizin oluşturucu olan 3 dakika blob kaynaklarının ve diğer tüm veri kaynakları 1 dakikadır.

## <a name="document-size-limits"></a>Belge boyutu sınırları
| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Dizin API başına belgenin boyutu |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |

Bir dizin API çağrılırken en fazla belge boyutuna başvuruyor. Belge, aslında bir dizin API istek gövdesi boyutu sınırı boyutudur. Dizin API için aynı anda birden çok belge toplu geçirebilirsiniz olduğundan, boyut sınırını gerçekte kaç belgeleri toplu işlemde bağlıdır. Bir toplu işin için tek bir belgenin, JSON 16 MB maksimum belge boyutudur.

Belge boyutu tutun, sorgulanabilir olmayan verileri istekten dışlamak unutmayın. Görüntüleri ve diğer ikili veriler doğrudan sorgulanabilir değildir ve dizinde saklanan döndürmemelidir. Arama sonuçlarında sorgulanabilir olmayan verilerini tümleştirmek için bir URL referansını kaynağa depolar yapılamayan bir alan tanımlayın.

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
