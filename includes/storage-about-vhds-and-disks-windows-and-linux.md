---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: aa740cfb203f50dc97a06359774dae367a20252b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60406502"
---
## <a name="about-vhds"></a>VHD'ler hakkında

Azure’da kullanılan VHD’ler, Azure’daki standart veya premium depolama hesabında sayfa blobları olarak depolanır. Sayfa blobları hakkında bilgi için bkz. [Blok bloblarını ve sayfa bloblarını anlama](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Premium depolama hakkında daha fazla ayrıntı için bkz. [Yüksek performanslı premium depolama ve Azure VM'leri](../articles/virtual-machines/windows/premium-storage.md).

Azure, sabit bir disk VHD biçimini destekler. Sabit biçim, mantıksal diski dosya içinde doğrusal olarak düzenlediğinden, disk farkı X'in içeriği blob farkı X konumunda depolanır. Blob'un sonundaki küçük bir alt bilgi VHD'nin özelliklerini tanımlar. Genellikle, çünkü çoğu diskte kullanılmayan büyük aralıklar Yürütülebilirler içlerinde sabit biçimli wastes boşluk. Ancak, Azure .vhd dosyalarını seyrek biçimde depoladığından aynı anda hem sabit hem de dinamik disklerin avantajlarından yararlanırsınız. Daha fazla bilgi için [sanal sabit diskleri kullanmaya başlama](https://technet.microsoft.com/library/dd979539.aspx).

Azure'da disk veya görüntü oluşturmak için bir kaynak olarak kullanmak istediğiniz tüm VHD dosyaları salt okunur, .vhd dosyaları karşıya veya Azure depolama alanına (hangi salt okunur veya salt okunur olabilir) kullanıcı tarafından kopyalanan dışında. Bir disk veya görüntü oluşturduğunuzda, Azure .vhd dosyalarının kaynak kopyasını getirir. Bu kopyalar, VHD’yi nasıl kullandığınıza bağlı olarak salt okunur veya okuma-yazma niteliktedir.

Bir görüntüden sanal makine oluşturduğunuzda Azure, sanal makine için kaynak .vhd dosyasının kopyası olan bir disk oluşturur. Yanlışlıkla silmeye karşı korumak üzere Azure, bir görüntü, işletim sistemi diski ya da veri diski oluşturmak için kullanılan her kaynak .vhd dosyasına kira koyar.

Bir kaynak .vhd dosyasını silmeden önce diski veya görüntüyü silerek kirayı kaldırmanız gerekir. Sanal makine tarafından işletim sistemi diski olarak kullanılan bir .vhd dosyasını silmek için, sanal makineyi ve ilişkili tüm diskleri silerek sanal makineyi, işletim sistemi diskini ve kaynak .vhd dosyasını tek seferde silebilirsiniz. Ancak, bir veri diskinin kaynağı olan .vhd dosyasının silinmesi, belirli bir sırada birkaç adımın uygulanmasını gerektirir. İlk olarak, diski sanal makineden ayırın, ardından diski ve sonra .vhd dosyasını silin.

> [!WARNING]
> Kaynak .vhd dosyasını depolama alanından silerseniz veya depolama hesabınızı silerseniz, Microsoft bu verileri kurtaramaz.

## <a name="types-of-disks"></a>Disk türleri

Azure Diskleri %99,999 kullanılabilirlik sunacak şekilde tasarlanmıştır. Azure diskleri ile sektör lideri Kurumsal düzeyde dayanıklılık sunarak hata oranı % tutarlı bir şekilde teslim.

Disklerinizi Premium SSD diskleri, standart SSD ve HDD depolama standart oluştururken seçebileceğiniz depolama için üç performans katmanı vardır. Ayrıca, yönetilmeyen ve yönetilen diskler--iki tür vardır.

### <a name="standard-hdd-disks"></a>Standart HDD diskler

Standart HDD diskler, HDD'ler ile desteklenir ve düşük maliyetli depolama sunar. Standart HDD depolama bir veri merkezinde yerel olarak çoğaltılabilir veya birincil ve ikincil veri merkezleri ile coğrafi olarak yedekli. Depolama çoğaltma hakkında daha fazla bilgi için bkz. [Azure depolama çoğaltma](../articles/storage/common/storage-redundancy.md).

Standart HDD diskleri kullanma hakkında daha fazla bilgi için bkz. [standart depolama ve diskler](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks"></a>Standart SSD disk

Standart SSD disk standart HDD diskleri aynı türde iş yükleri adres, ancak daha tutarlı performans ve güvenilirlik HDD sunmak üzere tasarlanmıştır. Standart SSD disk Premium SSD diskleri ve uygun maliyetli bir çözüme en iyi forma standart HDD disklerinin yüksek IOPS disklerde gerekmeyen web sunucuları gibi uygulamalar için uygun öğeleri birleştirin. Standart SSD disk, mümkün olan durumlarda iş yüklerinin çoğu için önerilen dağıtım seçeneği olan. Standart SSD disk olarak yönetilen diskler tüm bölgelerde kullanılabilir, ancak şu anda yalnızca yerel olarak yedekli depolama (LRS) dayanıklılık türü ile kullanılabilir.

### <a name="premium-ssd-disks"></a>Premium SSD diskleri

Premium SSD diskleri, SSD'ler ile desteklenir ve g/Ç açısından yoğun iş yükleri için yüksek performanslı, düşük gecikme süreli disk desteği sunar. Genellikle serisi adında "s" boyutları ile Premium SSD diskleri kullanabilirsiniz. Örneğin, Dv3 serisi yoktur ve Dsv3 serisi, Dsv3 serisi Premium SSD diskleri ile kullanılabilir.  Daha fazla bilgi için bkz. [Premium Depolama](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Yönetilmeyen diskler, VM'ler tarafından kullanılan geleneksel türdeki disklerdir. Bu disklerle kendi depolama hesabı oluşturun ve diski oluştururken bu depolama hesabı belirtin. Neden olmayan yerleştirdiğiniz çok fazla disk aynı depolama hesabında emin [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) depolama hesabının (örneğin 20.000 IOPS) aşarak VM'lerin kaynaklanan. Yönetilmeyen disklerde, VM’lerinizden en iyi performansı elde etmek için, bir veya daha fazla depolama hesabının kullanımını nasıl en üst düzeye çıkarabileceğinizi anlamanız gerekir.

### <a name="managed-disks"></a>Yönetilen diskler

Yönetilen Diskler, depolama hesabı oluşturma/yönetme işlemini arka planda gerçekleştirir ve depolama hesabının ölçeklenebilirlik sınırları hakkında endişe etmeniz gerekmez. Azure’un diski oluşturup yönetebilmesi için disk boyutunu ve performans katmanını (Standart/Premium) belirtmeniz yeterlidir. Disk eklediğinizde veya VM ölçeğini artırıp azalttığınızda kullanılan depolama alanı konusunda endişelenmeniz gerekmez.

Ayrıca, her Azure bölgesinde bir depolama hesabındaki özel görüntülerinizi yönetebilir ve aynı abonelikte yüzlerce VM oluşturmak için kullanabilirsiniz. Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Yönetilen Disklere Genel Bakış](../articles/virtual-machines/windows/managed-disks-overview.md).

Yeni VM’ler için Azure Yönetilen Diskleri kullanmanız ve Yönetilen Disklerde sunulan çok sayıda özellikten yararlanmak için önceki yönetilmeyen diskleri yönetilen disklere dönüştürmeniz önerilir.

### <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tablo standart HDD, standart bir SSD ve yönetilen ve yönetilmeyen diskler için Premium SSD kullanacağınız seçeneğe karar vermenize yardımcı olacak bir karşılaştırma sağlar. Bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır.

|    | Azure Premium Disk |Azure standart SSD Disk | Azure standart HDD Disk
|--- | ------------------ | ------------------------------- | -----------------------
| Disk Türü | Katı Hal Sürücüleri (SSD) | Katı Hal Sürücüleri (SSD) | Sabit Disk Sürücüleri (HDD)  
| Genel Bakış  | G/Ç yoğunluklu iş yükleri çalıştıran veya görev açısından kritik üretim ortamı barındıran VM’ler için SSD tabanlı, yüksek performans ve düşük gecikme süresi sunan disk desteği |Daha tutarlı performans ve güvenilirlik HDD. Düşük IOPS iş yükleri için iyileştirilmiş| Seyrek erişim için uygun maliyetli disk HDD tabanlı
| Senaryo  | Üretim ve performansa duyarlı iş yükleri |Web sunucuları, az kullanılan Kurumsal uygulama ve geliştirme/Test| Yedekleme, kritik olmayan, seyrek erişim
| Disk Boyutu | P4: 32 giB (yalnızca yönetilen diskler)<br>P6: 64 giB (yalnızca yönetilen diskler)<br>P10: 128 GiB<br>P15: 256 giB (yalnızca yönetilen diskler)<br>P20: 512 GiB<br>P30: 1024 giB<br>P40: 2048 giB<br>P50: 4.095 giB<br>P60: 8192 giB * (8 tib'a kadar)<br>P70: 16.384 giB * (16 tib'a kadar)<br>P80: 32.767 giB * (32 tib'a kadar) |Yalnızca yönetilen diskler:<br>E4: 32 GiB<br>E6: 64 GiB<br>E10: 128 GiB<br>E15: 256 GiB<br>E20: 512 GiB<br>E30: 1024 giB<br>E40: 2048 giB<br>E50: 4095 giB<br>E60: 8192 giB * (8 tib'a kadar)<br>E70: 16.384 giB * (16 tib'a kadar)<br> E80: 32.767 giB * (32 tib'a kadar) | Yönetilmeyen diskler: 1 giB – 4 TiB (4095 GiB) <br><br>Yönetilen Diskler:<br> S4: 32 GiB <br>S6: 64 GiB <br>S10: 128 GiB <br>S15: 256 GiB <br>S20: 512 GiB <br>S30: 1024 giB <br>S40: 2048 giB<br>S50: 4095 giB<br>S60: 8192 giB * (8 tib'a kadar)<br>S70: 16.384 giB * (16 tib'a kadar)<br>S80: 32.767 giB * (32 tib'a kadar)
| Disk Başına En Fazla Aktarım Hızı | P4: 25 MiB/s<br> P6: 50 MiB/s<br> P10: 100 MiB/s<br> P15: 125 MiB/s<br> P20: 150 MiB/s<br> P30: 200 MiB/s<br> P40 P50: 250 MiB/s<br> P60: 480 MiB/s *<br> P70 P80: 750 MiB/s * | E10-E50: En fazla 60 MiB/sn<br> E60: En fazla 300 MiB/sn *<br> E70-E80: 500 MiB/s *| S4 - S50: Upt o 60 MiB/sn<br> S60: En fazla 300 MiB/sn *<br> S70-S80: En fazla 500 MiB/sn *
| Disk başına en fazla IOPS | P4: 120 IOPS<br> P6: 240 IOPS<br> P10: 500 IOPS<br> P15: 1100 IOPS<br> P20: 2300 IOPS<br> P30: 5000 IOPS<br> P40 P50: 7500 IOPS<br> P60: 12.500 IOPS *<br> P70: 15.000 IOPS *<br> P80: 20.000 IOPS * | E10-E50: En fazla 500 IOPS<br> E60: 1300 IOPS kadar *<br> E70-E80: 2000 IOPS kadar * | S4-S50: En fazla 500 IOPS<br> S60: 1300 IOPS kadar *<br> S70-S80: 2000 IOPS kadar *
