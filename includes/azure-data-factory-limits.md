---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 03/27/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: e68e87bb19b8c64cf06c03831b22cf43d773efde
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
Veri Fabrikası aboneliklerini birbirlerinin iş yüklerini korunan emin olmak için yerinde aşağıdaki varsayılan sınırları sahip çok kiracılı bir hizmettir. Birçok sınırları kolayca sınırına kadar aboneliğiniz için desteğe başvurarak yükseltilebilir.

### <a name="version-2"></a>Sürüm 2

| Kaynak | Varsayılan Sınır | Üst Sınır | 
| -------- | ------------- | ------------- | 
| bir Azure aboneliğinizin Data factory'leri | 50 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| data factory içinde komut zincirleri | 2500 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| data factory içinde veri kümeleri | 2500 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data factory içinde Tetikleyiciler | 2500 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Bağlı hizmetler data factory içinde | 2500 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Data factory içinde tümleştirme çalışma zamanları <sup>4</sup> | 2500 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Ardışık Düzen eşzamanlı ardışık düzen çalışır | 20 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Ardışık Düzen başına en fazla etkinlikleri | 20 | 30 |
| Ardışık Düzen başına en fazla parametreleri | 20 | 30 |
| Ardışık Düzen nesneleri için nesne başına bayt <sup>1</sup> | 200 KB | 200 KB |
| Veri kümesi için nesne ve bağlantılı hizmet nesneleri başına bayt <sup>1</sup> | 100 KB | 2000 KB |
| Bulut veri taşıma birimleri <sup>3</sup> | 256 | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Yeniden deneme sayısı ardışık düzen etkinlik çalışması | 1 day(timeout) | 1 gün (zaman aşımı) |
| API çağrıları yazma | 2500/İK<br/><br/> Bu sınır, Azure Resource Manager Azure Data Factory sınırlamasıdır. | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Okuma API çağrıları | 12.500/İK<br/><br/> Bu sınır, Azure Resource Manager Azure Data Factory sınırlamasıdır. | [Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan Sınır** | **Üst Sınır** |
| --- | --- | --- |
| bir Azure aboneliğinizin Data factory'leri |50 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| data factory içinde komut zincirleri |2500 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| data factory içinde veri kümeleri |5000 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| veri kümesi başına eşzamanlı dilimleri |10 |10 |
| Ardışık Düzen nesneleri için nesne başına bayt <sup>1</sup> |200 KB |200 KB |
| Veri kümesi için nesne ve bağlantılı hizmet nesneleri başına bayt <sup>1</sup> |100 KB |2000 KB |
| Bir abonelik içindeki Hdınsight isteğe bağlı küme çekirdeği <sup>2</sup> |60 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Bulut veri taşıma birimleri <sup>3</sup> |32 |[Desteğe başvurun](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Yeniden deneme sayısı ardışık düzen etkinlik çalışması |1000 |MAXINT (32 bit) |

<sup>1</sup> ardışık düzeni, veri kümesi ve bağlantılı hizmet nesneleri temsil eden İş yükünüzün mantıksal bir gruplandırması. Bu nesneler için sınırları taşıyın ve Azure Data Factory hizmetiyle işlem veri miktarı ile ilgili değildir. Veri Fabrikası petabaytlarca verileri işlemek için ölçeklendirmek için tasarlanmıştır.

<sup>2</sup> isteğe bağlı Hdınsight çekirdek, veri fabrikası içeren abonelik dışında ayrılır. Sonuç olarak, yukarıdaki Data Factory isteğe bağlı Hdınsight çekirdek çekirdek sınırını zorunlu ve Azure aboneliğinizle ilişkili çekirdek sınırına farklıdır sınırlıdır.

<sup>3</sup> bulut veri taşıma birimi (DMU) bir Bulut Bulut kopyalama işleminde kullanılıyor. Veri Fabrikası'nda tek bir birimi (CPU, bellek ve ağ kaynağı ayırma birleşimi) gücünü temsil eden bir ölçüsüdür. Bazı senaryolar için daha fazla DMUs kullanarak daha yüksek kopyalama verimlilik elde edebilirsiniz. Başvurmak [bulut veri taşıma birimleri (V2)](../articles/data-factory/copy-activity-performance.md#cloud-data-movement-units) ve [bulut veri taşıma birimleri (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) ayrıntıları bölümü.

<sup>4</sup> tümleştirmesi çalışma zamanı (IR) olan farklı ağ ortamlar genelinde aşağıdaki veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan bilgi işlem altyapısı: veri taşıma etkinlikleri işlem Hizmetleri, gönderme SSIS paketleri yürütme. Daha fazla bilgi için bkz: [tümleştirme çalışma zamanına genel bakış](../articles/data-factory/concepts-integration-runtime.md).

| **Kaynak** | **Varsayılan alt sınırı** | **Alt sınırı** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Yeniden deneme girişimleri arasındaki aralığı |1 saniye |1 saniye |
| Zaman aşımı değeri yeniden deneyin |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti çağrısı sınırları
Azure Resource Manager API çağrıları için sınırları vardır. İçinde bir hızda API çağrıları yapma [Azure Kaynak Yöneticisi API'si sınırlar](../articles/azure-subscription-service-limits.md#resource-group-limits).
