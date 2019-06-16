---
title: NIC devre dışı olduğundan, Azure sanal makineler için uzaktan bağlanamıyor | Microsoft Docs
description: RDP başarısız olduğu içinde NIC Azure sanal Makinesinde devre dışı bırakıldığından bir sorun gidermeyi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/12/2018
ms.author: genli
ms.openlocfilehash: 742026a8ff35f318f58674ebc2fb5c03e45161a8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319012"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Bir VM'ye Uzak Masaüstü Ağ arabirimini devre dışı bırakıldığından olamaz

Bu makalede, ağ arabirimini devre dışı bırakılırsa, Uzak Masaüstü Bağlantısı Azure Windows sanal makinelerine (VM'ler) yapamazsınız bir sorunun nasıl giderileceği açıklanmaktadır.

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modelini kullanarak kapsar.

## <a name="symptoms"></a>Belirtiler

Sanal makine içindeki ağ arabirimini devre dışı olduğundan azure'da bir VM ile RDP bağlantısı veya başka türde bir bağlantı diğer bağlantı noktalarına yapamazsınız.

## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

VM için arabirimi etkinleştirmek için seri denetimi kullanın veya [sıfırlama ağ arabirimi](##reset-network-interface) VM için.

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Seri konsol sanal makinenizde etkinleştirilmemiş olmadığını [sıfırlama ağ arabirimi](#reset-network-interface).
2. Ağ arabirimi durumunu kontrol edin:

        netsh interface show interface

    Devre dışı ağ arabirimi adını not edin.

3. Ağ arabirimi etkinleştirin:

        netsh interface set interface name="interface Name" admin=enabled

    Örneğin, birlikte işlemek arabirimi "Ethernet 2" adında, aşağıdaki komutu çalıştırın:

        netsh interface set interface name=""Ethernet 2" admin=enabled


4.  Yeniden ağ arabiriminin etkin olduğundan emin olmak için ağ arabirimin durumunu denetleyin.

        netsh interface show interface

    Bu noktada VM yeniden başlatma gerekmez. VM'yi geri erişilebilir olacaktır.

5.  VM'ye bağlanın ve sorunun çözülüp olup olmadığını görebilirsiniz.

## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlayın

Ağ arabirimi sıfırlamak için alt ağdaki kullanılabilir başka bir IP adresi IP adresini değiştirin. Bunu yapmak için Azure portal veya Azure PowerShell kullanın. Daha fazla bilgi için [sıfırlama ağ arabirimi](reset-network-interface.md).