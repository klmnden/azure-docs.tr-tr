---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c7b73cad200666db9e926d8e808eaa4a8dccffb2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248950"
---
## <a name="premium-ssd"></a>Premium SSD

Azure premium SSD giriş/çıkış (GÇ) ile sanal makineleri (VM'ler) için yüksek performanslı ve düşük gecikme süreli disk desteği sunmak-yoğun iş yükleri. Hızı avantajından ve premium depolama disklerini performansını yararlanmak için var olan VM diskleri için Premium SSD geçirebilirsiniz. Premium SSD, görev açısından kritik üretim uygulamaları için uygundur. Premium SSD yalnızca premium depolama ile uyumlu olan VM serisi ile kullanılabilir.

Tek VM türleri ve Azure için premium depolama ile uyumlu olan hangi boyutları da dahil olmak üzere Windows, boyutları hakkında daha fazla bilgi için bkz. [Windows VM boyutları](../articles/virtual-machines/windows/sizes.md). Premium depolama ile uyumlu olan hangi boyutları da dahil olmak üzere Linux için ayrı VM türleri ve boyutları Azure hakkında daha fazla bilgi için bkz: [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md).

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Standart depolama aksine, bir premium depolama disk sağlarken, kapasite, IOPS ve aktarım hızı bu diskin garanti edilir. Örneğin, P50 disk oluşturursanız, Azure 4.095 GB depolama kapasitesi, 7.500 IOPS ve 250-MB/sn aktarım hızı bu disk için hazırlar. Uygulamanız, kapasite ve performans bölümünü veya tümünü kullanabilirsiniz. Premium SSD diskleri düşük tek basamaklı milisaniyelik gecikme süreleri sağlamak ve hedef IOPS ve aktarım hızı önceki tabloda % 99,9 oranında içinde açıklanan şekilde tasarlanmıştır.

### <a name="transactions"></a>İşlemler

Premium SSD için her g/ç işlemi daha az veya eşit 256 tek bir g/ç işleme aktarım hızının KiB olarak kabul edilir. Aktarım hızı 256 KiB daha büyük g/ç işlemleri birden çok g/ç boyutu 256 değerlendirilir KiB.

## <a name="standard-ssd"></a>Standart SSD

Azure standart SSD'ler düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için en iyi duruma getirilmiş düşük maliyetli depolama bir seçenektir. Standart SSD, özellikle varyansını HDD çözümlerinizi şirket içinde çalışan iş yükleri ile ilgili sorun yaşıyorsanız kullanıcıların buluta taşımak istediğiniz iyi giriş düzeyi deneyimi sunar. Standart Hdd'lere karşılaştırıldığında, standart SSD'ler daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve gecikme süresi sunar. Standart SSD, Web sunucuları, düşük IOPS uygulama sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/Test iş yükleri için uygundur. Standart HDD'ler gibi standart SSD'ler, tüm Azure Vm'leri üzerinde kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

Standart SSD Tek haneli milisaniyelik gecikme süreleri ve IOPS ve aktarım hızı önceki tabloda örneklerin % 99 oranında açıklanan limitlerde sağlamak için tasarlanmıştır. Bazen gerçek IOPS ve aktarım hızı trafik düzenlerini bağlı olarak değişiklik gösterebilir. Standart SSD daha düşük gecikme süresiyle HDD diskleri daha tutarlı performans sağlar.

### <a name="transactions"></a>İşlemler

Standart SSD için her g/ç işlemi daha az veya eşit 256 tek bir g/ç işleme aktarım hızının KiB olarak kabul edilir. Aktarım hızı 256 KiB daha büyük g/ç işlemleri birden çok g/ç boyutu 256 değerlendirilir KiB. Bu işlem bir faturalama etkisi.

## <a name="standard-hdd"></a>Standart HDD

Azure standart HDD, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. Gecikme süresi, IOPS ve aktarım hızı, standart HDD diskleri diskler SSD tabanlı karşılaştırıldığında daha yaygın olarak değişiklik gösterebilir. Vm'lerle çalışırken, kritik iş yüklerini daha az ve geliştirme/test senaryoları için HDD standart diskleri kullanabilirsiniz. Standart HDD, tüm Azure bölgelerinde kullanılabilir ve tüm Azure Vm'leri ile kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>İşlemler

Standart HDD için her bir GÇ işlemi g/ç boyutundan bağımsız olarak tek bir işlem olarak kabul edilir. Bu işlem bir faturalama etkisi.

## <a name="billing"></a>Faturalandırma

Yönetilen diskleri kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

- Disk türü
- Yönetilen disk boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- İşlem sayısı

**Yönetilen disk boyutu**: yönetilen diskler ile sağlanan boyutu faturalandırılır. Azure (en yakın önerilen disk boyutuna yuvarlanır) sağlanan boyut eşler. Sağlanan disk boyutları Ayrıntılar için önceki tablolara bakın. Her disk, desteklenen sağlanan disk boyutu teklifine eşlenir ve buna göre faturalandırılır. Örneğin, bu E15 disk boyutu teklifine eşleyen bir 200 GiB standart SSD sağladıysanız, (256 GiB). Sağlanmış bir diski için faturalama, saatlere Premium depolama teklif için aylık fiyat kullanılarak eşit olarak dağıtılır. E10 bir disk sağlanır ve 20 saat sonra silindi, için E10 teklif 20 saat eşit olarak dağıtılır. Örneğin, faturalandırılırsınız. Diske yazılan gerçek veri miktarından bağımsız olarak budur.

**Anlık görüntüleri**: Anlık görüntüler, kullanılan boyutuna göre faturalandırılır. Örneğin, anlık görüntü sağlanan kapasitesi 64 GiB ve gerçek kullanılan veri boyutu 10 GiB ile bir yönetilen diskin anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu 10 GiB için faturalandırılır.
