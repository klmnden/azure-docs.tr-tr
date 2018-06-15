---
title: Fiziksel bellek kapasitesi Azure yığınının yönetme | Microsoft Docs
description: İzleme ve kullanılabilir depolama alanı için Azure yığınına yönetme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: Thomas.Roettinger
ms.openlocfilehash: dc572353c2e27ddfbae2398f1aece56586955e26
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34074866"
---
<!---Loc Comment: Please, check the comment in coversation section---> 
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Fiziksel bellek kapasitesi Azure yığınının yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Azure yığın toplam kullanılabilir bellek kapasitesini artırmak için ek bellek ekleyebilirsiniz. Azure yığınında fiziksel sunucu olarak da adlandırılan bir *ölçek birimi düğümü*. Tek ölçek biriminin üyesi olan tüm ölçek birimi düğümleri aynı miktarda bellek olmalıdır.

> [!note]  
> Devam etmeden önce olmadığını görmek için donanım üreticisinin sağladığı belgelere bir üreticiniz fiziksel bellek yükseltmeyi destekler. OEM donanım satıcısı destek sözleşmesi satıcı fiziksel sunucu rafa yerleştirme ve aygıt üretici yazılımı güncelleştirmesi gerçekleştirmek gerektirebilir.

Aşağıdaki akış diyagramı her ölçek birimi düğüme bellek eklemek için genel süreç gösterilmektedir.

![Bellek her ölçek birimi düğümüne ekleyin](media\azure-stack-manage-storage-physical-capacity\process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Varolan bir düğümü için bellek ekleyin
Aşağıdaki adımlar Ekle bellek işlemi üst düzey bir genel bakış sağlar. 

> [!Warning]  
OEM tarafından sağlanan belgelerinize bakarak olmadan adımları izlemeyin.

> [!Warning]  
Bellek yükseltme desteklenmiyor gibi tüm ölçek birimi kapatılmalıdır.

1. Azure konusunda belgelenen adımları kullanarak yığın Durdur [başlatma ve durdurma Azure yığın](azure-stack-start-and-stop.md) makalesi.
2. Donanım üreticisinin sağladığı belgelere kullanan her fiziksel bilgisayarda bellek yükseltin.
3. Azure içinde yer alan adımları kullanarak yığını başlatmak [başlatma ve durdurma Azure yığın](azure-stack-start-and-stop.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

 - Azure bulmak, kurtarmak ve iş ihtiyaçlarına göre depolama kapasiteyi geri kazanmak için yığın depolama hesapları yönetmek öğrenmek için bkz: [depolama hesaplarını Azure yığınında yönetmek](azure-stack-manage-storage-accounts.md).
 - Azure yığın bulut operatörü izler ve bunların Azure yığın dağıtımına depolama kapasitesini yönetir öğrenmek için bkz: [Azure yığın depolama kapasitesi yönetmek](azure-stack-manage-storage-shares.md). 
