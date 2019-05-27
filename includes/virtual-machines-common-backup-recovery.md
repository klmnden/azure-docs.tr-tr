---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: be71b269e618d13a126e4005754b307e9c6517d7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66160059"
---
## <a name="azure-backup"></a>Azure Backup

Üretim iş yükleri çalıştıran Azure sanal makinelerini yedeklemek için Azure yedekleme kullanın. Azure Backup, Windows ve Linux VM'ler için uygulamayla tutarlı yedeklemeler yapılmasını destekler. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yükleme yaptığınızda VM’nin tamamını veya belirli dosyaları geri yükleyebilirsiniz. 

Bir basit, uygulamalı Azure Vm'leri için Azure Backup, "Azure sanal makinelerini yedekleme" öğretici için bkz [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

Azure Backup birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [azure'da VM yedekleme altyapınızı planlama](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery, bütün bir bölgeye ana doğal afet veya yaygın hizmet kesintisi nedeniyle kesinti yaşandığında önemli bir olağanüstü durumda senaryodan, Vm'lerinizi korur. Uygulamanızı yalnızca birkaç tıklama ile dakika kurtarabilmeniz için Azure Site Recovery yapılandırabilirsiniz. Tercih ettiğiniz bir Azure bölgesine çoğaltabilirsiniz, eşleştirilmiş bölgelerin sınırlı değildir. 

İsteğe bağlı test yük ile üretim iş yükleri veya sürmekte olan çoğaltmayı etkilemeden olağanüstü durum kurtarma tatbikatları çalıştırabilirsiniz. Yük devretme ve yeniden çalışma birden çok VM'de çalışan uygulamanın tamamı düzenlemek için kurtarma planları oluşturun. Kurtarma planı özelliği, Azure Otomasyonu runbook'ları ile tümleşiktir.

Tarafından oluşturabileceğinize dair [sanal makinelerinizi çoğaltma](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Yönetilen anlık görüntüler 

Geliştirme ve test ortamlarında anlık görüntüler, yönetilen diskleri kullanan sanal makineleri yedeklemeye hızlı ve basit bir seçenek sağlar. Yönetilen bir anlık görüntü, salt okunur bir tam kopya bir yönetilen diskin ' dir. Anlık görüntüler, kaynak disk bağımsız var ve VM yeniden oluşturma için yeni yönetilen diskler oluşturmak için kullanılabilir. Diskin kullanılan bölümü temel göre faturalandırılır. Örneğin, bir yönetilen diskin sağlanan kapasitesi 64 GB ve gerçek kullanılan veri boyutu 10 GB olan bir anlık görüntüsünü oluşturursanız, yalnızca kullanılan veri boyutu olan 10 GB için faturalandırılırsınız.  

Anlık görüntüleri oluşturma hakkında daha fazla bilgi için bkz:

* [Windows’da Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Sonraki adımlar
Azure Backup işlemi "Windows sanal makineleri öğretici yedekleme" izleyerek için deneyebileceğiniz [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).
