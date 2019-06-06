---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 5/30/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: ac0747dcff4e74363a58dc9aaf6da4dbd4c6a1c7
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427591"
---
Azure Data Factory yerde müşteri aboneliklerini birbirlerinin iş yüklerini korunan emin olmak için aşağıdaki varsayılan sınırlara sahip çok kiracılı bir hizmettir. Aboneliğiniz için en sınırları artırmak için desteğe başvurun.

### <a name="version-2"></a>Sürüm 2

| Resource | Varsayılan limit | Üst sınır |
| -------- | ------------- | ------------- |
| Bir Azure aboneliğinde veri fabrikaları | 50 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hatları, veri kümeleri, Tetikleyiciler, bağlı hizmetler ve data factory içinde tümleştirme çalışma zamanları gibi varlıkları toplam sayısı | 5,000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bir abonelik altında toplam CPU çekirdeği için Azure-SSIS tümleştirme çalışma zamanları | 256 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Tüm işlem hatları fabrikasında arasında paylaşılan bir veri fabrikası başına eşzamanlı işlem hattını çalışır | 10,000  | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Eş zamanlı dış etkinlik çalıştırmalarını abonelik başına [Azure tümleştirme çalışma zamanı bölgesine](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Dış etkinlikleri, tümleştirme çalışma zamanını yönetilen ancak bağlı hizmet, Databricks, saklı yordam, Hdınsights ve diğerleri dahil olmak üzere yürütün.</small> | 3000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Eşzamanlı işlem hattı çalıştırmalarını abonelik başına [Azure tümleştirme çalışma zamanı bölgesine](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) <br><small>İşlem hattı etkinliklerinin dahil arama, GetMetadata, tümleştirme çalışma zamanı üzerinde yürütülen ve silin. </small>| 1000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Eşzamanlı yazma işlemleri, abonelik başına [Azure tümleştirme çalışma zamanı bölgesine](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Bağlantıyı test et, göz atma klasör listesi ve Tablo listesi dahil olmak üzere verileri önizleyin. | 200 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Eş zamanlı veri tümleştirme birimleri<sup>1</sup> abonelik başına tüketim [Azure tümleştirme çalışma zamanı bölgesine](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)| Bölge grubu 1<sup>2</sup>: 6000<br>Bölge grubu 2<sup>2</sup>: 3000<br>Bölge Grup 3<sup>2</sup>: 1500 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Kapsayıcılar için iç etkinliklerini içeren işlem hattı başına en fazla etkinlikleri | 40 | 40 |
| Bir tek şirket içinde barındırılan tümleştirme çalışma zamanı karşı oluşturulabilir bağlı tümleştirme çalışma zamanları sayısı | 100 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| İşlem hattı başına en fazla parametreleri | 50 | 50 |
| ForEach öğeleri | 100,000 | 100,000 |
| ForEach paralelliği | 20 | 50 |
| Karakter başına ifadesi | 8,192 | 8,192 |
| En az bir atlayan pencere tetikleyicisi aralığı | 15 dakika | 15 dakika |
| İşlem hattı etkinliği için en uzun zaman aşımı çalıştırır | 7 gün | 7 gün |
| İşlem hattı nesneleri için nesne başına bayt<sup>3</sup> | 200 KB | 200 KB |
| Ve bağlı hizmet nesneleri veri kümesi için nesne başına bayt<sup>3</sup> | 100 KB | 2. 000'KB |
| Veri tümleştirme birimleri<sup>1</sup> kopyalama etkinliği çalıştırma başına | 256 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| API çağrıları yazma | 2.500/h<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Okuma API çağrıları | 12.500/h<br/><br/> Azure Resource Manager, Azure Data Factory tarafından bu sınırı uygulanmaktadır. | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına sorguları izleme | 1000 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Dakika başına varlık CRUD işlemleri | 50 | [Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |

<sup>1</sup> veri tümleştirme birimi (DIU) bir Bulut Bulut kopyalama işleminde kullanılan, daha fazla bilgi [veri tümleştirme birimleri (sürüm 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Faturalama hakkında daha fazla bilgi için bkz: [Azure Data Factory fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> [azure Integration Runtime](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) olduğu [küresel olarak kullanılabilir](https://azure.microsoft.com/global-infrastructure/services/) veri uyumluluk sağlamak için verimlilik ve daha düşük ağ çıkış maliyetlerini. 

| Bölge grubu | Bölgeler | 
| -------- | ------ |
| Bölge grubu 1 | Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 |
| Bölge grubu 2 | Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Hindistan, Japonya Doğu, Kuzeyorta ABD, kullanımdadır ABD, Güneydoğu Asya, Batı Orta ABD |
| Bölge grubu 3 | Kanada Orta, Güneydoğu Asya, Fransa Orta, Kore Orta, Birleşik Krallık Güney |

<sup>3</sup> işlem hattı, veri kümesi ve bağlı hizmet nesneleri temsil eden iş yükünüz mantıksal bir gruplandırmasıdır. Bu nesneler için sınırları, Azure Data Factory ile veri taşıma ve işleme miktarını ile ilişkili olmayan. Veri fabrikası, petabaytlarca veriyi işlemek için ölçeklendirilebilecek şekilde tasarlanmıştır.

### <a name="version-1"></a>Sürüm 1

| **Kaynak** | **Varsayılan sınır** | **Üst sınırı** |
| --- | --- | --- |
| Bir Azure aboneliğinde veri fabrikaları |50 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Data factory içindeki işlem hatları |2,500 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Data factory içinde veri kümeleri |5,000 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Veri kümesi başına eşzamanlı dilimleri |10 |10 |
| İşlem hattı nesneleri için nesne başına bayt<sup>1</sup> |200 KB |200 KB |
| Veriler için nesne başına bayt kümesi ve bağlı hizmet nesneleri<sup>1</sup> |100 KB |2. 000'KB |
| Bir Abonelikteki Azure HDInsight talep üzerine küme çekirdek<sup>2</sup> |60 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Bulut kopyalama etkinliği çalıştırma başına veri taşıma birimleri<sup>3</sup> |32 |[Destek ekibiyle iletişime geçin](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Yeniden deneme sayısı için işlem hattı etkinliği çalıştırmaları |1000 |MAXINT (32 bit) |

<sup>1</sup> işlem hattı, veri kümesi ve bağlı hizmet nesneleri temsil eden iş yükünüz mantıksal bir gruplandırmasıdır. Bu nesneler için sınırları, Azure Data Factory ile veri taşıma ve işleme miktarını ile ilişkili olmayan. Veri fabrikası, petabaytlarca veriyi işlemek için ölçeklendirilebilecek şekilde tasarlanmıştır.

<sup>2</sup> isteğe bağlı HDInsight çekirdekleri data factory içeren aboneliği dışında ayrılır. Sonuç olarak, önceki isteğe bağlı HDInsight çekirdekleri için Data Factory tarafından zorlanan çekirdek sınırı sınırlıdır. Azure aboneliğinizle ilişkili çekirdek sınırı farklıdır.

<sup>3</sup> bir Bulut Bulut kopyalama işleminde kullanılan sürüm 1 için bulut verisi taşıma birimi (DMU) daha fazla bilgi [bulut veri taşıma birimleri (sürüm 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Faturalama hakkında daha fazla bilgi için bkz: [Azure Data Factory fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/).

| **Kaynak** | **Varsayılan alt sınır** | **Alt sınır** |
| --- | --- | --- |
| Zamanlama aralığı |15 dakika |15 dakika |
| Yeniden deneme girişimleri arasındaki aralık |1 saniye |1 saniye |
| Zaman aşımı değeri yeniden deneyin |1 saniye |1 saniye |

#### <a name="web-service-call-limits"></a>Web hizmeti çağrısı sınırları
Azure Resource Manager API çağrıları için sınırlara sahiptir. İçinde bir hızda API çağrıları gerçekleştirmek [Azure Resource Manager API'si sınırlar](../articles/azure-subscription-service-limits.md#resource-group-limits).
