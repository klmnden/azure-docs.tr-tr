---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: bb9a2a884439b00f52adfa9b7c1010a4610a77f7
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47401611"
---
# <a name="ultra-ssd-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Ultra yüksek SSD (Önizleme) yönetilen diskler için Azure sanal makine iş yükleri

Azure Ultra SSD (Önizleme), Azure Iaas Vm'leri için yüksek performans, yüksek IOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunar. Bu yeni teklif, var olan diskleri tekliflerimizi aynı kullanılabilirlik düzeylerinde satırı performansının üst sağlar. Ultra yüksek SSD ek avantajları sanal makinelerinizi yeniden başlatmaya gerek kalmadan iş yüklerinizi yanı sıra disk performansını dinamik olarak değiştirme özelliği içerir. Ultra yüksek SSD, SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur.

## <a name="ultra-ssd-features"></a>Ultra yüksek SSD özellikleri

**Yönetilen diskler**: Ultra SSD bulunan ve yalnızca yönetilen diskler. Ultra yüksek SSD'ler, bir yönetilmeyen Disk veya sayfa blobu olarak dağıtılamıyor. Bir yönetilen diski oluştururken, disk sku UltraSSD_LRS yazın ve IOPS disk boyutunu belirtmek ve ihtiyacınız olan aktarım hızı ve Azure oluşturur ve diski oluşturup yönetebilmesi belirtin.  

**Sanal makineler**: Ultra SSD tüm Premium SSD etkin Azure sanal makine SKU'larıyla çalışma için tasarlanmıştır Önizleme zaman VM boyutlarını ES/DS v3 VM örnekleri için sınırlı olacaktır ancak.

**Dinamik performans Yapılandırması**: Ultra SSD sanal makinelerinizi yeniden başlatmaya gerek kalmadan diskin iş yükü gereksinimlerinize yanı sıra performans (IOPS ve aktarım hızı) dinamik olarak değiştirmenize olanak tanır.

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve performans hedefleri

Ultra yüksek bir SSD sağladığınızda, kapasite ve performans diskin bağımsız olarak yapılandırma seçeneğiniz olur. Ultra yüksek SSD birkaç sabit boyutta 4 GiB 64 Tib'a kadar gelir ve IOPS ve aktarım hızını bağımsız olarak yapılandırmanıza olanak tanıyan esnek performans yapılandırma modeli özelliği. Ultra yüksek SSD'ler, yalnızca veri diskleri yararlanılabilir. İşletim sistemi diski Premium SSD kullanmanızı öneririz.

Ultra yüksek SSD, bazı temel işlevler şunlardır:

- Disk kapasitesi: Ultra SSD, bir dizi farklı disk boyutları 64 Tib'a kadar 4 GiB sunuyor.
- Disk IOPS: Ultra SSD 300 IOPS/GiB, 160 K IOPS disk başına en fazla IOPS sınırları destekler. Sağladığınız IOPS elde etmek için seçili Disk IOPS VM IOPS değerinden küçük olmasını sağlayın. Minimum IOPS 100 IOPS disktir.
- Disk aktarım hızı: en çok 2000 MB disk başına IOPS, sağlanan her Ultra SSD'ler ile tek bir disk aktarım hızı sınırı 256 KiB/sn aranır (burada MB/sn = 10 ^ 6 bayt / saniye). Minimum disk aktarım hızı 1 MiB ' dir.

Aşağıdaki tabloda farklı disk boyutları için farklı yapılandırmalar özetlenmiştir:  

### <a name="ultra-ssd-managed-disk-offerings"></a>Ultra SSD Yönetilen Disk Teklifleri

|Disk Boyutu (GiB)  |IOPS Caps  |Aktarım hızı sınırı (MB/sn)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19.200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1024-65.536 (boyutları 1 TiB artışlarla artan bu aralıkta)     |160,000         |2,000         |

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Ultra yüksek SSD kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

- Yönetilen disk boyutu
- Yönetilen diskin sağlanan IOPS
- Yönetilen diskin sağlanan aktarım hızı
- Ultra yüksek SSD VM ayırma ücreti

### <a name="managed-disk-size"></a>Yönetilen disk boyutu

Yönetilen diskler, sağlanan boyutları faturalandırılır. Azure sağlanan boyutu (yukarı yuvarlayarak) en yakın disk boyutu teklifiyle eşleştirir. Sağlanan disk boyutları Ayrıntılar için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın. Her disk desteklenen sağlanan disk boyutuna eşler ve saatlik olarak buna uygun olarak faturalandırılır. Örneğin, bir 200 GiB Ultra SSD Disk sağlanır ve 20 saat sonra silindi, 256 GiB disk boyutu teklifine eşler ve 256 GiB için 20 saat için faturalandırılırsınız. Diske yazılan gerçek veri miktarından bağımsız olarak budur.

