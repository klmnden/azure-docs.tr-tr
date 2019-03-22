---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0201776914610ddaca50a670fc500156a65cd734
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2019
ms.locfileid: "58124689"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD giriş/çıkış (GÇ) ile sanal makineleri (VM'ler) için yüksek performanslı ve düşük gecikme süreli disk desteği sunmak-yoğun iş yükleri. Hızı avantajından ve premium depolama disklerini performansını yararlanmak için var olan VM diskleri için Premium SSD geçirebilirsiniz. Premium SSD, görev açısından kritik üretim uygulamaları için uygundur.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Premium SSD boyutları | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 * | P70 * | P80 * |
|-------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Disk boyutu gib biriminde | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8,192 | 16,384 | 32,767 |
| Disk başına IOPS | En fazla 120 | En fazla 240 | En fazla 500 | En fazla 1.100 | En fazla 2,300 | En fazla 5000 | En fazla 7.500 | En fazla 7.500 | En fazla 12.500 | En fazla 15000 | 20000 |
| Disk başına aktarım hızı | En fazla 25 MiB/sn | En çok 50 MiB/sn | En fazla 100 MiB/sn | En fazla 125 MiB/sn | En fazla 150 MiB/sn | En fazla 200 MiB/sn | En fazla 250 MiB/sn | En fazla 250 MiB/sn| En fazla 480 MiB/sn | En fazla 750 MiB/sn | En fazla 750 MiB/sn |

## <a name="standard-ssd"></a>Standart SSD

Azure standart SSD'ler düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için en iyi duruma getirilmiş düşük maliyetli depolama bir seçenektir. Standart SSD, özellikle varyansını HDD çözümlerinizi şirket içinde çalışan iş yükleri ile ilgili sorun yaşıyorsanız kullanıcıların buluta taşımak istediğiniz iyi giriş düzeyi deneyimi sunar. Standart SSD daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve HDD disklere karşılaştırıldığında gecikme süresi sunar. Standart SSD, Web sunucuları, düşük IOPS uygulama sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/Test iş yükleri için uygundur.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Standart SSD boyutları | E4 | E6 | E10 | E15 | E20 | E30 | E40 | E50 | E60* | E70 * | E80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Disk boyutu gib biriminde | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8,192 | 16,384 | 32,767 |
| Disk başına IOPS | En fazla 120 | En fazla 240 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 1.300 | En fazla 2.000 | En fazla 2.000 |
| Disk başına aktarım hızı |  En fazla 25 MiB/sn |  En çok 50 MiB/sn  |  En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn| En fazla 300 MiB/sn |  En fazla 500 MiB/sn | En fazla 500 MiB/sn |

## <a name="standard-hdd"></a>Standart HDD

Azure standart HDD, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Ayrıca, BLOB'ları, tabloları, kuyrukları ve dosyaları destekler. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. Vm'lerle çalışırken, geliştirme/test senaryoları için ve kritik iş yüklerini daha az SSD ve HDD standart diskleri kullanabilirsiniz. Standart depolama, tüm Azure bölgelerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu

Yıldız ile işaretlenmiş boyutları şu anda Önizleme aşamasındadır.

| Standart Disk Türü | S4 | S6 | S10 | S15 | S20 | S30 | S40 | S50 | S60 * | S70 * | S80* |
|--------------------|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Disk boyutu gib biriminde | 32 | 64 | 128 | 256 | 512 | 1,024 | 2,048 | 4,095 | 8,192 | 16,384 | 32,767 |
| Disk başına IOPS | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 500 | En fazla 1.300 | En fazla 2.000 | En fazla 2.000 |
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