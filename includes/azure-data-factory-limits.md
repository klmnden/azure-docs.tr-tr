---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/20/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 4aa4809c57eaf26b10053d432f9191580ec143a0
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44381096"
---
Veri Fabrikası yerde müşteri aboneliklerini birbirlerinin iş yüklerini korunan emin olmak için aşağıdaki varsayılan sınırlara sahip çok kiracılı bir hizmettir. Çoğu sınırlarını kolayca sınırına kadar aboneliğinizin destekle iletişim kurarak yükseltilebilir.

### <a name="version-2"></a>Sürüm 2

| Kaynak | Varsayılan Sınır | Üst Sınır |
| -------- | ------------- | ------------- |
| Bir Azure aboneliğinde veri fabrikaları | 50 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data factory içindeki varlıklar (işlem hattı, veri kümeleri, Tetikleyiciler, bağlı hizmetler, tümleştirme çalışma zamanları) toplam sayısı | 5000 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Bir abonelik altında Azure-SSIS tümleştirme Runtime(s) için toplam CPU Çekirdeği | 128 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| İşlem hattı eşzamanlı işlem hattını çalışır | 100 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri Fabrikası eşzamanlı işlem hattını çalışır | 10,000  | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| (Kapsayıcılar için iç etkinlikler dahil) işlem hattı başına en fazla etkinlikleri | 40 | 40 |
| İşlem hattı başına en fazla parametreleri | 50 | 50 |
| ForEach öğeleri | 100.000 | 100.000 |
| ForEach paralelliği | 20 | 50 |
| Karakter başına ifadesi | 8,192 | 8,192 |
| En düşük atlayan pencere tetikleyicisi aralığı | 15 dakika | 15 dakika |
| İşlem hattı etkinliği çalıştırmaları için en büyük zaman aşımı | 7 gün | 7 gün |
| İşlem hattı nesneleri için nesne başına bayt <sup>1</sup> | 200 KB | 200 KB |
| Ve bağlı hizmet nesneleri veri kümesi için nesne başına bayt <sup>1</sup> | 100 KB | 2000 KB |
| Kopyalama etkinliği çalıştırma başına veri tümleştirme birimi <sup>3</sup> | 256 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| API çağrıları yazma | 2500/SA<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Okuma API çağrıları | 12.500/SA<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Dakika başına sorguları izleme | 1000 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Dakika başına varlık CRUD işlemleri | 50 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan Sınır** | **Üst Sınır** |
| --- | --- | --- |
| Bir Azure aboneliğinde veri fabrikaları |50 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data factory içindeki işlem hatları |2500 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data factory içinde veri kümeleri |5000 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Veri kümesi başına eşzamanlı dilimleri |10 |10 |
| İşlem hattı nesneleri için nesne başına bayt <sup>1</sup> |200 KB |200 KB |
| Ve bağlı hizmet nesneleri veri kümesi için nesne başına bayt <sup>1</sup> |100 KB |2000 KB |
| Bir Abonelikteki HDInsight talep üzerine küme çekirdek <sup>2</sup> |60 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Bulut kopyalama etkinliği çalıştırma başına veri taşıma birimleri <sup>3</sup> |32 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Yeniden deneme sayısı için işlem hattı etkinliği çalıştırmaları |1000 |MAXINT (32 bit) |

<sup>1</sup> işlem hattı, veri kümesi ve bağlı hizmet nesneleri temsil eden iş yükünüz mantıksal bir gruplandırmasıdır. Bu nesneler için sınırları, taşıma ve işleme Azure Data Factory hizmetiyle veri miktarı için ilişkili değil. Veri fabrikası, petabaytlarca veriyi işlemek için ölçeklendirilebilecek şekilde tasarlanmıştır.

<sup>2</sup> isteğe bağlı HDInsight çekirdekleri data factory içeren aboneliği dışında ayrılır. Sonuç olarak, yukarıdaki Data Factory, isteğe bağlı HDInsight çekirdekleri için çekirdek sınırı zorunlu ve Azure aboneliğinizle ilişkili çekirdek sınırı farklıdır sınırlıdır.

<sup>3</sup> v2 veri tümleştirme birim (DIU) veya Bulut veri taşıma birimi (DMU) V1, bir Bulut Bulut kopyalama işleminde kullanıldığı. Data Factory içinde tek bir birim (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüdür. Bazı senaryolar için daha fazla DMUs kullanarak daha yüksek kopya aktarım hızı elde edebilirsiniz. Başvurmak [veri tümleştirme birimleri (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) ve [bulut veri taşıma birimleri (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) Ayrıntılar bölümünde ve [Azure Data Factory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/data-factory/) olduğu çıkarımında fatura için.

<sup>4</sup> Integration Runtime (IR), farklı ağ ortamlarında aşağıdaki veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır: veri taşıma etkinlikleri işlem Hizmetleri, için gönderme, SSIS paketlerini yürütme. Daha fazla bilgi için [tümleştirme çalışma zamanına genel bakış](../articles/data-factory/concepts-integration-runtime.md).

| **Kaynak** | **Varsayılan alt sınır** | **Alt sınır** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Yeniden deneme girişimleri arasındaki aralık |1 saniye |1 saniye |
| Zaman aşımı değeri yeniden deneyin |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti çağrısı sınırları
Azure Resource Manager API çağrıları için sınırlara sahiptir. İçinde bir hızda API çağrıları gerçekleştirmek [Azure Resource Manager API'si sınırlar](../articles/azure-subscription-service-limits.md#resource-group-limits).
