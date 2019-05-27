---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 880b630ae48eda086f6454f0d7108d27d3403b77
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161215"
---
Bir DHCP olmayan ortamda yedekleme ön yükleme yaparsanız, veri kutusu ağ geçidi için sanal makineyi dağıtmak için aşağıdaki adımları izleyin.

1. [Cihazın Windows PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Get-HcsIpAddress` sanal Cihazınızda etkin ağ arabirimleri listelemek için kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```
    [10.100.10.10]: PS>Get-HcsIpAddress

    OperationalStatus : Up
    Name              : Ethernet
    UseDhcp           : True
    IpAddress         : 10.100.10.10
    Gateway           : 10.100.10.1
    ```

3. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıdaki örneğe bakın:

    ```
    Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1
    ```

