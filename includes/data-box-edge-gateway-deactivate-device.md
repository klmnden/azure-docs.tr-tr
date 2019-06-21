---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: aebb82690a7a49aba071ed64349d37d516208cca
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188832"
---
Cihazınızı sıfırlamak için güvenli bir şekilde dışarı veri diski ve cihazınızın önyükleme diski üzerindeki tüm verileri silme gerekir. 

Kullanım `Reset-HcsAppliance` cmdlet'i, veri disklerini ve önyükleme diski veya veri disklerini yalnızca out temizleme. `ClearData` Ve `BootDisk` anahtarlar önyükleme diski ve veri diskleri temizleme, sırasıyla izin verir.

Yerel web kullanıcı Arabiriminde sıfırlama cihaz kullandığınız, veri disklerini yalnızca güvenli bir şekilde temizlendiğinde ancak önyükleme diski olduğu gibi tutulur. Cihaz yapılandırması önyükleme diski içerir.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Komut istemine şunları yazın:

    `Reset-HcsAppliance -ClearData -BootDisk`

    Aşağıdaki örnek bu cmdlet'in nasıl kullanılacağı gösterilmektedir:
    ```
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk
    
    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
