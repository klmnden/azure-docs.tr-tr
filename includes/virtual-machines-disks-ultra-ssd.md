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
ms.openlocfilehash: 4b5d2de2e9ccd44517e083a435e127bd5678f002
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564349"
---
# <a name="ultra-disks-preview-managed-disks-for-azure-virtual-machine-workloads"></a>Yönetilen diskler Azure sanal makine iş yükleri için Ultra yüksek diskler (Önizleme)

Azure ultra diskler (Önizleme), Azure Iaas Vm'leri için yüksek performans, yüksek IOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunar. Bu yeni teklif, var olan diskleri tekliflerimizi aynı kullanılabilirlik düzeylerinde satırı performansının üst sağlar. Ek avantajlar Ultra yüksek disk performansı diskin sanal makinelerinizi yeniden başlatmaya gerek kalmadan iş yüklerinizi yanı sıra dinamik olarak değiştirmenize olanak özelliğine sahiptir. Ultra yüksek diskler, SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur.

## <a name="ultra-disk-features"></a>Ultra yüksek disk özellikleri

**Yönetilen diskler**: Ultra yüksek diskler yalnızca yönetilen diskleri olarak kullanılabilir. Ultra yüksek diskleri, bir yönetilmeyen disk veya sayfa blobu olarak dağıtılamıyor. Yönetilen disk oluşturulurken disk sku UltraSSD_LRS yazın ve IOPS disk boyutunu belirtmek ve ihtiyacınız olan aktarım hızı ve Azure oluşturur ve diski oluşturup yönetebilmesi belirtin.  

**Sanal makineler**: Ultra yüksek diskler, etkin Azure sanal makine SKU'ları tüm Premium SSD ile çalışacak şekilde tasarlanmıştır; Ancak, şu anda Önizleme aşamasında olduğu gibi sanal makinelerin ES/DS v3 boyutlandırılır.

**Dinamik performans Yapılandırması**: Ultra diskler, sanal makinelerinizi yeniden başlatmaya gerek kalmadan diskin iş yükü gereksinimlerinize yanı sıra performans (IOPS ve aktarım hızı) dinamik olarak değiştirmenize olanak tanır.

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve performans hedefleri

Ultra yüksek bir diskler sağladığınızda, kapasite ve performans diskin bağımsız olarak yapılandırma seçeneğiniz olur. Ultra yüksek diskler birkaç sabit boyutta 4 GiB 64 Tib'a kadar gelir ve IOPS ve aktarım hızını bağımsız olarak yapılandırmanıza olanak tanıyan esnek performans yapılandırma modeli özelliği. Ultra yüksek diskler yalnızca veri diskleri yararlanılabilir. İşletim sistemi diski Premium SSD kullanmanızı öneririz.

Ultra yüksek disk bazı temel işlevler şunlardır:

- Disk kapasitesi: Ultra yüksek diskleri sunar, bir dizi farklı disk boyutları 64 Tib'a kadar 4 GiB öğesinden.
- Disk IOPS: Ultra yüksek diskler, 300 IOPS/GiB, 160 K IOPS disk başına en fazla IOPS sınırları destekler. Sağladığınız IOPS elde etmek için seçili Disk IOPS VM IOPS değerinden küçük olmasını sağlayın. Minimum IOPS 100 IOPS disktir.
- Disk aktarım hızı: Her 2000 MB disk başına en fazla IOPS sağlanması için ultra disklerle olan tek bir disk aktarım hızı sınırı 256 KiB/sn olan (burada MB/sn = 10 ^ 6 bayt / saniye). Minimum disk aktarım hızı 1 MiB ' dir.

Aşağıdaki tabloda farklı disk boyutları için farklı yapılandırmalar özetlenmiştir:  

### <a name="ultra-disks-managed-disk-offerings"></a>Ultra yüksek diskler yönetilen disk teklifleri

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

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Ultra yüksek diskleri kullanırken aşağıdaki fatura değerlendirmeleri uygulanacak:

- Yönetilen disk boyutu
- Yönetilen diskin sağlanan IOPS
- Yönetilen diskin sağlanan aktarım hızı
- Ultra yüksek disk VM ayırma ücreti

### <a name="managed-disk-size"></a>Yönetilen disk boyutu

Yönetilen diskler, yeni bir Azure sanal makine sağlanırken seçtiğiniz VM boyutlarına faturalandırılır. Azure sağlanan boyutu (yukarı yuvarlayarak) en yakın disk boyutu teklifiyle eşleştirir. Sağlanan disk boyutları Ayrıntılar için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın. Her disk için bir desteklenen sağlanan disk boyutu ve buna göre faturalandırılır eşler saatlik olarak. Örneğin, 200 GiB Ultra yüksek disk sağlanır ve 20 saat sonra silindi, 256 GiB disk boyutu teklifine eşler ve 256 GiB için 20 saat için faturalandırılırsınız. Bu faturalandırma gerçekten diske yazılan veri hacmi bağımsız olarak işlem saati tüketimine temel alınmıştır.

