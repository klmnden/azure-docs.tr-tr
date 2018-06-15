---
title: Azure Site kurtarma yeniden çalışma | Microsoft Docs
description: Bu makalede, yeniden çalışma sırasında Azure Site Recovery hizmeti ile şirket içi dön başarısız olarak değerlendirilmesi için uyarılar ve çeşitli türlerine genel bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: guaravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanki
ms.openlocfilehash: 372a7867b47960338d7a1bf7e646fb9fffbe72e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29803561"
---
# <a name="overview-of-failback"></a>Yeniden çalışma genel bakış

Azure üzerinde başarısız oldu sonra şirket içi sitenize başarısız olabilir. Azure Site Recovery ile mümkün olan iki farklı tür geri dönme vardır: 

- Özgün konuma geri alabilirsiniz 
- Farklı bir konuma başarısız

Bir VMware sanal makinesi üzerinde başarısız olursa hala devam ediyorsa geri aynı kaynak şirket içi sanal makine için başarısız olabilir. Bu senaryoda, yalnızca değişiklikleri geri çoğaltılır. Bu senaryo olarak bilinen **özgün konuma kurtarma**. Şirket içi sanal makine yoksa senaryodur bir **alternatif konum kurtarma**.

> [!NOTE]
> Yalnızca özgün vCenter ve yapılandırma sunucusu için yeniden çalışma kullanabilirsiniz. Yeni bir yapılandırma sunucusu dağıtmak ve kullanarak geri başarısız. Ayrıca, yeni bir vCenter yeniden çalışma ve var olan yapılandırma sunucusu için yeni vCenter ekleyemezsiniz.

## <a name="original-location-recovery-olr"></a>Özgün konuma Kurtarma (OLR)
Özgün sanal makine yük seçerseniz, aşağıdaki koşulların karşılanması gerekir:

* Sanal makine bir vCenter sunucusu tarafından yönetiliyorsa, ana hedefin ESX konak sanal makinenin veri deposuna erişimi olmalıdır.
* Sanal makine bir ESX konağında olmakla birlikte vCenter tarafından yönetilmiyor, sabit disk sanal makinenin ana hedefin konak erişmek için bir veri deposu olması gerekir.
* Sanal makinenize bir ESX konağında ise ve vCenter kullanmaz, koruyun önce ana hedefinin ESX konağının bulma tamamlamanız gerekir. Geri fiziksel sunucuları, çok çalıştırma işlemini gerçekleştiriyorsanız bu geçerlidir.
* Bir sanal depolama alanı ağı (vSAN) veya diskleri zaten mevcutsa ve şirket içi sanal makineye bağlı (RDM) eşleme ham aygıt temel bir disk dön başarısız olabilir.

> [!IMPORTANT]
> Yeniden çalışma sırasında Azure Site Recovery hizmetine ilişkin bekleyen değişiklikleri yazılır sanal makinede özgün VMDK tanımlamak böylelikle disk.enableUUID= TRUE etkinleştirmek önemlidir. Bu değer ayarlanmamışsa TRUE olması, daha sonra hizmeti karşılık gelen şirket içi VMDK en iyi çaba olarak tanımlamak çalışır. Sağ VMDK bulunmazsa, ek bir disk oluşturur ve veri açın, yazılan.

## <a name="alternate-location-recovery-alr"></a>Alternatif konuma Kurtarma (alr özelliğini)
Senaryo, şirket içi sanal makine sanal makineyi yeniden korumayı önce mevcut değilse, alternatif bir konum kurtarma adı verilir. Yeniden koruma iş akışı, şirket içi sanal makine yeniden oluşturur. Bu ayrıca tam veri indirme neden olur.

* Farklı bir konuma başarısız olduğunda, sanal makine ana hedef sunucusu dağıtıldığı aynı ESX konak kurtarılır. Disk oluşturmak için kullanılan veri deposu, sanal makineyi yeniden korumayı, seçilen veri deposu olacaktır.
* Yük devredebilir geriye yalnızca bir sanal makine dosya sistemi (VMFS) veya vSAN veri deposu. Bir RDM varsa, yeniden koruma ve geri dönme çalışmaz.
* Yeniden koruma değişikliklerden ve ardından bir büyük ilk veri aktarımını içerir. Sanal makine şirket içinde var olmadığından bu işlem bulunmaktadır. Tam veri geri çoğaltılması gerekir. Bu yeniden koruma ayrıca bir özgün konuma kurtarma daha uzun sürer.
* Geri RDM tabanlı disklere kapatamazsınız. Yalnızca yeni sanal makine disklerini (VMDKs) VMFS/vSAN veri deposu üzerinde oluşturulabilir.

> [!NOTE]
> Fiziksel makine üzerinden Azure için başarısız olduğunda başarısız yalnızca VMware sanal makinesi olarak yedekleyin. Aynı iş akışı alternatif konuma kurtarma olarak izler. En az bir ana hedef sunucusu ve geri dönecek yapmanız gereken ESX/ESXi konaklarının Bul emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Adımları gerçekleştirmek için [yeniden çalışma işlemi](vmware-azure-failback.md).

