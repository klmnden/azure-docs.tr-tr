---
title: (Sayfa blobları) yönetilmeyen ve yönetilen diskler depolaması için Microsoft Azure Windows Vm'lerine hakkında | Microsoft Docs
description: İle ilgili temel bilgileri öğrenin (sayfa blobları) yönetilmeyen ve yönetilen diskler depolaması için azure'da Windows sanal makineleri.
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0caa88939e263aa5d18460144893cdbce72f10ec
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961542"
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Azure Windows Vm'leri için diskleri depolama hakkında

Yalnızca diğer bilgisayarlar gibi azure'da sanal makineler bir işletim sistemini, uygulamalarını ve verilerini depolamak için bir farkı şudur diskleri kullanın. Tüm Azure sanal makineler, en az iki diskin – bir Windows işletim sistemi diski ve geçici bir diskle sahiptir. İşletim sistemi diski bir görüntüden oluşturulur ve hem işletim sistemi diski ile görüntü sanal sabit bir Azure depolama hesabında depolanan diskleri (VHD). Sanal makineler, VHD'ler olarak da depolanan bir veya daha fazla veri diski olarak da olabilir. 

Bu makalede, biz diskleri farklı kullanımları hakkında konuşacak ve ardından farklı tür diskler oluşturma ve kullanma tartışın. Bu makale için de kullanılabilir olan [Linux sanal makineleri](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Sanal makineleri tarafından kullanılan diskler

Diskler, VM'ler tarafından nasıl kullanıldığı bir bakalım.

### <a name="operating-system-disk"></a>İşletim sistemi diski

Her bir sanal makinede bir ekli işletim sistemi diski var. Bir SATA sürücüsünü kayıtlı ve C: sürücüsünde varsayılan olarak etiketlenmiş. Bu disk 2048 gigabayt (GB) maksimum kapasiteye sahiptir.

### <a name="temporary-disk"></a>Geçici disk

Her sanal makine geçici bir diskle içerir. Geçici disk, uygulamalar ve işlemler için kısa vadeli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için tasarlanmıştır. Geçici diskteki veriler kaybolabilir sırasında bir [bakım olayı](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) veya ne zaman, [bir VM'yi yeniden dağıtma](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Başarılı standart yeniden başlatma VM'nin sırasında geçici sürücüdeki verilerin kalıcı olması. Bununla birlikte, burada verileri, yeni bir ana bilgisayara taşımayı gibi korunmayabilir durumlar vardır. Buna geçici sürücüdeki tüm verileri sisteme önemli olan verilerin olmamalıdır. Geçici sürücü, uygulama performansını artırmak için bir veri önbelleği olarak kullanan bir uygulama tasarlarken, tasarımınızda başlatma işlemi sırasında geçici sürücü üzerindeki veri önbelleği kayıp olduğundan ve uygulamanın önce bir benzer veri önbelleğini yeniden derleme için zaman ihtiyacı varsayılır Performans ulaşıldı.

Geçici disk pagefile.sys depolamak için kullanılır ve bir varsayılan olarak D: sürücüsü etiketlenir. Bu farklı bir sürücü harfi diske yeniden eşlemek için bkz: [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md). Geçici diskin boyutunu, sanal makine boyutuna göre değişir. Daha fazla bilgi için [boyutları için Windows sanal makineleri](sizes.md).

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz. [Microsoft Azure sanal makineler üzerinde geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

### <a name="data-disk"></a>Veri diski

Veri diski uygulama verileri veya tutmak için ihtiyacınız olan diğer verileri depolamak için bir sanal makineye bağlı bir vhd'dir. Veri diskleri SCSI sürücüsü olarak kaydedilir ve seçtiğiniz bir harf ile etiketlenir. Her veri diski 4.095 GB'lık maksimum kapasiteye sahiptir, yönetilen diskler 32.767 GiB kapasiteye sahiptir. Bunu ve depolama türünü ekleyebilirsiniz kaç veri diskinin diskleri barındırmak için kullanabileceğiniz sanal makinenin boyutunu belirler.

> [!NOTE]
> Sanal makineler kapasiteler hakkında daha fazla bilgi için bkz. [boyutları için Windows sanal makineleri](sizes.md).

Bir görüntüden sanal makine oluşturduğunuzda azure, bir işletim sistemi diski oluşturur. Veri diskleri içeren bir görüntü kullanırsanız, sanal makine oluşturduğunda, Azure veri diskleri de oluşturur. Aksi takdirde, sanal makineyi oluşturduktan sonra veri diski ekleyin.

Veri diskleri için sanal makine herhangi bir zamanda göre ekleyebileceğiniz **ekleme** sanal makineye disk. Karşıya yüklenen veya depolama hesabınıza kopyalanır bir VHD kullanın veya Azure sizin için oluşturduğu boş bir VHD kullanın. Bir veri diski eklemeyi hala bağlıyken depolamadan silinemez şekilde VHD'de 'kira' yerleştirerek VHD dosyasını VM ile ilişkilendirir.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

Önizleme boyutlar için bkz. bizim [SSS](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged) bulunan hangi bölgelerde öğrenin.

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Bir son öneri: Standart yönetilmeyen diskler ile KIRPMA kullanma

Yönetilmeyen standart diskler (HDD) kullanıyorsanız, KIRPMA etkinleştirmeniz gerekir. Yalnızca gerçekten kullandığınız depolama alanı için faturalandırılır, böylece TRIM diskte kullanılmayan blokları atar. Büyük dosyaları oluşturup ardından bunları silerseniz bu maliyetlerinden tasarruf edebilirler.

KIRPMA ayarı denetlemek için şu komutu çalıştırabilirsiniz. Windows VM ve tür üzerindeki bir komut istemi açın:

```
fsutil behavior query DisableDeleteNotify
```

Komut 0 döndürürse, KIRPMA doğru şekilde etkinleştirilir. 1 döndürür, KIRPMA etkinleştirmek için aşağıdaki komutu çalıştırın:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Not: Kırpma desteği, Windows Server 2012 ile başlayan / Windows 8 ve yukarıda bkz [yeni API "TRIM ve eşlemeyi" ipuçları depolama ortamına göndermek, uygulamaların verir](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Sonraki adımlar
* [Bir diski](attach-disk-portal.md) VM'niz için ek depolama alanı eklemek için.
* [Anlık görüntü oluşturma](snapshot-copy-managed-disk.md).
* [Yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md).


