---
title: include dosyası
description: include dosyası
services: storage
author: yuemlu
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: yuemlu
ms.custom: include file
ms.openlocfilehash: 4e62342a32456787863da775ea98df178ab1d559
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34806307"
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Düşük maliyetli standart depolama ve yönetilen ve yönetilmeyen Azure VM diskleri

Azure Standard Storage, gecikmeye duyarlı olmayan iş yükleri çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği sunar. Ayrıca, BLOB'lar, tablolar, kuyruklar ve dosyaları destekler. Standart depolama ile verileri sabit disk sürücülerinin (HDD'ler) depolanır. VM ile birlikte çalışırken, standart SSD ve HDD diskler geliştirme ve Test senaryoları için ve daha az önemli iş yüklerine ve premium SSD diskleri kritik üretim uygulamaları için kullanabilirsiniz. Standart depolama Azure tüm bölgelerde kullanılabilir. 

Bu makalede standart SSD ve HDD diskler kullanımını odaklanır. BLOB'lar, tablolar, kuyruklar ve dosyalar ile depolama kullanımı hakkında daha fazla bilgi için bkz: [depolama giriş](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Disk türleri

Azure VM'ler için standart diskler oluşturmanın iki yolu vardır:

**Yönetilmeyen diskleri**: Bu tür diski özgün VM diskleri karşılık VHD dosyaları depolamak için kullanılan depolama hesapları yöneteceğiniz yöntemidir. VHD dosyaları, sayfa bloblarını depolama hesaplarındaki olarak depolanır. Yönetilmeyen diskleri öncelikle gibi DSv2 ve GS serisi Premium depolama kullanan sanal makineleri de dahil olmak üzere, herhangi bir Azure VM boyutu bağlı. Azure VM'ler birkaç standart diskler ekleme VM başına en fazla 256 TB izin vererek destekler.

[**Azure yönetilen diskleri**](../articles/virtual-machines/windows/managed-disks-overview.md): Bu özellik VM diskleri için kullandığınız depolama hesapları yönetir. Türü (Premium SSD, standart SSD veya standart HDD) ve boyutunu belirtebilir duyduğunuz disk ve Azure oluşturur ve disk tarafından yönetilir. Diskleri depolama hesapları için ölçeklenebilirlik sınırları içinde kalmasını--Azure işler, sizin yerinize sağlamak için birden çok depolama hesaplarında yerleştirme hakkında endişelenmeniz gerekmez.

Her iki disk türleri için kullanılabilir olsa bile, birçok özelliklerden yararlanmak için yönetilen diskleri kullanmanızı öneririz.

