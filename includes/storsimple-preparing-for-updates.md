---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4e262c9e5bb88e77bc9c09853c06f4cdb41eedaa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188564"
---
## <a name="preparing-for-updates"></a>Güncelleştirmeler için hazırlama
Tarama güncelleştirmeyi uygulamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Cihaz verileri bulut anlık görüntüsünü alın.
2. Denetleyicinizi sabit IP'ler yönlendirilebilir ve Internet'e bağlanabilir emin olun. Bu sabit IP'lerin cihazınıza güncelleştirmeleri hizmeti için kullanılacak. Bu her denetleyicisinde cihazın Windows PowerShell arabiriminden aşağıdaki cmdlet'i çalıştırarak test edebilirsiniz:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network>`
   
    **Test-Connection sabit Ip'sinin Internet'e bağlanabilir zaman için örnek çıktı**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Bu el ile ön denetimleri başarıyla tamamladıktan sonra taramak ve güncelleştirmeleri yüklemek için geçebilirsiniz.

