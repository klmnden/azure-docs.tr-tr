---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/11/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 9d96bd76a4d284e9b4390c564446e8b27c43d591
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613675"
---
## <a name="benefits-of-managed-disks"></a>Yönetilen disklerin avantajlarından

Şimdi, yönetilen diskleri kullanarak elde avantajlarından bazıları üzerinden geçelim.

### <a name="highly-durable-and-available"></a>Yüksek oranda dayanıklı ve kullanılabilir

Yönetilen diskleri % 99,999 kullanılabilirlik için tasarlanmıştır. Yönetilen diskler için yüksek bir dayanıklılık düzeyi izin verme, verilerinizin üç kopyasını sağlayarak elde edin. Çoğaltmaların birinde hatta ikisinde sorunlarla karşılaşılırsa, kalan çoğaltmalar verilerinizin kalıcı olmasını ve başarısızlıklara karşı yüksek tolerans gösterilmesini sağlar. Bu mimari Azure Kurumsal düzeyde tutarlı bir şekilde teslim olmuştur dayanıklılık düzeyi sunmak için altyapı (ıaas) olarak diskleri, endüstri lideri ile sıfır % değer yıllık hata oranı.

### <a name="simple-and-scalable-vm-deployment"></a>Basit ve ölçeklenebilir VM dağıtımı

Yönetilen diskleri kullanarak, en fazla 50.000 VM oluşturabilirsiniz **diskleri** bir abonelikte bölge başına bir tür binlerce oluşturmanızı sağlayan **Vm'leri** tek bir abonelik. Ayrıca bu özellik ölçeklenebilirliği artırır [sanal makine ölçek kümeleri](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Market görüntüsü kullanarak bir sanal makine ölçek kümesinde 1000 adede kadar VM oluşturmak sağlayarak.

### <a name="integration-with-availability-sets"></a>Kullanılabilirlik kümeleri ile tümleştirme

Yönetilen diskler, diskler sağlamak için kullanılabilirlik kümeleri ile tümleşik [bir kullanılabilirlik kümesindeki VM'ler](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) yeterince bir tek hata noktasını önlemek için birbirinden yalıtılmıştır. Diskleri otomatik olarak (damgaları ') farklı depolama ölçek birimlerine yerleştirilir. Bir damga donanım veya yazılım arızasından dolayı başarısız olursa, yalnızca bu Damgalar üzerinde diskleri olan VM örnekleri başarısız. Örneğin, beş Vm'lerde çalışan bir uygulamaya sahip ve bir kullanılabilirlik kümesi'nde vm'leridir varsayalım. Bu sanal makineler tümü aynı damgaya kaydedilmez için uygulamanın diğer örneklerini aşağı bir damga aşması durumunda diskleri şekilde çalışmaya devam eder.

### <a name="integration-with-availability-zones"></a>Kullanılabilirlik alanları ile tümleştirme

Yönetilen diskleri destekler [kullanılabilirlik](../articles/availability-zones/az-overview.md), uygulamalarınızın veri merkezi arızasına karşı koruyan bir yüksek kullanılabilirlik sunan olduğu. Kullanılabilirlik, bir Azure bölgesi içinde benzersiz fiziksel konumlara bölgeleridir. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur. Kullanılabilirlik alanları ile Azure, sektördeki en iyi % 99,99 VM çalışma SLA'sı sunar.

### <a name="azure-backup-support"></a>Azure yedekleme desteği