### <a name="managed-disk-provisioned-iops"></a>Yönetilen diskin sağlanan IOPS

IOPS uygulamanızı diske bir saniyede gönderdiği isteklerinin sayısı ' dir. Bir giriş/çıkış işlem sıralı veya rastgele olabilir, okur veya yazar. Disk boyutu gibi sağlanan IOPS saatlik olarak faturalandırılır. Sağlanan IOPS disk ayrıntıları için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın.

### <a name="managed-disk-provisioned-throughput"></a>Yönetilen diskin sağlanan aktarım hızı

Uygulamanızı bayt/saniye cinsinden ölçülen diskleri belirli bir aralıktaki gönderdiği veri miktarını aktarım hızıdır. Uygulamanızın büyük giriş/çıkış işlemi gerçekleştiriliyorsa, yüksek aktarım hızı gerektirir.  

Aşağıdaki formülde gösterilen şekilde aktarım hızı ve IOPS arasında bir ilişki olduğunu: x g/ç boyutu, IOPS aktarım hızı =

Bu nedenle, uygulamanızın ihtiyaç duyduğu en iyi aktarım hızı ve IOPS değerlerini belirlemek önemlidir. Bir en iyi duruma getirmek çalışırken, diğeri de etkilenir. Aktarım hızı 16 KiB g/ç boyutuna karşılık gelen ile başlayan ve daha fazla aktarım hızı gerekirse ayarlama öneririz.

Desteklenen disk aktarım hızı Ultra SSD üzerinde hakkında daha fazla bilgi için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın. Disk boyutu ve IOPS gibi sağlanan aktarım hızı sağlanmış MB/sn başına saatlik olarak faturalandırılır.

### <a name="ultra-ssd-vm-reservation-fee"></a>Ultra yüksek SSD VM ayırma ücreti

Sanal makinenizin Ultra SSD uyumlu olduğunu gösteren VM üzerinde bir özellik sunuyoruz. Ultra yüksek bir SSD uyumlu VM'nin ayrılmış bant genişliği kapasitesi işlem VM örneği arasındaki gecikme süresini azaltmak ve performansı iyileştirmek için blok depolama ölçek birimi ayırır. Bu özellik, VM sonuçlarına Ultra SSD disk eklemeden VM'de Ultra SSD özelliği etkinse, yalnızca uygulanan rezervasyon ücreti ekleniyor. Ultra yüksek SSD disk Ultra SSD uyumlu VM'ye bağlı olduğu zaman bu ücret uygulanır değil. Bu ücretlendirme, VM üzerinde sağlanan vCPU başına yapılır.

Başvurmak [Azure fiyatlandırma sayfasını diskler](https://azure.microsoft.com/pricing/details/managed-disks/) yeni Ultra SSD diskleri fiyatı ayrıntıları sınırlı önizlemede kullanılabilir.

### <a name="ultra-ssd-preview-scope-and-limitations"></a>Ultra yüksek SSD Önizleme kapsamı ve sınırlamalar

Önizleme süresince, Ultra SSD diskleri:

- Doğu ABD 2 tek bir kullanılabilirlik alanında başlangıçta desteklenecektir.  
- Yalnızca (kullanılabilirlik kümeleri ve Ultra yüksek bir SSD Disk ekleme yeteneğinin yok tek VM dağıtımları bölgeleri dışında) kullanılabilirlik alanları ile kullanılabilir
- ES/DS v3 Vm'lerde yalnızca desteklenir
- Veri diskleri ve yalnızca destek 4 k fiziksel kesim boyutu yalnızca kullanılabilir  
- Yalnızca boş diskler olarak oluşturulabilir  
- Şu anda yalnızca Resource Manager şablonları, CLI ve Python SDK'sı kullanılarak dağıtılabilir.
- Destek değil disk (henüz) anlık görüntüleri, VM görüntüleri, kullanılabilirlik kümeleri, olacak sanal makine ölçek kümeleri ve Azure Disk şifrelemesi.
- Yapamazsınız (henüz) Azure Backup'ı veya Azure Site Recovery ile tümleştirmeyi destekler.
- Olduğu gibi [çoğu önizlemeler](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bu özellik genel kullanılabilirlik (GA kadar) üretim iş yükleri için kullanılmamalıdır.