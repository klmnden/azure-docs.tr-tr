---
title: include dosyası
description: include dosyası
services: storage
author: rogara
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f64645db782b055e1c544f257149411f29fc99d7
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "34806324"
---
## <a name="about-vhds"></a>VHD'ler hakkında
Azure’da kullanılan VHD’ler, Azure’daki standart veya premium depolama hesabında sayfa blobları olarak depolanır. Sayfa blobları hakkında bilgi için bkz. [Blok bloblarını ve sayfa bloblarını anlama](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Premium depolama hakkında daha fazla ayrıntı için bkz. [Yüksek performanslı premium depolama ve Azure VM'leri](../articles/virtual-machines/windows/premium-storage.md).

Azure, sabit bir disk VHD biçimini destekler. Sabit biçim, mantıksal diski dosya içinde doğrusal olarak düzenlediğinden, disk farkı X'in içeriği blob farkı X konumunda depolanır. Blob'un sonundaki küçük bir alt bilgi VHD'nin özelliklerini tanımlar. Genellikle, çoğu diskleri büyük kullanılmayan aralıkları olması nedeniyle sabit biçimli wastes boşluk. Ancak, Azure .vhd dosyalarını seyrek biçimde depoladığından aynı anda hem sabit hem de dinamik disklerin avantajlarından yararlanırsınız. Daha fazla bilgi için bkz: [sanal sabit diskler ile çalışmaya başlama](https://technet.microsoft.com/library/dd979539.aspx).

Diskleri veya görüntüleri oluşturmak için bir kaynak olarak kullanmak istediğiniz Azure tüm VHD dosyaları, salt okunur, .vhd dosyaları karşıya veya Azure depolama birimine (olabilen okuma-yazma veya salt okunur) kullanıcı tarafından kopyalanan dışında. Bir disk veya görüntü oluşturduğunuzda, Azure .vhd dosyalarını kaynağı kopyalarını oluşturur. Bu kopyalar, VHD’yi nasıl kullandığınıza bağlı olarak salt okunur veya okuma-yazma niteliktedir.

Bir görüntüden sanal makine oluşturduğunuzda Azure, sanal makine için kaynak .vhd dosyasının kopyası olan bir disk oluşturur. Yanlışlıkla silmeye karşı korumak üzere Azure, bir görüntü, işletim sistemi diski ya da veri diski oluşturmak için kullanılan her kaynak .vhd dosyasına kira koyar.

Bir kaynak .vhd dosyasını silmeden önce diski veya görüntüyü silerek kirayı kaldırmanız gerekir. Sanal makine tarafından işletim sistemi diski olarak kullanılan bir .vhd dosyasını silmek için, sanal makineyi ve ilişkili tüm diskleri silerek sanal makineyi, işletim sistemi diskini ve kaynak .vhd dosyasını tek seferde silebilirsiniz. Ancak, bir veri diskinin kaynağı olan .vhd dosyasının silinmesi, belirli bir sırada birkaç adımın uygulanmasını gerektirir. İlk olarak, diski sanal makineden ayırın, ardından diski ve sonra .vhd dosyasını silin.

> [!WARNING]
> Kaynak .vhd dosyasını depolama alanından silerseniz veya depolama hesabınızı silerseniz, Microsoft bu verileri kurtaramaz.

## <a name="types-of-disks"></a>Disk türleri 
Azure Diskleri %99,999 kullanılabilirlik sunacak şekilde tasarlanmıştır. Azure diskleri tutarlı bir şekilde endüstri lideri ile Kurumsal düzeyde dayanıklılık sıfır % Annualized hata oranı teslim.

Disklerinizi--Premium SSD diskleri, standart SSD (Önizleme) ve standart HDD depolama oluştururken seçebileceğiniz depolama için üç performans katmanı vardır. Ayrıca, yönetilmeyen ve yönetilen diskleri--iki tür vardır.

### <a name="standard-hdd-disks"></a>Standart HDD diskler
Standart HDD diskler HDD tarafından desteklenir ve düşük maliyetli depolama teslim. Standart HDD depolama yerel olarak bir veri merkezine çoğaltılabilir veya coğrafi birincil ve ikincil veri merkezleri ile yedekli. Depolama çoğaltma hakkında daha fazla bilgi için bkz: [Azure Storage çoğaltma](../articles/storage/common/storage-redundancy.md). 

Standart HDD diskleri kullanma hakkında daha fazla bilgi için bkz: [standart depolama ve disk](../articles/virtual-machines/windows/standard-storage.md).

### <a name="standard-ssd-disks-preview"></a>Standart SSD diskleri (Önizleme)
Standart SSD diskleri standart HDD diskleri aynı türdeki iş yüklerini adres, ancak daha tutarlı performans ve güvenilirlik HDD sunmak üzere tasarlanmıştır. Standart SSD diskleri disklerde yüksek IOPS gerekmez web sunucuları gibi uygulamalar için uygun öğeleri Premium SSD diskleri ve uygun maliyetli bir çözüm en iyi forma standart HDD disklerinin birleştirin. Kullanılabilir olduğunda, standart SSD diskleri çoğu iş yükleri için önerilen dağıtım seçenektir. Standart SSD diskleri yalnızca yönetilen diskleri olarak kullanılabilir ve while Önizleme'de yalnızca kullanılabilir [seçin bölgeleri](../articles/virtual-machines/windows/faq-for-disks.md) ile yerel olarak yedekli depolama (LRS) dayanıklılık türü.

### <a name="premium-ssd-disks"></a>Premium SSD diskleri 
Premium SSD diskleri SSD tarafından desteklenir ve g/Ç kullanımı yoğun iş yükleri çalıştıran VM'ler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Genellikle seri adında bir "s" boyutlarıyla Premium SSD diskleri kullanabilirsiniz. Örneğin, Dv3 serisi yoktur ve Dsv3 serisi, Dsv3 serisi Premium SSD disk ile kullanılabilir.  Daha fazla bilgi için bkz. [Premium Depolama](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler
Yönetilmeyen diskler, VM'ler tarafından kullanılan geleneksel türdeki disklerdir. Bu diskleri kendi depolama hesabı oluşturun ve disk oluşturduğunuzda, bu depolama hesabı belirtin. Aştığından aynı depolama hesabında çok sayıda disk put yok emin olun [ölçeklenebilirlik hedefleri](../articles/storage/common/storage-scalability-targets.md) depolama hesabının (örneğin 20.000 IOPS) karşılaşıldığı VM'ler sonuçlanır. Yönetilmeyen disklerde, VM’lerinizden en iyi performansı elde etmek için, bir veya daha fazla depolama hesabının kullanımını nasıl en üst düzeye çıkarabileceğinizi anlamanız gerekir.

### <a name="managed-disks"></a>Yönetilen diskler 
Yönetilen Diskler, depolama hesabı oluşturma/yönetme işlemini arka planda gerçekleştirir ve depolama hesabının ölçeklenebilirlik sınırları hakkında endişe etmeniz gerekmez. Azure’un diski oluşturup yönetebilmesi için disk boyutunu ve performans katmanını (Standart/Premium) belirtmeniz yeterlidir. Disk eklediğinizde veya VM ölçeğini artırıp azalttığınızda kullanılan depolama alanı konusunda endişelenmeniz gerekmez. 

Ayrıca, her Azure bölgesinde bir depolama hesabındaki özel görüntülerinizi yönetebilir ve aynı abonelikte yüzlerce VM oluşturmak için kullanabilirsiniz. Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Yönetilen Disklere Genel Bakış](../articles/virtual-machines/windows/managed-disks-overview.md).

Yeni VM’ler için Azure Yönetilen Diskleri kullanmanız ve Yönetilen Disklerde sunulan çok sayıda özellikten yararlanmak için önceki yönetilmeyen diskleri yönetilen disklere dönüştürmeniz önerilir.

### <a name="disk-comparison"></a>Disk karşılaştırması
Aşağıdaki tabloda standart HDD, standart SSD ve yönetilen ve yönetilmeyen diskler için Premium SSD ne kullanmaya karar vermenize yardımcı olacak karşılaştırması sağlar.

|    | Azure Premium Disk |Azure standart SSD Disk (Önizleme)| Azure standart HDD Disk 
|--- | ------------------ | ------------------------------- | ----------------------- 
| Disk Türü | Katı Hal Sürücüleri (SSD) | Katı Hal Sürücüleri (SSD) | Sabit Disk Sürücüleri (HDD)  
| Genel Bakış  | G/Ç yoğunluklu iş yükleri çalıştıran veya görev açısından kritik üretim ortamı barındıran VM’ler için SSD tabanlı, yüksek performans ve düşük gecikme süresi sunan disk desteği |Daha tutarlı performansı ve güvenilirliği HDD daha. Düşük IOPS iş yükleri için en iyi duruma getirilmiş| Sık erişim için uygun maliyetli disk HDD tabanlı
| Senaryo  | Üretim ve performansa duyarlı iş yükleri |Web sunucuları, hafifçe kullanılan Kurumsal uygulama ve geliştirme ve Test| Yedekleme, kritik olmayan, sık erişim 
| Disk Boyutu | P4: 32 Gib'den (yalnızca yönetilen diskleri)<br>P6: 64 Gib'den (yalnızca yönetilen diskleri)<br>P10: 128 Gib'den<br>P15: 256 Gib'den (yalnızca yönetilen diskleri)<br>P20: 512 Gib'den<br>P30: 1024 Gib'den<br>P40: 2048 Gib'den<br>P50: 4095 Gib'den |Yalnızca yönetilen disklerde:<br>E10: 128 Gib'den<br>E15: 256 Gib'den<br>E20: 512 Gib'den<br>E30: 1024 Gib'den<br>E40: 2048 Gib'den<br>E50: 4095 Gib'den | Yönetilmeyen diskler: 1 Gib'den – 4 Tıb (4095 GiB) <br><br>Yönetilen Diskler:<br> S4: 32 Gib'den <br>S6: 64 Gib'den <br>S10: 128 Gib'den <br>S15: 256 Gib'den <br>S20'in: 512 Gib'den <br>S30: 1024 Gib'den <br>S40: 2048 Gib'den<br>S50: 4095 Gib'den
| Disk Başına En Fazla Aktarım Hızı | 250 MIB/s | Fazla 60 MIB/s | Fazla 60 MIB/s 
| Disk başına en fazla IOPS | 7500 IOPS | Fazla 500 IOPS | Fazla 500 IOPS 