Azure Standard Storage ile çalışmaya başlamak için ziyaret [ücretsiz olarak başlayın](https://azure.microsoft.com/pricing/free-trial/). 

Yönetilen disklerle bir VM oluşturma hakkında daha fazla bilgi için aşağıdaki makaleler birine bakın.

* [Resource Manager ve PowerShell kullanarak VM oluşturma](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Azure CLI 2.0 kullanarak bir Linux VM oluşturma](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standart depolama özellikleri 

Standart depolama özelliklerden bazıları bir göz atalım. Daha fazla ayrıntı için lütfen bkz. [Azure Storage'a giriş](../articles/storage/common/storage-introduction.md).

**Standart depolama**: Azure Standard Storage destekleyen Azure diskleri, Azure BLOB'ları, Azure dosyaları, Azure tabloları ve Azure sıralar. İle standart depolama hizmetleri kullanmaya başlamak [bir Azure depolama hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).

**Standart SSD diskleri:** standart SSD diskleri standart HDD diskler daha daha güvenilir performans sağlar ve şu anda Önizleme'de kullanılabilir. Standart SSD diskleri bölge kullanılabilirliği hakkında daha fazla bilgi için bkz: [standart SSD diskleri (Önizleme) bölge kullanılabilirliği](../articles/virtual-machines/windows/faq-for-disks.md#standard-ssds-azure-regions).

**Standart HDD diskler:** standart HDD diskleri gibi DSv2 ve GS serisi Premium Storage ile kullanılan boyutu-serisi Vm'leri de dahil olmak üzere tüm Azure vm'lerinin eklenebilir. Bir standart HDD disk yalnızca bir VM bağlanabilir. Ancak, bu VM boyutu için tanımlanan en fazla disk sayısı kadar bir VM için bir veya daha fazla bu diskleri ekleyebilirsiniz. Standard Storage ölçeklenebilirlik ve performans hedefleri aşağıdaki bölümde daha ayrıntılı özellikleri açıklanmaktadır.

**Standart sayfa blobu**: Standart sayfa BLOB'ları VM'ler için kalıcı diskleri tutmak için kullanılır ve doğrudan Azure BLOB'ları diğer türleri gibi REST aracılığıyla da erişilebilir. [Sayfa blobları](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) rastgele okuma ve yazma işlemleri için en iyi duruma getirilmiş 512 baytlık sayfaların koleksiyonudur. 

**Depolama çoğaltma:** çoğu bölgede bir standart depolama hesabı verilerde birden çok veri merkezleri arasında yerel olarak çoğaltılmış veya coğrafi olarak çoğaltılmış olabilir. Çoğaltma kullanılabilir dört tür yerel olarak yedekli depolama (LRS), bölge olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) ve okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) var. Standart depolama yönetilen diskleri şu anda yalnızca yerel olarak yedekli depolama (LRS) destekler. Daha fazla bilgi için lütfen bkz [depolama çoğaltma](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Ölçeklenebilirlik ve Performans Hedefleri

Bu bölümde, standart depolama kullanırken göz önünde bulundurmanız gereken ölçeklenebilirlik ve performans hedefleri açıklanmaktadır.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Sınırları hesap – yönetilen diskler için geçerli değil

| **Kaynak** | **Varsayılan Sınır** |
|--------------|-------------------|
| Depolama hesabı başına  | 500 TB |
| En fazla giriş<sup>1</sup> her depolama hesabı (BİZE bölgelerde) | GRS/ZRS etkinleştirilirse, 20 GB/sn LRS için 10 GB/sn |
| En büyük çıkış<sup>1</sup> her depolama hesabı (BİZE bölgelerde) | RA-GRS/GRS/ZRS etkinleştirilirse, LRS için 30 GB/sn 20 GB/sn |
| En fazla giriş<sup>1</sup> her depolama hesabı (Avrupa ve Asya bölgeleri) | GRS/ZRS etkinleştirilirse, 10 GB/sn LRS için 5 GB/sn |
| En büyük çıkış<sup>1</sup> her depolama hesabı (Avrupa ve Asya bölgeleri) | RA-GRS/GRS/ZRS etkinleştirilirse, 15 GB/sn LRS için 10 GB/sn |
| Toplam istek (1 KB nesne boyutu varsayılarak) başına hızını depolama hesabı | Saniye başına varlıklar veya saniye başına ileti 20000 IOPS |

<sup>1</sup> bir depolama hesabına gönderilen tüm veriler (istek sayısı) için giriş başvuruyor. Çıkış, bir depolama hesabından alınan tüm verileri (yanıtları) ifade eder.

Daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../articles/storage/common/storage-scalability-targets.md).

Uygulamanızın gereksinimlerine tek bir depolama hesabı ölçeklenebilirlik hedeflerini aşarsa, birden çok depolama hesabı kullanın ve bu depolama hesaplarında verilerinizi bölümlemek için uygulamanızı oluşturun. Alternatif olarak, Azure yönetilen diskleri olabilir ve bölümlendirme ve verilerinizi yerleştirme sizin için Azure yönetir.

### <a name="standard-disks-limits"></a>Standart diskler sınırları

Premium diskler farklı olarak, girdi/çıktı işlemleri / saniye (IOPS) ve standart diskler (bant) verimini sağlanmayan. Standart diskler performansını VM boyutu disk ekli olduğu için değil, diskin boyutuna göre değişir. Aşağıdaki tabloda listelenen performans sınırına kadar elde etmek beklediğiniz.

**Standart diskler sınırları (yönetilen ve yönetilmeyen)**

| **VM katmanı**            | **Temel katman VM** | **Standart katmanı VM** |
|------------------------|-------------------|----------------------|
| En büyük Disk boyutu          | 4095 GB           | 4095 GB              |
| Disk başına maksimum 8 KB IOPS | En fazla 300         | En fazla 500            |
| Disk başına en fazla bant genişliği | En fazla 60 MB/sn     | En fazla 60 MB/sn        |

Yüksek performanslı, düşük gecikmeli disk desteği İş yükünüzün gerektirdiği Premium Storage kullanmayı düşünmelisiniz. Premium Storage daha fazla avantajları bilmeniz ziyaret [yüksek performanslı Premium depolama ve Azure VM diskleri](../articles/virtual-machines/windows/premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Anlık görüntüler ve kopyalama blob

Depolama hizmeti için bir sayfa blob'u VHD dosyasıdır. Sayfa bloblarını anlık görüntülerini almak ve bunları farklı bir depolama hesabı gibi başka bir konuma kopyalayın.

### <a name="unmanaged-disks"></a>Yönetilmeyen diskler

Oluşturabileceğiniz [artımlı anlık görüntüleri](../articles/virtual-machines/windows/incremental-snapshots.md) için aynı şekilde yönetilmeyen standart diskler, anlık görüntüler ile standart depolama kullanın. Anlık görüntü oluşturma ve yerel olarak yedekli depolama hesabı, kaynak disk ise, bu anlık görüntülerin bir standart coğrafi olarak yedekli depolama hesabına kopyalamak öneririz. Daha fazla bilgi için bkz: [Azure depolama artıklığı seçeneği](../articles/storage/common/storage-redundancy.md).

Bir VM'ye bir disk bağlıysa, bazı API işlemleri disklerde izin verilmez. Örneğin, gerçekleştiremezsiniz bir [kopyalama Blob](/rest/api/storageservices/Copy-Blob) disk bir VM öğesine bağlı olduğu sürece bu blob işlemi. Bunun yerine, önce bir anlık görüntüsünü o blob kullanarak oluşturmak [anlık görüntü Blob](/rest/api/storageservices/Snapshot-Blob) REST API yöntemi ve gerçekleştirin [kopyalama Blob](/rest/api/storageservices/Copy-Blob) bağlı diske kopyalamak için anlık görüntü. Alternatif olarak, disk ayırma ve gerekli işlemleri gerçekleştirin.

Coğrafi olarak yedekli, anlık görüntü kopyalarını bulundurmak için anlık görüntüler bir yerel olarak yedekli depolama hesabından bir standart coğrafi olarak yedekli depolama hesabına AzCopy veya kopya Blob kullanarak kopyalayabilirsiniz. Daha fazla bilgi için bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](../articles/storage/common/storage-use-azcopy.md) ve [kopyalama Blob](/rest/api/storageservices/Copy-Blob).

Standart depolama hesapları sayfa bloblarını karşı REST işlemlerini gerçekleştirme hakkında ayrıntılı bilgi için bkz: [Azure Storage Hizmetleri REST API'sini](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Yönetilen diskler

Yönetilen bir disk için bir anlık görüntü, standart yönetilen disk olarak depolanan yönetilen diskin salt okunur bir kopyasıdır. Artımlı anlık görüntüler şu anda yönetilen disklerde desteklenmez, ancak gelecekte desteklenecektir.

Yönetilen bir disk için bir VM bağlıysa, bazı API işlemleri disklerde izin verilmez. Örneğin, bir paylaşılan erişim imzası (SAS) disk için bir VM eklenirken bir kopyalama işlemi gerçekleştirmek için oluşturulamıyor. Bunun yerine, önce diskin anlık görüntüsünü oluşturabilir ve sonra anlık görüntü kopyası gerçekleştirin. Alternatif olarak, disk ayırma ve paylaşılan erişim imzası (SAS) kopyalama işlemi gerçekleştirmek için oluşturur.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Standart depolama kullanırken, aşağıdaki fatura değerlendirmeleri geçerlidir:

* Yönetilmeyen standart depolama diskleri/veri boyutu 
* Standart yönetilen diskler
* Standart depolama anlık görüntüleri
* Giden veri aktarımları
* İşlemler

**Yönetilmeyen depolama diski ve veri boyutu:** yönetilmeyen diskleri ve diğer verileri (BLOB'lar, tablolar, kuyruklar ve dosyaları) için kullandığınız alan miktarı için ücretlendirilirsiniz. Bir VM'ye sahip olması gibi sayfa blobu 127 GB, ancak VM sağlandığında gerçekten ise 10 GB alanı kullanarak yalnızca için 10 GB alanı faturalandırılır. Standart depolama kadar destekliyoruz 8191 GB ve yönetilmeyen standart diskler 4095 GB'a kadar. 

**Yönetilen diskleri:** yönetilen diskleri sağlanan boyutuna faturalandırılır. Diskinizin 10 GB disk olarak sağlanır ve yalnızca 5 GB kullanıyorsanız, 10 GB sağlama boyutu için sizden ücret kesilir.

**Anlık Görüntü**: standart disklerin anlık görüntüleri, anlık görüntü tarafından kullanılan ek kapasite için faturalandırılır. Anlık görüntüler hakkında daha fazla bilgi için bkz: [Blob anlık görüntüsünün oluşturulmasına](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Giden veri aktarımları**: [giden veri aktarımları](https://azure.microsoft.com/pricing/details/data-transfers/) (Azure veri merkezlerinde dışında giderek veri) bant genişliği kullanımı için fatura doğurur.

**İşlem**: Azure 0.0036 100.000 işlemleri için standart depolama başına ücretlendirilen. İşlemlere Depolama'da yapılan hem okuma hem de yazma işlemleri dahildir.

Standart depolama, sanal makineler ve yönetilen diskleri için fiyatlandırma hakkında ayrıntılı bilgi için bkz:

* [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
* [Sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Fiyatlandırma yönetilen diskleri](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Azure yedekleme hizmeti desteği 

Sanal makineler yönetilmeyen disklerle Azure Yedekleme kullanılarak yedeklenebilir. [Daha fazla ayrıntı](../articles/backup/backup-azure-vms-first-look-arm.md).

Bir yedekleme işi zaman tabanlı yedeklemeler, kolay VM geri yükleme ve yedekleme bekletme ilkeleri oluşturmak için yönetilen disklerle Azure Backup hizmeti de kullanabilirsiniz. Daha fazla bilgiyi bu hakkında [kullanarak Azure Backup hizmeti yönetilen diskleri olan VM'ler için](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama’ya giriş](../articles/storage/common/storage-introduction.md)

* [Depolama hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md)

* [Yönetilen Disklere Genel Bakış](../articles/virtual-machines/linux/managed-disks-overview.md)

* [Resource Manager ve PowerShell kullanarak VM oluşturma](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Azure CLI 2.0 kullanarak bir Linux VM oluşturma](../articles/virtual-machines/linux/quick-create-cli.md)
