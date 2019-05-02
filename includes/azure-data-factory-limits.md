---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 1/10/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 6d06ac6efd08c14f77fd963ddf2c67de54260959
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64733781"
---
Azure Data Factory yerde müşteri aboneliklerini birbirlerinin iş yüklerini korunan emin olmak için aşağıdaki varsayılan sınırlara sahip çok kiracılı bir hizmettir. Aboneliğiniz için en sınırları artırmak için desteğe başvurun.

### <a name="version-2"></a>Sürüm 2

| Kaynak | Varsayılan limit | Üst sınır |
| -------- | ------------- | ------------- |
| Bir Azure aboneliğinde veri fabrikaları | 50 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hatları, veri kümeleri, Tetikleyiciler, bağlı hizmetler ve data factory içinde tümleştirme çalışma zamanları gibi varlıkları toplam sayısı | 5.000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bir abonelik altında toplam CPU çekirdeği için Azure-SSIS tümleştirme çalışma zamanları | 256 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Tüm işlem hatları fabrikasında arasında paylaşılan bir veri fabrikası başına eşzamanlı işlem hattını çalışır | 10,000  | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Kapsayıcılar için iç etkinliklerini içeren işlem hattı başına en fazla etkinlikleri | 40 | 40 |
| Bir tek şirket içinde barındırılan tümleştirme çalışma zamanı karşı oluşturulabilir bağlı tümleştirme çalışma zamanları sayısı | 100 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hattı başına en fazla parametreleri | 50 | 50 |
| ForEach öğeleri | 100.000 | 100.000 |
| ForEach paralelliği | 20 | 50 |
| Karakter başına ifadesi | 8,192 | 8,192 |
| En az bir atlayan pencere tetikleyicisi aralığı | 15 dakika | 15 dakika |
| İşlem hattı etkinliği için en uzun zaman aşımı çalıştırır | 7 gün | 7 gün |
| İşlem hattı nesneleri için nesne başına bayt<sup>1</sup> | 200 KB | 200 KB |
| Ve bağlı hizmet nesneleri veri kümesi için nesne başına bayt<sup>1</sup> | 100 KB | 2. 000'KB |
| Kopyalama etkinliği çalıştırma başına veri tümleştirme birimi<sup>3</sup> | 256 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| API çağrıları yazma | 2.500/h<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Okuma API çağrıları | 12.500/h<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına sorguları izleme | 1000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına varlık CRUD işlemleri | 50 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |


### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan sınır** | **Üst sınırı** |
| --- | --- | --- |
| Bir Azure aboneliğinde veri fabrikaları |50 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Data factory içindeki işlem hatları |2,500 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Data factory içinde veri kümeleri |5.000 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Veri kümesi başına eşzamanlı dilimleri |10 |10 |
| İşlem hattı nesneleri için nesne başına bayt<sup>1</sup> |200 KB |200 KB |
| Veriler için nesne başına bayt kümesi ve bağlı hizmet nesneleri<sup>1</sup> |100 KB |2. 000'KB |
| Bir Abonelikteki Azure HDInsight talep üzerine küme çekirdek<sup>2</sup> |60 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bulut kopyalama etkinliği çalıştırma başına veri taşıma birimleri<sup>3</sup> |32 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Yeniden deneme sayısı için işlem hattı etkinliği çalıştırmaları |1000 |MAXINT (32 bit) |

<sup>1</sup>işlem hattı, veri kümesi ve bağlı hizmet nesneleri temsil eden iş yükünüz mantıksal bir gruplandırmasıdır. Bu nesneler için sınırları, Azure Data Factory ile veri taşıma ve işleme miktarını ile ilişkili olmayan. Veri fabrikası, petabaytlarca veriyi işlemek için ölçeklendirilebilecek şekilde tasarlanmıştır.

<sup>2</sup>isteğe bağlı HDInsight çekirdekleri data factory içeren aboneliği dışında ayrılır. Sonuç olarak, önceki isteğe bağlı HDInsight çekirdekleri için Data Factory tarafından zorlanan çekirdek sınırı sınırlıdır. Azure aboneliğinizle ilişkili çekirdek sınırı farklıdır.

<sup>3</sup>veri tümleştirme birimi için sürüm 2 (DIU) veya Bulut verisi taşıma birimi (DMU) sürüm 1 için bir Bulut Bulut kopyalama işleminde kullanılır. Data Factory içinde tek bir birim gücünü temsil eden bir ölçüdür. Bu, CPU, bellek ve ağ kaynağı ayırma birleştirir. Bazı senaryolar için daha fazla DMUs kullanarak daha yüksek kopya aktarım hızı elde edebilirsiniz. Daha fazla bilgi için [veri tümleştirme birimleri (sürüm 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) ve [bulut veri taşıma birimleri (sürüm 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Faturalama hakkında daha fazla bilgi için bkz: [Azure Data Factory fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>4</sup>Integration runtime (IR) gibi veri taşıma etkinlikleri işlem Hizmetleri, için gönderme, farklı ağ ortamları genelinde veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır ve SSIS paketlerini yürütme. Daha fazla bilgi için [tümleştirme çalışma zamanına genel bakış](../articles/data-factory/concepts-integration-runtime.md).

| **Kaynak** | **Varsayılan alt sınır** | **Alt sınır** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Yeniden deneme girişimleri arasındaki aralık |1 saniye |1 saniye |
| Zaman aşımı değeri yeniden deneyin |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti çağrısı sınırları
Azure Resource Manager API çağrıları için sınırlara sahiptir. İçinde bir hızda API çağrıları gerçekleştirmek [Azure Resource Manager API'si sınırlar](../articles/azure-subscription-service-limits.md#resource-group-limits).