Bölgesel felaketlere karşı koruma sağlamak [Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md) zaman tabanlı yedeklemeler ve yedekleme saklama ilkeleri ile bir yedekleme işi oluşturmak için kullanılabilir. Bu, en kolay VM geri yüklemeler gerçekleştirmeyi sağlar. Şu anda Azure Backup, dört tebibyte (TiB) diskleri kadar disk boyutları desteklemektedir. Daha fazla bilgi için [yönetilen disklere sahip VM'ler için Azure Backup'ı kullanarak](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

### <a name="granular-access-control"></a>Ayrıntılı erişim denetimi

Kullanabileceğiniz [Azure rol tabanlı erişim denetimi (RBAC)](../articles/role-based-access-control/overview.md) bir veya daha fazla kullanıcı için yönetilen disk için belirli izinler atamak için. Çeşitli işlemler de dahil olmak üzere, bir yönetilen diskler sunmaya okuma, yazma (oluşturma/güncelleştirme), silme ve alınırken bir [paylaşılan erişim imzası (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) disk için. Yalnızca bir kişi, işlerini gerçekleştirmek için gereken işlemlere erişim izni verebilirsiniz. Örneğin, yönetilen disk bir depolama hesabına kopyalamak için bir kişi istemiyorsanız, bu yönetilen disk verme eylemi için erişim izni verilmemesi seçebilirsiniz. Benzer şekilde, bir kişi, yönetilen disk kopyalamak için bir SAS URI kullanmayı istemiyorsanız, bu yönetilen disk izni olmayan seçebilirsiniz.

## <a name="disk-roles"></a>Disk rolleri

Azure'da üç ana disk rolü vardır: veri diski, işletim sistemi diski ve geçici disk. Bu roller, sanal makineye bağlı diskler eşleyin.

![Eylem disk rolleri](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Veri diski

Veri diski uygulama verileri veya tutmak için ihtiyacınız olan diğer verileri depolamak için bir sanal makineye bağlı yönetilen bir disktir. Veri diskleri SCSI sürücüsü olarak kaydedilir ve seçtiğiniz bir harf ile etiketlenir. Her veri diski 32.767 gibibayt (GiB) kapasiteye sahiptir. Bunu ve depolama türünü ekleyebilirsiniz kaç veri diskinin diskleri barındırmak için kullanabileceğiniz sanal makinenin boyutunu belirler.

### <a name="os-disk"></a>İşletim sistemi diski

Her bir sanal makinede bir ekli işletim sistemi diski var. Bu işletim sistemi diski, VM oluşturulduğunda, seçilen önceden yüklenmiş bir OS sahiptir.

Bu diski 2.048 GiB kapasiteye sahiptir.

### <a name="temporary-disk"></a>Geçici disk

Her VM için yönetilen disk değil geçici bir diskle içerir. Geçici disk, uygulamalar ve işlemler için kısa vadeli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için tasarlanmıştır. Geçici diskteki veriler kaybolabilir sırasında bir [bakım olayı](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) olay veya ne zaman, [bir VM'yi yeniden dağıtma](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json). Azure Linux vm'lerinde geçici disk /dev/sdb varsayılan olarak ve Windows Vm'lerinde geçici disk E: varsayılan olarak. Başarılı standart yeniden başlatma VM'nin sırasında geçici diskteki veriler korunur.

## <a name="managed-disk-snapshots"></a>Yönetilen disk anlık görüntüleri

Varsayılan olarak standart yönetilen disk olarak depolanan bir yönetilen diskin salt okunur tam kopya yönetilen disk anlık görüntüsüdür. Anlık görüntüler sayesinde, yönetilen diskleriniz herhangi bir noktada sürede yedekleyebilirsiniz. Bu anlık görüntüler kaynak disk bağımsız olarak mevcut ve yeni yönetilen diskler oluşturmak için kullanılabilir. Kullanılan boyutuna göre faturalandırılır. Örneğin, bu anlık görüntü sağlanan kapasitesi 64 GiB ve gerçek kullanılan veri boyutu 10 GiB ile bir yönetilen diskin anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu 10 GiB için faturalandırılır.  

Yönetilen disklerle anlık görüntüleri oluşturma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Windows anlık görüntüler kullanılarak yönetilen disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta anlık görüntüler kullanılarak yönetilen disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Görüntüler

Yönetilen diskler, yönetilen bir özel görüntü oluşturma da destekler. Bir depolama hesabındaki özel VHD'nizi veya doğrudan bir genelleştirilmiş (Sysprep kullanılarak hazırlanmış) VM görüntü oluşturabilirsiniz. Bu işlem, tek bir görüntüsünü yakalar. Bu görüntü, işletim sistemi ve veri diskleri dahil olmak üzere bir VM ile ilişkili tüm yönetilen diskler içerir. Bu yönetilen bir özel görüntü oluşturma yüzlerce VM kopyalayın veya tüm depolama hesaplarını yönetmenize gerek kalmadan özel görüntü kullanarak sağlar.

Görüntüleri oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü yakalama](../articles/virtual-machines/windows/capture-image-resource.md)
* [Azure CLI'yi kullanarak Linux sanal makinelerini genelleştirme ve yakalama](../articles/virtual-machines/linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Anlık görüntülerle

Görüntüleri ve anlık görüntü arasındaki farkı anlamak önemlidir. Yönetilen diskler ile serbest bıraktı genelleştirilmiş bir sanal makine görüntüsü alabilir. Bu görüntü VM'ye bağlı disklerin tümünü içerir. Bu görüntü bir VM oluşturmak için kullanabileceğiniz ve disklerin tümünü içerir.

Anlık görüntü bir noktada bir disk anlık görüntünün alınma sürede kopyasıdır. Bu, yalnızca bir diske uygulanır. Bir disk (işletim sistemi diski) sahip bir VM'niz varsa, bir anlık görüntü veya bir görüntüsünü alın ve anlık görüntünün veya görüntü bir VM oluşturun.

Anlık görüntü tanıma içerdiği dışındaki herhangi bir disk yok. Bu sorunlu şeritleme gibi birden çok disk eşgüdümünü gerektiren senaryolar kullanmasını sağlar. Birbiriyle koordine edebilmek anlık görüntüleri gerekir ve bu şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Azure'un sunduğu tek tek disk türleri hakkında daha fazla bilgi edinin ve disk türlerinde makalemizde gereksinimlerinize uygun türüdür.
