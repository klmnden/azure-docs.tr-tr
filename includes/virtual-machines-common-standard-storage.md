---
title: include dosyası
description: include dosyası
services: storage
author: yuemlu
ms.service: storage
ms.topic: include
ms.date: 01/08/2019
ms.author: yuemlu
ms.custom: include file
ms.openlocfilehash: ad57d373422e0fc310e51ac31f2a2e76999abf22
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61127166"
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Uygun maliyetli standart depolama ile yönetilmeyen ve yönetilen Azure VM diskleri

Azure standart depolama, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Ayrıca, BLOB'ları, tabloları, kuyrukları ve dosyaları destekler. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. Vm'lerle çalışırken, standart diskler SSD ve HDD daha az kritik iş yükleri ve premium SSD diskleri ve geliştirme/Test senaryoları için Görev açısından kritik üretim uygulamaları için kullanabilirsiniz. Standart depolama, tüm Azure bölgelerinde kullanılabilir. 

Bu makalede, SSD ve HDD standart diskler kullanımı üzerinde odaklanır. Depolama BLOB'ları, tabloları, kuyrukları ve dosyaları ile kullanımı hakkında daha fazla bilgi için bkz. [depolamaya giriş](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Disk türleri

Standart diskler, Azure Vm'leri için oluşturmanın iki yolu vardır:

**Yönetilmeyen diskler**: Bu tür bir disk, VM diskleri için karşılık gelen VHD dosyalarını depolamak için kullanılan depolama hesapları yönettiğiniz özgün yöntemidir. VHD dosyaları, depolama hesaplarındaki sayfa blobları olarak depolanır. Yönetilmeyen diskler, öncelikle DSv2 ve GS serisi gibi Premium depolama kullanan VM'ler dahil olmak üzere, herhangi bir Azure VM boyutu iliştirilebilir. Azure sanal makineler en fazla 256 Tib'a kadar depolama alanı sağlayan birkaç standart diskler, iliştirilmeyi destekler. Önizleme disk boyutları kullanırsanız, sanal makine başına yaklaşık 2 PiB kadar olabilir.

[**Azure yönetilen diskler**](../articles/virtual-machines/windows/managed-disks-overview.md): Bu özellik, VM diskleri için kullanılan depolama hesapları yönetir. Türü (Premium SSD, standart SSD veya HDD standart) ve boyutunu belirtmeniz duyduğunuz disk ve Azure oluşturur ve diski oluşturup yönetebilmesi. Diskler her şeyi Azure gerçekleştirir, sizin için--depolama hesapları için ölçeklenebilirlik sınırları içinde kalmasını sağlamak için birden çok depolama hesabında yerleştirme hakkında endişelenmeniz gerekmez.

Her iki disk türleri kullanılabilir olsa bile, çok sayıda özellikten yararlanmak için yönetilen diskler kullanmanızı öneririz.

