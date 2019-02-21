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
ms.openlocfilehash: 8a067474fb172d4ff7a7fdf7eb6d24536bd2d017
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443281"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure'da hangi disk türleri mevcuttur?

Azure yönetilen diskler, şu anda genel kullanıma (GA) ve şu anda önizlemede olan bir üç biri olan dört disk türü sunar. Kendi uygun hedef müşteri senaryoları her şu dört disk türleri vardır.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, kullanacağınız seçeneğe karar vermenize yardımcı olması için ultra solid sürücüler state (SSD) (Önizleme), premium SSD, standart SSD ve yönetilen diskler için standart sabit disk sürücülerinin (HDD) bir karşılaştırması verilmiştir.

|   | Ultra yüksek SSD (Önizleme)   | Premium SSD   | Standart SSD   | Standart HDD   |
|---------|---------|---------|---------|---------|
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |SAP HANA, en çok katmanı veritabanları (örneğin, SQL, Oracle gibi) ve diğer işlem yoğunluklu iş yükleri gibi g/ç yoğunluklu iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişim   |
|Disk boyutu   |65.536 gibibayt (GiB) (Önizleme)   |4.095 giB (GA), 32.767 GiB (Önizleme)    |4.095 (GA) GiB 32.767 GiB (Önizleme)   |4.095 giB (GA), 32.767 GiB (Önizleme)   |
|En fazla aktarım hızı   |2. 000'mib / sn (Önizleme)   |250 (GA) MiB/sn, 750 MiB/sn (Önizleme)   |60 MiB/sn (GA), 500 MiB/sn (Önizleme)   |60 Mib/sn (GA), 500 MiB/sn (Önizleme)   |
|Maks. IOPS   |160,000 (Önizleme)   |7500 (GA) 20.000 (Önizleme)   |500 (GA), 2.000 (Önizleme)   |500 (GA), 2.000 (Önizleme)   |

## <a name="ultra-ssd-preview"></a>Ultra yüksek SSD (Önizleme)

Azure ultra SSD (Önizleme), Azure Iaas Vm'leri için yüksek performans, yüksek IOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunun. Ultra SSD bazı ek avantajları, iş yüklerinizi, sanal makinelerinizi yeniden başlatmaya gerek kalmadan yanı sıra, disk performansını dinamik olarak değiştirme özelliği içerir. Ultra yüksek SSD SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur. Ultra yüksek SSD yalnızca veri diskleri kullanılabilir. İşletim sistemi diski premium SSD kullanmanızı öneririz.

### <a name="performance"></a>Performans

Ultra yüksek bir disk sağladığınızda, kapasite ve performans diskin bağımsız olarak yapılandırabilirsiniz. Ultra yüksek SSD 64 Tib'a kadar 4 GiB arasında değişen birçok sabit boyutta gelir ve IOPS ve aktarım hızını bağımsız olarak yapılandırmanıza olanak tanıyan esnek performans yapılandırma modeli özelliği.

Ultra yüksek SSD, bazı temel işlevler şunlardır:

- Disk kapasitesi: Ultra yüksek SSD kapasitesi aralıkları 4'ten en fazla 64 TiB GiB.
- Disk IOPS: Ultra yüksek SSD 300 IOPS/GiB, 160 K IOPS disk başına en fazla IOPS sınırları destekler. Sağladığınız IOPS elde etmek için seçili Disk IOPS VM IOPS değerinden küçük olmasını sağlayın. Minimum disk IOPS olan 100 IOPS.
- Disk aktarım hızı: Her 2000 MB disk başına en fazla IOPS sağlanması için Ultra yüksek SSD ile tek bir disk aktarım hızı sınırı 256 KiB/sn olan (burada MB/sn = 10 ^ 6 bayt / saniye). Minimum disk aktarım hızı 1 MiB ' dir.

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

## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD giriş/çıkış (GÇ) ile sanal makineleri (VM'ler) için yüksek performanslı ve düşük gecikme süreli disk desteği sunmak-yoğun iş yükleri. Hızı avantajından ve premium depolama disklerini performansını yararlanmak için var olan VM diskleri için Premium SSD geçirebilirsiniz. Premium SSD, görev açısından kritik üretim uygulamaları için uygundur.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Premium SSD boyutları  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60 *              | P70 *              | P80 *              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu gib biriminde           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| Disk başına IOPS       | En fazla 120 | En fazla 240              | En fazla 500              | En fazla 1.100 | En fazla 2,300              | En fazla 5000              | En fazla 7.500             | En fazla 7.500              | En fazla 12.500              | En fazla 15000              | 20000              |
| Disk başına aktarım hızı | En fazla 25 MiB/sn | En çok 50 MiB/sn | En fazla 100 MiB/sn | En fazla 125 MiB/sn | En fazla 150 MiB/sn | En fazla 200 MiB/sn | En fazla 250 MiB/sn | En fazla 250 MiB/sn| En fazla 480 MiB/sn | En fazla 750 MiB/sn | En fazla 750 MiB/sn |

## <a name="standard-ssd"></a>Standart SSD

Azure standart SSD'ler düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için en iyi duruma getirilmiş düşük maliyetli depolama bir seçenektir. Standart SSD, özellikle varyansını HDD çözümlerinizi şirket içinde çalışan iş yükleri ile ilgili sorun yaşıyorsanız kullanıcıların buluta taşımak istediğiniz iyi giriş düzeyi deneyimi sunar. Standart SSD daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve HDD disklere karşılaştırıldığında gecikme süresi sunar. Standart SSD, Web sunucuları, düşük IOPS uygulama sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/Test iş yükleri için uygundur.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Standart SSD boyutları  | E10               | E15               | E20             | E30 | E40              | E50              | E60*              | E70 *              | E80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu gib biriminde           | 128             | 256             | 512            | 1,024  | 2,048            | 4,095     | 8,192     | 16,384     | 32,767    |
| Disk başına IOPS       | En fazla 500              | En fazla 500              | En fazla 500              | En fazla 500 | En fazla 500              | En fazla 500              | En fazla 500             | En fazla 500              | En fazla 1.300              | En fazla 2.000              | En fazla 2.000              |
| Disk başına aktarım hızı | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn| En fazla 300 MiB/sn |  En fazla 500 MiB/sn | En fazla 500 MiB/sn |

## <a name="standard-hdd"></a>Standart HDD

Azure standart HDD, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Ayrıca, BLOB'ları, tabloları, kuyrukları ve dosyaları destekler. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. Vm'lerle çalışırken, geliştirme/test senaryoları için ve kritik iş yüklerini daha az SSD ve HDD standart diskleri kullanabilirsiniz. Standart depolama, tüm Azure bölgelerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Standart Disk Türü  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *              | S70 *              | S80*              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu gib biriminde          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| Disk başına IOPS       | En fazla 500              | En fazla 500              | En fazla 500              | En fazla 500 | En fazla 500              | En fazla 500              | En fazla 500             | En fazla 500              | En fazla 1.300              | En fazla 2.000              | En fazla 2.000              |
| Disk başına aktarım hızı | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn| En fazla 300 MiB/sn | En fazla 500 MiB/sn | En fazla 500 MiB/sn |

## <a name="billing"></a>Faturalandırma

Yönetilen diskleri kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

- Disk türü
- Yönetilen disk boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- İşlem sayısı

**Yönetilen disk boyutu**: yönetilen diskler ile sağlanan boyutu faturalandırılır. Azure (en yakın önerilen disk boyutuna yuvarlanır) sağlanan boyut eşler. Sağlanan disk boyutları Ayrıntılar için önceki tablolara bakın. Her disk, desteklenen sağlanan disk boyutu teklifine eşlenir ve buna göre faturalandırılır. Örneğin, bu E15 disk boyutu teklifine eşleyen bir 200 GiB standart SSD sağladıysanız, (256 GiB). Sağlanmış bir diski için faturalama, saatlere Premium depolama teklif için aylık fiyat kullanılarak eşit olarak dağıtılır. E10 bir disk sağlanır ve 20 saat sonra silindi, için E10 teklif 20 saat eşit olarak dağıtılır. Örneğin, faturalandırılırsınız. Diske yazılan gerçek veri miktarından bağımsız olarak budur.

**Anlık görüntüleri**: Anlık görüntüler, kullanılan boyutuna göre faturalandırılır. Örneğin, anlık görüntü sağlanan kapasitesi 64 GiB ve gerçek kullanılan veri boyutu 10 GiB ile bir yönetilen diskin anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu 10 GiB için faturalandırılır.
