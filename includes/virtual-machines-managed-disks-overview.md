---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 26268c892b0e900c410cd669454b8b6f02ee8886
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102598"
---
# <a name="azure-managed-disks-overview"></a>Azure yönetilen disklere genel bakış

Azure yönetilen diskler yöneterek Azure Iaas Vm'leri için disk yönetimini basitleştirir [depolama hesapları](../articles/storage/common/storage-introduction.md) VM diskleriyle ilişkili. Yalnızca türünü belirtmeniz gerekir ([standart HDD](../articles/virtual-machines/windows/standard-storage.md), [standart SSD](../articles/virtual-machines/windows/disks-standard-ssd.md), veya [Premium SSD](../articles/virtual-machines/windows/premium-storage.md)) ihtiyacınız disk boyutunu ve Azure oluşturur ve diski oluşturup yönetebilmesi.

## <a name="benefits-of-managed-disks"></a>Yönetilen disklerin avantajlarından

Bazı avantajları elde yönetilen diskleri kullanarak bu kanal 9 videosu ile başlayan bir göz atalım [yönetilen diskler ile daha iyi Azure VM dayanıklılığını](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Basit ve ölçeklenebilir VM dağıtımı

Diskleri tanıtıcılarını depolama arka planda sizin için yönetilen. Daha önce Azure sanal makineleriniz için diskleri (VHD dosyaları) tutmak için depolama hesapları oluşturmanız gerekirdi. Büyütme, ek depolama hesapları herhangi disklerinizi depolama IOPS sınırını aşan yaramadı için oluşturduğunuz emin olması gerekiyordu. Yönetilen depolama işleme diskler ile depolama hesabı sınırları tarafından artık sınırlıdır (örneğin, 20.000 IOPS / hesabı). Ayrıca artık kendi özel görüntülerinizi (VHD dosyaları) için birden çok depolama hesabında kopyalama gerekmez. Bunları merkezi bir konumda – Azure bölgesi başına bir depolama hesabına – yönetebilir ve bunları bir abonelikte yüzlerce VM oluşturmak için kullanın.

Yönetilen diskler, en fazla 50.000 VM oluşturmanızı sağlayacaktır **diskleri** bir abonelikte bölge başına bir tür hangi etkinleştirir, binlerce oluşturmanızı **Vm'leri** tek bir abonelik. Ayrıca bu özellik ölçeklenebilirliği artırır [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) , Market görüntüsü kullanarak bir sanal makine ölçek kümesinde en fazla binlerce VM'ler oluşturmanızı sağlayarak.

### <a name="better-reliability-for-availability-sets"></a>Daha fazla güvenilirlik için kullanılabilirlik kümeleri

Yönetilen diskler sağlayan daha fazla güvenilirlik için kullanılabilirlik kümelerini, disklerin sağlayarak [bir kullanılabilirlik kümesindeki Vm'leri](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) yeterince tek hata noktalarından kaçınmak için birbirinden yalıtılmıştır. Diskleri otomatik olarak (damgaları ') farklı depolama ölçek birimlerine yerleştirilir. Bir damga donanım veya yazılım arızasından dolayı başarısız olursa, yalnızca bu Damgalar üzerinde diskleri olan VM örnekleri başarısız. Örneğin, beş Vm'lerde çalışan bir uygulamaya sahip ve bir kullanılabilirlik kümesi'nde vm'leridir varsayalım. Bu sanal makineler tümü aynı damgaya kaydedilmez için uygulamanın diğer örneklerini aşağı bir damga aşması durumunda diskleri şekilde çalışmaya devam eder.

### <a name="highly-durable-and-available"></a>Yüksek oranda dayanıklı ve kullanılabilir

Azure Diskleri %99,999 kullanılabilirlik sunacak şekilde tasarlanmıştır. Daha kolay, yüksek düzeyde dayanıklılık sayesinde verilerinizin üç kopyasını olduğunu bilmenin rahatlığını yaşayın. Çoğaltmaların birinde hatta ikisinde sorunlarla karşılaşılırsa, kalan çoğaltmalar verilerinizin kalıcı olmasını ve başarısızlıklara karşı yüksek tolerans gösterilmesini sağlar. Azure bu mimari sayesinde IaaS diskleri için tutarlı bir şekilde kurumsal düzeyde dayanıklılık sunarak sektördeki en başarılı %SIFIR Yıllık Hata Oranını elde etmektedir.

### <a name="granular-access-control"></a>Ayrıntılı erişim denetimi

Kullanabileceğiniz [Azure rol tabanlı Access Control (RBAC)](../articles/role-based-access-control/overview.md) bir veya daha fazla kullanıcı için yönetilen disk için belirli izinler atamak için. Yönetilen diskler kullanıma sunan okuma işlemleri dahil olmak üzere çeşitli, yazma (oluşturma/güncelleştirme), silme ve alınırken bir [paylaşılan erişim imzası (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) disk için. Yalnızca bir kişi, işlerini gerçekleştirmek için gereken işlemlere erişim izni verebilirsiniz. Örneğin, yönetilen disk bir depolama hesabına kopyalamak için bir kişi istemiyorsanız, bu yönetilen disk verme eylemi için erişim izni verilmemesi seçebilirsiniz. Benzer şekilde, bir kişi, yönetilen disk kopyalamak için bir SAS URI kullanmayı istemiyorsanız, bu yönetilen disk izni olmayan seçebilirsiniz.

### <a name="azure-backup-service-support"></a>Azure Backup hizmeti desteği

Azure Backup hizmeti, bir yedekleme işi zaman tabanlı yedeklemeler, kolay VM geri yükleme ve yedek bekletme ilkeleri oluşturmak için yönetilen diskler ile kullanın. Yönetilen diskler, çoğaltma seçeneği olarak yalnızca yerel olarak yedekli depolama (LRS) destekler. Tek bir bölgede üç veri kopyası tutulur. Bölgesel bir olağanüstü durum kurtarma için farklı bir bölgeye kullanarak bir sanal makine disklerinizi yedeklemelisiniz [Azure Backup hizmeti](../articles/backup/backup-introduction-to-azure-backup.md) ve yedekleme kasasıyla GRS depolama hesabı. Şu anda Azure Backup, disk boyutu 4 TB'tan büyük disklere kadar destekler, bkz: [anında geri yükleme](../articles/backup/backup-instant-restore-capability.md) 4 TB disk desteği. Daha fazla bilgi için [yönetilen disklere sahip VM'ler için Azure Backup'ı kullanarak hizmeti](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Yönetilen diskleri kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* Depolama türü

* Disk Boyutu

* İşlem sayısı

* Giden veri aktarımları

* Yönetilen disk anlık görüntülerini (tam disk kopyalama)

Bu seçenekler daha yakın bir göz atalım.

**Depolama türü:** Yönetilen diskler teklifler 3 performans katmanları [Standart HDD](../articles/virtual-machines/windows/standard-storage.md), [standart SSD](../articles/virtual-machines/windows/disks-standard-ssd.md), ve [Premium](../articles/virtual-machines/windows/premium-storage.md). Faturalama yönetilen disk depolama türü için disk üzerinde seçmiş olduğunuz bağlıdır.

**Disk boyutu**: Yönetilen diskler için faturalandırma, sağlanan disk boyutuna bağlıdır. Azure aşağıdaki tablolarda belirtildiği gibi en yakın yönetilen diskler seçeneğini (yuvarlanır) sağlanan boyut eşlenir. Her yönetilen disk desteklenen sağlanan boyutları birine eşlenir ve buna göre faturalandırılır. Örneğin, standart yönetilen disk oluşturma ve 200 GB sağlanan boyutlu belirtin, S15 Disk türünü fiyatlandırmasına göre faturalandırılır.

Premium yönetilen disk için kullanılabilir disk boyutları İşte, yıldız işareti ile gösterilen boyutları şu anda Önizleme aşamasındadır:

| **Yönetilen premium SSD <br>Disk türü** | **P4** | **P6** | **P10** | **P15** | **P20** | **P30** | **P40** | **P50** | **P60*** | **P70*** | **P80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Disk Boyutu        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1,024 GiB (1 TiB) | 2,048 GiB (2 TiB) | 4,095 GiB (4 TiB) | 8,192 GiB (8 TiB) | 16,384 giB (16 tib'a kadar) | 32.767 giB (TiB) |

İşte yönetilen disk için bir standart SSD disk boyutu, bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır:

| **Standart SSD yönetilen <br>Disk türü** | **E4** | **E6** | **E10** | **E15** | **E20** | **E30** | **E40** | **E50** | **E60*** | **E70*** | **E80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Disk Boyutu        | 32 GiB | 64 GiB | 128 GiB | 256 GiB | 512 GiB | 1,024 GiB (1 TiB) | 2,048 GiB (2 TiB) | 4,095 GiB (4 TiB) | 8,192 GiB (8 TiB) | 16,384 giB (16 tib'a kadar) | 32.767 giB (TiB) |

İşte yönetilen disk için standart bir HDD kullanılabilir disk boyutu, bir yıldız işaretiyle gösterilen boyutları şu anda Önizleme aşamasındadır:

| **Yönetilen standart HDD <br>Disk türü** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60*** | **S70*** | **S80*** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Disk Boyutu        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1,024 GiB (1 TiB) | 2,048 GiB (2 TiB) | 4,095 GiB (4 TiB) | 8,192 GiB (8 TiB) | 16,384 giB (16 tib'a kadar) | 32.767 giB (TiB) |

**İşlem sayısı**: Standart yönetilen disk üzerinde gerçekleştirdiğiniz işlem sayısı için faturalandırılırsınız.

Standart SSD Disk GÇ birim boyutu 256 KB'ı kullanın. Aktarılan veriler 256 KB'den küçük ise, 1 g/ç birim olarak kabul edilir. Büyük g/ç boyutları sayılır olarak birden çok g/ç boyutu 256 KB. Örneğin, bir 1.100 KB g/ç beş g/ç birimleri sayılır.

Premium yönetilen disk işlemleri herhangi bir maliyet yoktur.

**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/data-transfers/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için fatura doğurur.

Yönetilen diskler için fiyatlandırma hakkında ayrıntılı bilgi için bkz: [yönetilen diskler fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Yönetilen Disk anlık görüntüleri

Yönetilen anlık görüntü, salt okunur bir tam kopya varsayılan olarak standart yönetilen disk olarak depolanan bir yönetilen diskin ' dir. Anlık görüntüler sayesinde, yönetilen diskleriniz herhangi bir noktada sürede yedekleyebilirsiniz. Bu anlık görüntüler kaynak disk bağımsız olarak mevcut ve yeni yönetilen diskler oluşturmak için kullanılabilir. Kullanılan boyutuna göre faturalandırılır. Örneğin, anlık görüntü sağlanan kapasitesi 64 GiB ve gerçek kullanılan veri boyutu 10 GiB ile bir yönetilen diskin anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu 10 GiB için faturalandırılırsınız.  

[Artımlı anlık](../articles/virtual-machines/windows/incremental-snapshots.md) yönetilen diskler için şu anda desteklenmemektedir.

Yönetilen diskler ile anlık görüntüleri oluşturma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Windows’da Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

## <a name="images"></a>Görüntüler

Yönetilen diskler, yönetilen bir özel görüntü oluşturma da destekler. Bir depolama hesabındaki özel VHD'nizi veya doğrudan bir genelleştirilmiş VM'nin (sys prepped) görüntü oluşturabilirsiniz. Bu işlem, tüm yönetilen diskler işletim sistemi ve veri diskleri dahil olmak üzere bir VM ile ilişkili tek bir görüntüde yakalar. Bu yönetilen bir özel görüntü oluşturma yüzlerce VM kopyalayın veya tüm depolama hesaplarını yönetmenize gerek kalmadan özel görüntü kullanarak sağlar.

Görüntüleri oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü yakalama](../articles/virtual-machines/windows/capture-image-resource.md)
* [Genelleştirmek ve Azure CLI kullanarak bir Linux sanal makinesini yakalama](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Anlık görüntülerle

"İmage Vm'leri ile kullanılan" sözcüğü sık bakın ve "anlık" de göreceksiniz. Bu terimler arasındaki farkı anlamak önemlidir. Yönetilen diskler ile serbest bıraktı genelleştirilmiş bir sanal makine görüntüsü alabilir. Bu görüntü VM'ye bağlı disklerin tümünü içerir. Tüm diskler dahil edilir ve bu görüntü, yeni bir VM oluşturmak için kullanabilirsiniz.

Anlık görüntü bir noktada bir disk, geçen sürede kopyasıdır. Yalnızca tek bir disk için de geçerlidir. Yalnızca tek bir disk (işletim sistemi) sahip bir VM'niz varsa, bir anlık görüntü veya bir görüntüsünü alın ve anlık görüntünün veya görüntü bir VM oluşturun.

Ne beş disk bir sanal makine içeriyor ve bunlar şeritli? Her disk anlık görüntüsünü alabilir, ancak hiçbir farkındalık VM diskleri – durumunun anlık görüntüler yalnızca tek bir disk hakkında bilmeniz var. Bu durumda, anlık görüntüleri birbiriyle koordine olmanız gerekir ve şu anda desteklenmiyor.

## <a name="managed-disks-and-encryption"></a>Yönetilen diskler ve şifreleme

Yönetilen diskler yalnızca tartışmak için şifreleme iki tür vardır. İlk Depolama hizmeti tarafından gerçekleştirilen depolama hizmeti Şifrelemesi'ne (SSE) olur. İkincisi işletim sistemi ve veri diskleri üzerinde sanal makineleriniz için etkinleştirebilirsiniz Azure Disk Şifrelemesi ' dir.

### <a name="storage-service-encryption-sse"></a>Depolama hizmeti şifrelemesi (SSE)

[Azure depolama hizmeti şifrelemesi](../articles/storage/common/storage-service-encryption.md) bekleyen şifreleme sağlar ve organizasyonel güvenlik ve uyumluluk yükümlülüklerinizin yerine için verilerinizi koruyun. SSE, tüm yönetilen diskler, anlık görüntüler ve yönetilen diskleri kullanılabilir olduğu tüm bölgelerde görüntüleri için varsayılan olarak etkindir. 10 Haziran 2017'den itibaren tüm yeni disk/anlık görüntü/görüntüler ve mevcut yönetilen disklere yazılan yeni veriler varsayılan olarak Microsoft tarafından yönetilen anahtarlarla otomatik olarak şifrelenmiş bekleyen yönetildiği. Ziyaret [yönetilen diskler ile ilgili SSS sayfasını](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) daha fazla ayrıntı için.

### <a name="azure-disk-encryption-ade"></a>Azure Disk şifrelemesi (ADE)

Azure Disk şifrelemesi bir Iaas sanal makine tarafından kullanılan işletim sistemi ve veri diskleri şifreleme sağlar. Bu şifreleme, yönetilen diskler içerir. Windows için sürücüleri, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için DM-Crypt teknolojisini kullanarak diskleri şifrelenir. Şifreleme işlemi, disk şifreleme anahtarlarını yönetmek ve denetlemek izin vermek için Azure anahtar kasası ile tümleştirilmiştir. Daha fazla bilgi için [için Azure Disk şifrelemesi Windows ve Linux Iaas sanal makineleri](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen diskler hakkında daha fazla bilgi için lütfen aşağıdaki makalelere bakın.

### <a name="get-started-with-managed-disks"></a>Yönetilen Diskler’i kullanmaya başlama

* [Resource Manager ve PowerShell kullanarak VM oluşturma](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Azure CLI kullanarak bir Linux VM’si oluşturma](../articles/virtual-machines/linux/quick-create-cli.md)

* [PowerShell kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Linux VM’ye yönetilen disk ekleme](../articles/virtual-machines/linux/add-disk.md)

* [Yönetilen diskler PowerShell örnek betikler](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Azure Resource Manager şablonlarında yönetilen diskleri kullanma](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Yönetilen diskler depolama seçeneklerini karşılaştırma

* [Premium SSD diskleri](../articles/virtual-machines/windows/premium-storage.md)

* [Standart SSD ve HDD diskleri](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>İşlemsel kılavuz

* [AWS ve diğer platformlardan azure'da yönetilen disklere geçirme](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Azure Vm'leri azure'da yönetilen disklere dönüştürme](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