Azure standart depolama ile çalışmaya başlamak için ziyaret [ücretsiz olarak kullanmaya başlayın](https://azure.microsoft.com/pricing/free-trial/). 

Yönetilen disklerle bir VM oluşturma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın.

* [Resource Manager ve PowerShell kullanarak VM oluşturma](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Azure CLI kullanarak bir Linux VM’si oluşturma](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standart depolama özellikleri

Standart depolama özelliklerinden bazılarını gösteren bir göz atalım. Daha fazla ayrıntı için lütfen bkz [Azure Storage'a giriş](../articles/storage/common/storage-introduction.md).

**Standart depolama**: Azure standart depolama, Azure diskleri, Azure Blobları, Azure dosyaları, Azure tabloları ve Azure kuyruklarının destekler. İle standart depolama hizmetlerini kullanmak için başlangıç [bir Azure depolama hesabı oluşturma](../articles/storage/common/storage-quickstart-create-account.md).

**Standart SSD disk:** Standart SSD diskleri HDD standart diskleri daha daha güvenilir performans sağlar ve şu anda kullanılabilir. Standart SSD disk bölge kullanılabilirliği hakkında daha fazla bilgi için bkz. [bölge kullanılabilirliği standart SSD disk](../articles/virtual-machines/windows/faq-for-disks.md#standard-ssds-azure-regions).

**Standart HDD diskler:** Standart HDD diskler, Premium depolama sayesinde, DSv2 ve GS serisi gibi kullanılan boyutu serisi VM'ler dahil olmak üzere tüm Azure vm'lere eklenebilir. Standart HDD disk yalnızca bir VM'ye iliştirilebilir. Ancak, bu VM boyutu için tanımlanan en fazla disk sayısı kadar bir VM için bir veya daha fazla bu diskleri ekleyebilirsiniz. Standart depolama ölçeklenebilirlik ve performans hedefleri aşağıdaki bölümde, biz özellikleri daha ayrıntılı açıklanmaktadır.

**Standard sayfa blobu**: Standart sayfa blobları, sanal makineler için kalıcı diskler tutmak için kullanılır ve doğrudan Azure BLOB'ları diğer türleri gibi REST aracılığıyla da erişilebilir. [Sayfa blobları](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) rastgele okuma ve yazma işlemleri için iyileştirilmiş 512 baytlık sayfaları oluşan bir koleksiyondur. 

**Depolama çoğaltması:** Çoğu bölgede yerel olarak çoğaltılmış veya coğrafi olarak çoğaltılmış bir standart depolama hesabındaki verileri birden çok veri merkezleri arasında olabilir. Çoğaltma kullanılabilir dört türleri, yerel olarak yedekli depolama (LRS), bölgesel olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) şunlardır. Standart depolama yönetilen diskleri şu anda yalnızca yerel olarak yedekli depolama (LRS) destekler. Daha fazla bilgi için lütfen bkz [depolama çoğaltma](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve Performans Hedefleri

Bu bölümde, biz standart depolama kullanırken dikkate almanız gereken ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Sınırları hesap – yönetilen diskler için geçerli değildir

| **Kaynak** | **Varsayılan Sınır** |
|--------------|-------------------|
| Depolama hesabı başına  | 500 TB |
| En büyük giriş<sup>1</sup> (ABD bölgeleri) depolama hesabı başına | GRS/ZRS etkinleştirilirse, LRS için 20 GB/sn 10 GB/sn |
| En büyük çıkış<sup>1</sup> (ABD bölgeleri) depolama hesabı başına | RA-GRS/GRS/ZRS etkinleştirilirse, LRS için 30 GB/sn 20 GB/sn |
| En büyük giriş<sup>1</sup> (Avrupa ve Asya bölgelerinin) depolama hesabı başına | GRS/ZRS etkinleştirilirse, LRS için 10 GB/sn 5 GB/sn |
| En büyük çıkış<sup>1</sup> (Avrupa ve Asya bölgelerinin) depolama hesabı başına | RA-GRS/GRS/ZRS etkinleştirilirse, LRS için 15 GB/sn 10 GB/sn |
| Toplam istek (1 KB nesne boyutu varsayımıyla) oranı depolama hesabı başına | Saniye başına varlıklar veya saniye başına ileti kadar 20.000 IOPS |

<sup>1</sup> Giriş bir depolama hesabına gönderilen tüm verileri (istekler) ifade eder. Çıkış, bir depolama hesabından alınan tüm verileri (yanıtlar) ifade eder.

Daha fazla bilgi için [Azure Storage ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md).

Uygulamanızın ihtiyaçlarını tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsanız, birden fazla depolama hesabı kullanıyor ve bu depolama hesabı arasında veri bölümlemek için uygulamanızı oluşturun. Alternatif olarak, Azure yönetilen diskler olabilir ve bölümlendirme ve verilerinizi yerleşimini sizin için Azure yönetir.

### <a name="standard-disks-limits"></a>Standart diskler sınırları

Premium diskler, giriş/çıkış işlemi (IOPS) ve standart disk aktarım hızı (bant) başına sağlanmadı. Standart diskler performansını VM boyutuna göre değişir, diskin eklendiği ve diskin boyutu.

İş yükünüz yüksek performanslı ve düşük gecikme süreli disk desteği gerektiriyorsa, Premium depolama kullanmayı düşünmeniz gerekir. Daha fazla Premium depolama avantajlarını öğrenmek için ziyaret [yüksek performanslı Premium depolama ile Azure VM diskleri](../articles/virtual-machines/windows/premium-storage.md).

## <a name="snapshots-and-copy-blob"></a>Anlık görüntü ve kopya blob'u

Depolama hizmeti için VHD dosyasının bir sayfa blobudur. Sayfa blobları anlık ve bunları farklı bir depolama hesabı gibi başka bir konuma kopyalayın.

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Oluşturabileceğiniz [artımlı anlık](../articles/virtual-machines/windows/incremental-snapshots.md) aynı şekilde yönetilmeyen standart diskler, anlık görüntüler ile standart depolama kullanın. Anlık görüntü oluşturma ve yerel olarak yedekli depolama hesabında, kaynak disk ise, bu anlık görüntülerin bir standart coğrafi olarak yedekli depolama hesabına kopyalamak öneririz. Daha fazla bilgi için [Azure depolama Yedekliliği seçenekleri](../articles/storage/common/storage-redundancy.md).

Bir VM'ye bir disk bağlıysa, belirli API işlemlerini disklerde izin verilmez. Örneğin, gerçekleştiremezsiniz bir [kopya blob'u](/rest/api/storageservices/Copy-Blob) disk sanal Makineye bağlı olduğu sürece, blob üzerinde işlem. Bunun yerine, önce bu blob anlık görüntüsünü kullanarak oluşturmanız [anlık görüntü Blob](/rest/api/storageservices/Snapshot-Blob) REST API yöntemi ve ardından gerçekleştirmek [kopya blob'u](/rest/api/storageservices/Copy-Blob) bağlı disk kopyalamak için anlık görüntü. Alternatif olarak, disk ayırma ve gerekli işlemleri gerçekleştirin.

Coğrafi olarak yedekli, anlık görüntü kopyalarını bulundurmak için anlık görüntüler bir yerel olarak yedekli depolama hesabından bir coğrafi olarak yedekli standart depolama hesabı için AzCopy veya kopya blob'u kullanarak kopyalayabilirsiniz. Daha fazla bilgi için [AzCopy komut satırı yardımcı programı ile veri aktarma](../articles/storage/common/storage-use-azcopy.md) ve [kopya blob'u](/rest/api/storageservices/Copy-Blob).

Standart depolama hesaplarında sayfa blobları karşı REST işlemlerini gerçekleştirme hakkında ayrıntılı bilgi için bkz: [Azure Storage Hizmetleri REST API'sini](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Yönetilen diskler

Yönetilen disk için anlık görüntü, standart yönetilen disk olarak depolanan bir yönetilen diskin salt okunur bir kopyasıdır. Artımlı anlık yönetilen diskler için şu anda desteklenmemektedir, ancak gelecekte desteklenecektir.

Yönetilen disk sanal Makineye bağlıysa, belirli API işlemlerini disklerde izin verilmez. Örneğin, bir paylaşılan erişim imzası (SAS) disk sanal Makineye bağlı durumdayken, bir kopyalama işlemini gerçekleştirmek için oluşturulamıyor. Bunun yerine, ilk diskin anlık görüntüsünü oluşturun ve ardından anlık görüntü kopyası gerçekleştirin. Alternatif olarak, disk ayırma ve ardından bir paylaşılan erişim imzası (SAS) kopyalama işlemini gerçekleştirmek için oluşturun.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Standart depolama kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* Standart depolama yönetilmeyen diskler/veri boyutu
* Standart yönetilen diskler
* Standart depolama anlık görüntüleri
* Giden veri aktarımları
* İşlemler

**Yönetilmeyen depolama diski ve veri boyutu:** Yönetilmeyen diskler ve diğer veri (BLOB'lar, tablolar, kuyruklar ve dosyalar), kullanmakta olduğunuz alanı miktarı ücretlendirilir. Bir VM'niz varsa örneğin, sayfa blobu 127 GB, ancak VM sağlandıktan gerçekten olan 10 GB alanı kullanarak yalnızca alanı 10 GB için faturalandırılırsınız. Standart depolama kadar destekliyoruz 8191 GB ve standart yönetilmeyen diskler 4095 GB'a kadar. 

**Yönetilen diskler:** Standart yönetilen diskler için faturalandırma, sağlanan disk boyutuna bağlıdır. Azure aşağıdaki tablolarda belirtildiği gibi en yakın yönetilen diskler seçeneğini (yuvarlanır) sağlanan boyut eşlenir. Her yönetilen disk desteklenen sağlanan boyutları birine eşlenir ve buna göre faturalandırılır. Örneğin, standart yönetilen disk oluşturma ve 200 GiB sağlanan bir boyutunu belirtin, S15 Disk türünü fiyatlandırmasına göre faturalandırılır.

Bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır.

| **Yönetilen standart HDD <br>Disk türü** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60*** | **S70*** | **S80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Disk Boyutu        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1,024 GiB (1 TiB) | 2,048 GiB (2 TiB) | 4,095 GiB (4 TiB) | 8,192 GiB (8 TiB) | 16,385 giB (16 tib'a kadar) | 32.767 giB (32 tib'a kadar) |


**Anlık görüntüleri**: Standart diskler, anlık görüntüler, anlık görüntüler görüntülenerek kullanılan ek kapasite için faturalandırılırsınız. Anlık görüntüleri hakkında daha fazla bilgi için bkz: [bir blobun anlık görüntüsünü oluşturma](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Giden veri aktarımları**: [giden veri aktarımları](https://azure.microsoft.com/pricing/details/data-transfers/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için fatura doğurur.

**İşlem**: Azure 0.0036 100.000 işlem için standart depolama ücretleri. İşlemlere Depolama'da yapılan hem okuma hem de yazma işlemleri dahildir.

Standart depolama, sanal makineler ve yönetilen diskler için fiyatlandırma hakkında ayrıntılı bilgi için bkz:

* [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
* [Sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Azure Backup hizmeti desteği

Yönetilmeyen disklere sahip sanal makineleri Azure Backup kullanılarak yedeklenebilir. [Daha fazla ayrıntı](../articles/backup/backup-azure-vms-first-look-arm.md).

Bir yedekleme işi zaman tabanlı yedeklemeler, kolay VM geri yükleme ve yedek bekletme ilkeleri oluşturmak için yönetilen diskler ile Azure Backup hizmeti de kullanabilirsiniz. Daha fazla bilgi şu anda hakkında [yönetilen disklere sahip VM'ler için Azure Backup'ı kullanarak hizmeti](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama’ya giriş](../articles/storage/common/storage-introduction.md)

* [Depolama hesabı oluşturma](../articles/storage/common/storage-quickstart-create-account.md)

* [Yönetilen Disklere Genel Bakış](../articles/virtual-machines/linux/managed-disks-overview.md)

* [Resource Manager ve PowerShell kullanarak VM oluşturma](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Azure CLI kullanarak bir Linux VM’si oluşturma](../articles/virtual-machines/linux/quick-create-cli.md)
