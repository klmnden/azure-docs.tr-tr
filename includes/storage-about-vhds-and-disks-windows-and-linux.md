---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b4d208ca28f6287489f104ba4e2ea9696e7a1f58
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
## <a name="about-vhds"></a>VHD'ler hakkında

Azure’da kullanılan VHD’ler, Azure’daki standart veya premium depolama hesabında sayfa blobları olarak depolanır. Sayfa blobları hakkında bilgi için bkz. [Blok bloblarını ve sayfa bloblarını anlama](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Premium depolama hakkında daha fazla ayrıntı için bkz. [Yüksek performanslı premium depolama ve Azure VM'leri](../articles/virtual-machines/windows/premium-storage.md).

Azure, sabit bir disk VHD biçimini destekler. Sabit biçim, mantıksal diski dosya içinde doğrusal olarak düzenlediğinden, disk farkı X'in içeriği blob farkı X konumunda depolanır. Blob'un sonundaki küçük bir alt bilgi VHD'nin özelliklerini tanımlar. Çoğunlukla, sabit biçim alanı israf eder, çünkü çoğu diskte kullanılmayan büyük aralıklar vardır. Ancak, Azure .vhd dosyalarını seyrek biçimde depoladığından aynı anda hem sabit hem de dinamik disklerin avantajlarından yararlanırsınız. Daha ayrıntılı bilgi için bkz. [Sanal sabit diskleri kullanmaya başlama](https://technet.microsoft.com/library/dd979539.aspx).

Diskleri veya görüntüleri oluşturmak için bir kaynak olarak kullanmak istediğiniz Azure tüm .vhd dosyaları salt okunur, .vhd dosyaları karşıya veya Azure depolama birimine (olabilen okuma-yazma veya salt okunur) kullanıcı tarafından kopyalanan dışında. Bir disk veya görüntü oluşturduğunuzda, Azure .vhd dosyalarını kaynağı kopyalarını oluşturur. Bu kopyalar, VHD’yi nasıl kullandığınıza bağlı olarak salt okunur veya okuma-yazma niteliktedir.

Bir görüntüden sanal makine oluşturduğunuzda Azure, sanal makine için kaynak .vhd dosyasının kopyası olan bir disk oluşturur. Yanlışlıkla silmeye karşı korumak üzere Azure, bir görüntü, işletim sistemi diski ya da veri diski oluşturmak için kullanılan her kaynak .vhd dosyasına kira koyar.

Bir kaynak .vhd dosyasını silmeden önce diski veya görüntüyü silerek kirayı kaldırmanız gerekir. Sanal makine tarafından işletim sistemi diski olarak kullanılan bir .vhd dosyasını silmek için, sanal makineyi ve ilişkili tüm diskleri silerek sanal makineyi, işletim sistemi diskini ve kaynak .vhd dosyasını tek seferde silebilirsiniz. Ancak, bir veri diskinin kaynağı olan .vhd dosyasının silinmesi, belirli bir sırada birkaç adımın uygulanmasını gerektirir. İlk olarak, diski sanal makineden ayırın, ardından diski ve sonra .vhd dosyasını silin.
> [!WARNING]
> Kaynak .vhd dosyasını depolama alanından silerseniz veya depolama hesabınızı silerseniz, Microsoft bu verileri kurtaramaz.
> 
> Sayfa bloblarını Premium depolama yalnızca VHD'ler olarak kullanılmak üzere tasarlanmıştır. Maliyetini önemli ölçüde daha büyük olabilir gibi diğer veri türleri Premium depolama, sayfa blobları depolamak Microsoft önermez. Blok blobları, bir VHD değil veri depolamak için kullanın.

## <a name="types-of-disks"></a>Disk türleri 

Azure Diskleri %99,999 kullanılabilirlik sunacak şekilde tasarlanmıştır. Azure diskleri tutarlı bir şekilde endüstri lideri ile Kurumsal düzeyde dayanıklılık sıfır % Annualized hata oranı teslim.

Disklerinizi oluştururken depolama için seçebileceğiniz iki performans katmanı mevcuttur: Standart Depolama ve Premium Depolama. Ayrıca, yönetilmeyen ve yönetilen olmak üzere iki tür disk mevcuttur ve bunlar herhangi bir performans katmanın bulunabilir.


### <a name="standard-storage"></a>Standart depolama 

Standart Depolama, HDD’ler ile desteklenir ve yüksek performans sunarken uygun maliyetli depolama sağlar. Standart depolama bir veri merkezine yerel olarak çoğaltılabilir veya birincil ve ikincil veri merkezleri ile coğrafi yedekleri olabilir. Depolama çoğaltma hakkında daha fazla bilgi için bkz. [Azure Depolama çoğaltma](../articles/storage/common/storage-redundancy.md). 

VM diskleri ile Standart Depolama kullanma hakkında daha fazla bilgi için lütfen bkz. [Standart Depolama ve Diskler](../articles/virtual-machines/windows/standard-storage.md).

### <a name="premium-storage"></a>Premium depolama 

Premium Depolama, SSD’ler ile desteklenir ve G/Ç yoğunluklu iş yükleri için yüksek performanslı, düşük gecikme süresine sahip disk desteği sunar. Genellikle seri adında bir "s" boyutlarıyla Premium depolama kullanabilirsiniz. Örneğin, Dv3 serisi yoktur ve Dsv3 serisi, Dsv3 serisi Premium depolama ile kullanılabilir.  Daha fazla bilgi için bkz. [Premium Depolama](../articles/virtual-machines/windows/premium-storage.md).

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Yönetilmeyen diskler, VM'ler tarafından kullanılan geleneksel türdeki disklerdir. Bunları kullanarak kendi depolama hesabınızı oluşturabilir ve diski oluştururken bu depolama hesabını belirtebilirsiniz. Depolama hesabının [ölçeklendirme hedeflerini](../articles/storage/common/storage-scalability-targets.md) (örneğin, 20.000 IOPS) aşarak VM’lerin azaltılmasına neden olabileceğinden, aynı depolama hesabına çok fazla disk yerleştirmediğinizden emin olun. Yönetilmeyen disklerde, VM’lerinizden en iyi performansı elde etmek için, bir veya daha fazla depolama hesabının kullanımını nasıl en üst düzeye çıkarabileceğinizi anlamanız gerekir.

### <a name="managed-disks"></a>Yönetilen diskler 

Yönetilen Diskler, depolama hesabı oluşturma/yönetme işlemini arka planda gerçekleştirir ve depolama hesabının ölçeklenebilirlik sınırları hakkında endişe etmeniz gerekmez. Azure’un diski oluşturup yönetebilmesi için disk boyutunu ve performans katmanını (Standart/Premium) belirtmeniz yeterlidir. Disk eklediğinizde veya VM ölçeğini artırıp azalttığınızda bile kullanılan depolama alanı konusunda endişelenmeniz gerekmez. 

Ayrıca, her Azure bölgesinde bir depolama hesabındaki özel görüntülerinizi yönetebilir ve aynı abonelikte yüzlerce VM oluşturmak için kullanabilirsiniz. Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Yönetilen Disklere Genel Bakış](../articles/virtual-machines/windows/managed-disks-overview.md).

Yeni VM’ler için Azure Yönetilen Diskleri kullanmanız ve Yönetilen Disklerde sunulan çok sayıda özellikten yararlanmak için önceki yönetilmeyen diskleri yönetilen disklere dönüştürmeniz önerilir.

### <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, kullanacağınız seçeneğe karar vermenize yardımcı olmak üzere hem yönetilmeyen hem de yönetilen diskler için Premium ve Standart katmanların karşılaştırması yapılmaktadır.

|    | Azure Premium Disk | Azure Standart Disk |
|--- | ------------------ | ------------------- |
| Disk Türü | Katı Hal Sürücüleri (SSD) | Sabit Disk Sürücüleri (HDD)  |
| Genel Bakış  | G/Ç yoğunluklu iş yükleri çalıştıran veya görev açısından kritik üretim ortamı barındıran VM’ler için SSD tabanlı, yüksek performans ve düşük gecikme süresi sunan disk desteği | Geliştirme/Test VM senaryoları için HDD tabanlı, uygun maliyetli disk desteği |
| Senaryo  | Üretim ve performansa duyarlı iş yükleri | Geliştirme/Test, kritik olmayan, <br>Nadir erişim |
| Disk Boyutu | P4: 32 GB (yalnızca yönetilen diskleri)<br>P6: 64 GB (yalnızca yönetilen diskleri)<br>P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB<br>P40: 2048 GB<br>P50: 4095 GB | Yönetilmeyen diskler: 1 GB – 4 TB (4095 GB) <br><br>Yönetilen Diskler:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB <br>S40: 2048 GB<br>S50: 4095 GB| 
| Disk Başına En Fazla Aktarım Hızı | 250 MB/sn | 60 MB/sn | 
| Disk başına en fazla IOPS | 7500 IOPS | 500 IOPS | 

