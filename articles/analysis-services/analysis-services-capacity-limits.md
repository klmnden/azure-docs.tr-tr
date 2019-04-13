---
title: Azure Analysis Services kaynak ve nesne sınırları | Microsoft Docs
description: Azure Analysis Services kaynak ve nesne sınırlarını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0ffbffc788baaffd4a0532c3918ed82cc3eaf5c3
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527821"
---
# <a name="analysis-services-resource-and-object-limits"></a>Analysis Services kaynak ve nesne sınırları

Bu makalede resource ve model nesnesi sınırlar.

## <a name="tier-limits"></a>Katmanın limitlerine

### <a name="developer-tier"></a>Geliştirici katmanı

Bu katman değerlendirme, geliştirme ve test senaryoları için önerilir. Tek bir plan, standart katman ile sunulan aynı işlevleri içerir ancak işlemci gücü, QPU ve bellek boyutu bakımından sınırlıdır. Sorgu çoğaltma genişletmesini Bu katman için kullanılamaz. Bu katman bir SLA sunmaz.

|Planlama  |QPU’lar  |Bellek (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Temel katman

Bu katman küçük tablolu modeller, sınırlı düzeyde kullanıcı eşzamanlılığı ve basit veri yenileme gereksinimlerine sahip olan üretim çözümleri için önerilir. Sorgu çoğaltma genişletmesini *kullanılamıyor* bu katmanı için. Bu katmanda Perspektifler, birden çok bölüme ve DirectQuery tablosal model özellikleri desteklenmez.  

|Planlama  |QPU’lar  |Bellek (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Standart katman

Bu katman, kullanıcı eşzamanlılığının elastik olmasını gerektiren ve hızla büyüyen veri modellerine sahip olan görev açısından kritik üretim uygulamalarına yöneliktir. Neredeyse gerçek zamanlı veri modeli güncelleştirmeleri için gelişmiş veri yenilemeyi ve tüm tablo modelleme özelliklerini destekler.

|Planlama  |QPU’lar  |Bellek (GB)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Tüm bölgelerde kullanılamaz.  

## <a name="object-limits"></a>Nesne sınırı

Bu sınırlar, teorik. Performans, düşük numaralarını yayınladıklarını.

|Nesne|En büyük boyutları/numaraları|  
|------------|----------------------------|  
|Bir örneğindeki veritabanları|16,000|  
|Tabloları ve sütunları veritabanında toplam istemci sayısı|16,000|  
|Bir tablodaki satırlar|Sınırsız<br /><br /> **Uyarı:** Kısıtlama tablodaki hiçbir sütunu birden fazla 1.999.999.997'dir farklı değerleri olabilir.|  
|Bir tablodaki hiyerarşiler|15,999|  
|Bir hiyerarşi düzeyleri|15,999|  
|İlişkiler|8,000|  
|Tüm tabloda anahtar sütunları|15,999|  
|Tablo ölçüler|2^31-1 = 2,147,483,647|  
|Bir sorgu tarafından döndürülen hücreleri|2^31-1 = 2,147,483,647|  
|Kaynak sorguyu kayıt boyutu|64 K|  
|Nesne adı uzunluğu|512 karakteri|  


