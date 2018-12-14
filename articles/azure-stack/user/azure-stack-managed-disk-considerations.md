---
title: Farklılıklar ve dikkat edilmesi gerekenler Azure Stack'te yönetilen diskler | Microsoft Docs
description: Azure Stack'te yönetilen diskler ile çalışırken farklılıklar ve dikkat edilmesi gerekenler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 9eed4c4bd8cd6290bd2126c91bcf4e37c1b0fa0b
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341958"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack yönetilen diskler: Farklılıklar ve dikkat edilmesi gerekenler
Bu makalede, Azure Stack yönetilen diskler için Azure yönetilen diskler arasındaki bilinen farklar özetlenmektedir. Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

Yönetilen diskler yöneterek Iaas Vm'leri için disk yönetimini basitleştirir [depolama hesapları](/azure/azure-stack/azure-stack-manage-storage-accounts) VM diskleriyle ilişkili.

> [!Note]  
> Azure Stack'te yönetilen diskler, 1808 kullanılabilir.
  

## <a name="cheat-sheet-managed-disk-differences"></a>Kopya kağıdı: Yönetilen disk farkları

| Özellik | Azure (Genel) | Azure Stack |
| --- | --- | --- |
|Bekleyen veriler için şifreleme |Azure depolama hizmeti şifrelemesi (SSE), Azure Disk şifrelemesi (ADE)     |BitLocker 128 bit AES şifrelemesi      |
|Görüntü          | Yönetilen bir özel görüntü desteği |Henüz desteklenmiyor|
|Yedekleme seçenekleri |Azure Yedekleme hizmetini destekler |Henüz desteklenmiyor |
|Olağanüstü durum kurtarma seçenekleri |Azure Site kurtarma desteği |Henüz desteklenmiyor|
|Disk türleri     |Premium SSD, standart SSD (Önizleme) ve standart HDD |Premium SSD, standart HDD |
|Premium diskler  |Tam olarak desteklenir |Performans sınır sağlanabilir veya garanti  |
|Premium disklerde IOPS  |Disk boyutuna bağlıdır.  |2300 disk başına IOPS |
|Premium disk aktarım hızı |Disk boyutuna bağlıdır. |Disk başına 145 MB/saniye |
|Disk boyutu  |Azure Premium Disk: P4 (32 GiB) için P80 (32 tib'a kadar)<br>Azure standart SSD Disk: E10 (128 Gib'a) için E80 (32 tib'a kadar)<br>Azure standart HDD Disk: S4 (32 GiB) için S80 (32 tib'a kadar) |M4: 32 GiB<br>M6: 64 GiB<br>M10: 128 GiB<br>M15: 256 giB<br>M20: 512 GiB<br>M30: 1024 giB |
|Disk anlık görüntü kopyalama|Anlık görüntü Azure yönetilen diskler desteklenen bir çalışan VM bağlı|Henüz desteklenmiyor |
|Disk performansı analiz |Ölçümleri toplamak ve desteklenen disk ölçüm başına |Henüz desteklenmiyor |
|Geçiş      |Var olan yönetilmeyen Azure Resource Manager sanal Makineyi yeniden oluşturmaya gerek kalmadan geçirmek için araç sağlar  |Henüz desteklenmiyor |

> [!Note]  
> Yönetilen disklerin IOPS ve aktarım hızı, Azure Stack ise donanım ve Azure Stack'te çalışan iş yükleri tarafından etkilenmiş bir sağlanan sayı yerine bir cap numarasıdır.


## <a name="metrics"></a>Ölçümler
Depolama ölçümleri farklılıklar vardır:
- Azure Stack ile depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayrım yapmaz.
- Azure Stack işlem verileri depolama ölçümleri, bağlama diskleri için sanal makine erişimini içermez.


## <a name="api-versions"></a>API sürümleri
Azure Stack yönetilen diskler aşağıdaki API sürümlerini destekler:
- 2017-03-30


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack sanal makineleri hakkında bilgi edinin](azure-stack-compute-overview.md)
