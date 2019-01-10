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
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: jiahan
ms.openlocfilehash: 3445974cf832b7ed594f704615482e1d9b0e351c
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159375"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack yönetilen diskler: farklılıklar ve dikkat edilmesi gerekenler

Bu makalede bilinen farklılıkları özetler [Azure Stack yönetilen diskler](azure-stack-manage-vm-disks.md) ve [yönetilen diskler için Azure](../../virtual-machines/windows/managed-disks-overview.md). Azure Stack ve Azure arasında üst düzey farklılıklar hakkında bilgi edinmek için bkz. [anahtar konuları](azure-stack-considerations.md) makalesi.

Yönetilen diskler yöneterek Iaas Vm'leri için disk yönetimini basitleştirir [depolama hesapları](../azure-stack-manage-storage-accounts.md) VM diskleriyle ilişkili.

> [!Note]  
> Azure Stack'te yönetilen diskler, 1808 Update'ten kullanılabilir. 1811 Update Azure Stack portalı kullanarak sanal makine oluştururken, varsayılan olarak etkindir.
  

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
|Geçiş      |Mevcut yönetilen Azure Resource Manager sanal Makineyi yeniden oluşturmaya gerek kalmadan geçirmek için araç sağlar  |Henüz desteklenmiyor |

> [!NOTE]  
> Yönetilen disklerin IOPS ve aktarım hızı, Azure Stack ise donanım ve Azure Stack'te çalışan iş yükleri tarafından etkilenmiş bir sağlanan sayı yerine bir cap numarasıdır.

## <a name="metrics"></a>Ölçümler

Depolama ölçümleri farklılıklar vardır:

- Azure Stack ile depolama ölçümleri işlem verilerinde iç veya dış ağ bant genişliği ayırmaz.
- Azure Stack işlem verileri depolama ölçümleri, bağlama diskleri için sanal makine erişimini içermez.

## <a name="api-versions"></a>API sürümleri

Azure Stack yönetilen diskler aşağıdaki API sürümlerini destekler:

- 2017-03-30

## <a name="known-issues"></a>Bilinen Sorunlar

1808 sonra güncelleştirmeleri uygulandıktan sonra yönetilen disklere sahip VM'ler dağıtırken aşağıdaki sorunlarla karşılaşabilirsiniz:

- Yönetilen disklerle bir VM dağıtma 1808 güncelleştirmeden önce Abonelik oluşturulurken bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için her abonelik için şu adımları izleyin:
   1. Kiracı Portalı'nda Git **abonelikleri** ve aboneliği bulunamıyor. Tıklayın **kaynak sağlayıcıları**, ardından **Microsoft.Compute**ve ardından **yeniden kaydettirin**.
   2. Aynı abonelik altında Git **erişim denetimi (IAM)**, doğrulayın **Azure Stack – yönetilen Disk** listelenir.
- Bir konuk dizin ile ilişkili bir abonelik içindeki Vm'leri dağıtma, çok kiracılı bir ortam yapılandırdıysanız, bir iç hata iletisi ile başarısız olabilir. Hatayı gidermek için aşağıdaki adımları izleyin. [bu makalede](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) her Konuk dizinlerinizi yeniden yapılandırmak için.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack sanal makineleri hakkında bilgi edinin](azure-stack-compute-overview.md)
