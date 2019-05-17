---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: d2daafa6bf5f9a28ad2b61a97e7a8bd2246ae18d
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65538385"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure'da hangi disk türleri mevcuttur?

Azure yönetilen diskler, şu anda genel kullanıma (GA) ve şu anda önizlemede olan bir üç biri olan dört disk türü sunar. Kendi uygun hedef müşteri senaryoları her şu dört disk türleri vardır.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, kullanacağınız seçeneğe karar vermenize yardımcı olması için ultra solid sürücüler state (SSD) (Önizleme), premium SSD, standart SSD ve yönetilen diskler için standart sabit disk sürücülerinin (HDD) bir karşılaştırması verilmiştir.

|   | Ultra yüksek SSD (Önizleme)   | Premium SSD   | Standart SSD   | Standart HDD   |
|---------|---------|---------|---------|---------|
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |SAP HANA, en çok katmanı veritabanları (örneğin, SQL, Oracle gibi) ve diğer işlem yoğunluklu iş yükleri gibi g/ç yoğunluklu iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişim   |
|Disk boyutu   |65.536 gibibayt (GiB) (Önizleme)   |32.767 giB    |32.767 giB   |32.767 giB   |
|En fazla aktarım hızı   |2. 000'mib / sn (Önizleme)   |900 MiB/s   |750 MiB/s   |500 MiB/s   |
|Maks. IOPS   |160,000 (Önizleme)   |20.000   |6,000   |2,000   |

## <a name="ultra-ssd-preview"></a>Ultra yüksek SSD (Önizleme)

Azure ultra SSD (Önizleme), Azure Iaas Vm'leri için yüksek performans, yüksek IOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunun. Ultra SSD bazı ek avantajları, iş yüklerinizi, sanal makinelerinizi yeniden başlatmaya gerek kalmadan yanı sıra, disk performansını dinamik olarak değiştirme özelliği içerir. Ultra yüksek SSD SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur. Ultra yüksek SSD yalnızca veri diskleri kullanılabilir. İşletim sistemi diski premium SSD kullanmanızı öneririz.

### <a name="performance"></a>Performans

Ultra yüksek bir disk sağladığınızda, kapasite ve performans diskin bağımsız olarak yapılandırabilirsiniz. Ultra yüksek SSD 64 Tib'a kadar 4 GiB arasında değişen birçok sabit boyutta gelir ve IOPS ve aktarım hızını bağımsız olarak yapılandırmanıza olanak tanıyan esnek performans yapılandırma modeli özelliği.

Ultra yüksek SSD, bazı temel işlevler şunlardır:

- Disk kapasitesi: Ultra yüksek SSD kapasitesi aralıkları 4'ten en fazla 64 TiB GiB.
- Disk IOPS: Ultra yüksek SSD 300 IOPS/GiB, 160 K IOPS disk başına en fazla IOPS sınırları destekler. Sağladığınız IOPS elde etmek için seçili Disk IOPS VM IOPS değerinden küçük olmasını sağlayın. Minimum disk IOPS olan 100 IOPS.
- Disk aktarım hızı: Her 2000 MB disk başına en fazla IOPS sağlanması için Ultra yüksek SSD ile tek bir disk aktarım hızı sınırı 256 KiB/sn olan (burada MB/sn = 10 ^ 6 bayt / saniye). Minimum disk aktarım hızı 1 MiB ' dir.
- Ultra yüksek SSD disk performans öznitelikleri (IOPS ve aktarım hızı) ayarlama, çalışma zamanında'diski sanal makineden ayırmayı olmadan destekler. Disk performansı yeniden boyutlandırma işlemi bir diskte verildikten sonra bu değişikliğin gerçekten etkili bir saate kadar sürebilir.

### <a name="disk-size"></a>Disk boyutu

|Disk Boyutu (GiB)  |IOPS Caps  |Aktarım hızı sınırı (MB/sn)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19,200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1024-65.536 (boyutları 1 TiB artışlarla artan bu aralıkta)     |160,000         |2,000         |

### <a name="transactions"></a>İşlemler

Ultra yüksek SSD için her g/ç işlemi daha az veya eşit 256 tek bir g/ç işleme aktarım hızının KiB olarak kabul edilir. Aktarım hızı 256 KiB daha büyük g/ç işlemleri birden çok g/ç boyutu 256 değerlendirilir KiB.

### <a name="preview-scope-and-limitations"></a>Önizleme kapsamı ve sınırlamalar

Önizleme sırasında ultra SSD:

- Doğu ABD 2'de tek bir kullanılabilirlik alanına desteklenir  
- Yalnızca (kullanılabilirlik kümeleri ve Ultra yüksek bir diski olanağına sahip olmayan tek VM dağıtımları bölgeleri dışında) kullanılabilirlik alanları ile kullanılabilir
- ES/DS v3 Vm'lerde yalnızca desteklenir
- Veri diskleri ve yalnızca destek 4 k fiziksel kesim boyutu yalnızca kullanılabilir  
- Yalnızca boş diskler olarak oluşturulabilir  
- Şu anda yalnızca Azure Resource Manager şablonları, CLI ve python SDK'sı kullanılarak dağıtılabilir.
- Henüz diski anlık görüntüleri, VM görüntüleri, kullanılabilirlik kümeleri, sanal makine ölçek kümeleri ve Azure disk şifrelemesini desteklemiyor.
- Henüz Azure Backup'ı veya Azure Site Recovery ile tümleştirmesini desteklemiyor.
- Olduğu gibi [çoğu önizlemeler](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bu özellik genel kullanılabilirlik (GA) kadar üretim iş yükleri için kullanılmamalıdır.