### <a name="managed-disk-provisioned-iops"></a>Yönetilen disk IOPS sağlanması

IOPS, uygulamanızın gönderdiği istekleri dizi için saniye başına disk sayısı. Bir giriş/çıkış işlemi, sıralı bir okuma veya yazma veya rastgele okuma veya yazma. Ortalama IOPS sayısı, disk boyutu veya VM'ye disk sayısına göre saatlik olarak faturalandırılır. Sağlanan IOPS disk ayrıntıları için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın.

### <a name="managed-disk-provisioned-throughput"></a>Yönetilen diskin sağlanan aktarım hızı

Uygulamanızı bayt/saniye cinsinden ölçülen diskleri belirli bir aralıktaki gönderdiği veri miktarını aktarım hızıdır. Uygulamanızın büyük giriş/çıkış işlemi gerçekleştiriliyorsa, yüksek aktarım hızı gerektirir.  

Aktarım hızı ve IOPS aşağıdaki formülde gösterilen şekilde arasında bir ilişki yoktur:  GÇ boyutu x IOPS aktarım hızı =

Bu nedenle, uygulamanızın ihtiyaç duyduğu en iyi aktarım hızı ve IOPS değerlerini belirlemek önemlidir. Bir en iyi duruma getirmek çalışırken, diğeri de etkilenir. Aktarım hızı 16 KiB g/ç boyutuna karşılık gelen ile başlayan ve daha fazla aktarım hızı gerekirse ayarlama öneririz.

Desteklenen disk aktarım hızı ultra disklerde hakkında daha fazla bilgi için ölçeklenebilirlik ve performans hedefleri bölümde yukarıdaki tabloya bakın. Disk boyutu ve IOPS gibi sağlanan aktarım hızı sağlanmış MB/sn başına saatlik olarak faturalandırılır.

### <a name="ultra-disk-vm-reservation-fee"></a>Ultra yüksek disk VM ayırma ücreti

VM'nizi Ultra yüksek disk uyumlu olduğunu gösteren VM üzerinde bir özellik sunuyoruz. Uyumlu sanal makine ayıran bir Ultra yüksek disk adanmış işlem VM örneği ve performansı iyileştirmek ve gecikme süresini azaltmak için blok depolama ölçek birimi arasında bant genişliği kapasitesi. Ultra yüksek bir disk eklemeden VM'de Ultra yüksek disk özelliği etkinse, yalnızca uygulanan rezervasyon ücreti VM sonuçları bu özellik ekleniyor. Ultra yüksek bir disk için Ultra yüksek uyumlu VM'nin bağlı olduğu zaman bu ücret uygulanır değil. Bu ücretlendirme, VM üzerinde sağlanan vCPU başına yapılır.

Başvurmak [Azure fiyatlandırma sayfasını diskler](https://azure.microsoft.com/pricing/details/managed-disks/) yeni ultra diskleri fiyat ayrıntıları sınırlı önizlemede kullanılabilir.

### <a name="ultra-disk-preview-scope-and-limitations"></a>Ultra yüksek disk Önizleme kapsamı ve sınırlamalar

Önizleme süresince, ultra diskler:

- Doğu ABD 2 tek bir kullanılabilirlik alanında başlangıçta desteklenecektir.  
- Yalnızca (kullanılabilirlik kümeleri ve bölgeleri dışında tek VM dağıtımları Ultra yüksek bir diski olanağına sahip değildir) kullanılabilirlik alanları ile kullanılabilir
- ES/DS v3 Vm'lerde yalnızca desteklenir
- Veri diskleri ve yalnızca destek 4 k fiziksel kesim boyutu yalnızca kullanılabilir  
- Yalnızca boş diskler olarak oluşturulabilir  
- Şu anda yalnızca Resource Manager şablonları, CLI ve Python SDK'sı kullanılarak dağıtılabilir.
- Destek değil disk (henüz) anlık görüntüleri, VM görüntüleri, kullanılabilirlik kümeleri, olacak sanal makine ölçek kümeleri ve Azure Disk şifrelemesi.
- Yapamazsınız (henüz) Azure Backup'ı veya Azure Site Recovery ile tümleştirmeyi destekler.
- Olduğu gibi [çoğu önizlemeler](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bu özellik genel kullanılabilirlik (GA kadar) üretim iş yükleri için kullanılmamalıdır.
