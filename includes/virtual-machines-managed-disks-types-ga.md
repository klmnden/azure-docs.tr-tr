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
ms.openlocfilehash: 885bc1c627626ee7ba4f391be31131b18fa1ab39
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65212055"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD giriş/çıkış (GÇ) ile sanal makineleri (VM'ler) için yüksek performanslı ve düşük gecikme süreli disk desteği sunmak-yoğun iş yükleri. Hızı avantajından ve premium depolama disklerini performansını yararlanmak için var olan VM diskleri için Premium SSD geçirebilirsiniz. Premium SSD, görev açısından kritik üretim uygulamaları için uygundur.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Standart depolama aksine, bir premium depolama disk sağlarken, kapasite, IOPS ve aktarım hızı bu diskin garanti edilir. Örneğin, P50 disk oluşturursanız, Azure 4.095 GB depolama kapasitesi, 7.500 IOPS ve 250-MB/sn aktarım hızı bu disk için hazırlar. Uygulamanız, kapasite ve performans bölümünü veya tümünü kullanabilirsiniz. Premium SSD diskleri, hedef performans % 99,9 oranında sağlamak üzere tasarlanmıştır.

## <a name="standard-ssd"></a>Standart SSD

Azure standart SSD'ler düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için en iyi duruma getirilmiş düşük maliyetli depolama bir seçenektir. Standart SSD, özellikle varyansını HDD çözümlerinizi şirket içinde çalışan iş yükleri ile ilgili sorun yaşıyorsanız kullanıcıların buluta taşımak istediğiniz iyi giriş düzeyi deneyimi sunar. Standart SSD daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve HDD disklere karşılaştırıldığında gecikme süresi sunar. Standart SSD, Web sunucuları, düşük IOPS uygulama sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/Test iş yükleri için uygundur.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Standart SSD GÇ işlemlerinin çoğu için Tek haneli milisaniyelik gecikme süreleri sağlamak ve IOPS ve aktarım hızı önceki tabloda örneklerin % 99 oranında açıklanan limitlerde sunmak için tasarlanmıştır. Bazen gerçek IOPS ve aktarım hızı trafik düzenlerini bağlı olarak değişiklik gösterebilir. Standart SSD daha düşük gecikme süresiyle HDD diskleri daha tutarlı performans sağlar.

## <a name="standard-hdd"></a>Standart HDD

Azure standart HDD, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Ayrıca, BLOB'ları, tabloları, kuyrukları ve dosyaları destekler. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. Vm'lerle çalışırken, geliştirme/test senaryoları için ve kritik iş yüklerini daha az SSD ve HDD standart diskleri kullanabilirsiniz. Standart depolama, tüm Azure bölgelerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

## <a name="billing"></a>Faturalama

Yönetilen diskleri kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

- Disk türü
- Yönetilen disk boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- İşlem sayısı

**Yönetilen disk boyutu**: yönetilen diskler ile sağlanan boyutu faturalandırılır. Azure (en yakın önerilen disk boyutuna yuvarlanır) sağlanan boyut eşler. Sağlanan disk boyutları Ayrıntılar için önceki tablolara bakın. Her disk, desteklenen sağlanan disk boyutu teklifine eşlenir ve buna göre faturalandırılır. Örneğin, bu E15 disk boyutu teklifine eşleyen bir 200 GiB standart SSD sağladıysanız, (256 GiB). Sağlanmış bir diski için faturalama, saatlere Premium depolama teklif için aylık fiyat kullanılarak eşit olarak dağıtılır. E10 bir disk sağlanır ve 20 saat sonra silindi, için E10 teklif 20 saat eşit olarak dağıtılır. Örneğin, faturalandırılırsınız. Diske yazılan gerçek veri miktarından bağımsız olarak budur.

**Anlık görüntüleri**: Anlık görüntüler, kullanılan boyutuna göre faturalandırılır. Örneğin, anlık görüntü sağlanan kapasitesi 64 GiB ve gerçek kullanılan veri boyutu 10 GiB ile bir yönetilen diskin anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu 10 GiB için faturalandırılır.