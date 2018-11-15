---
title: NIC devre dışı olduğundan Uzak Masaüstü Azure sanal makineler için kullanılamaz. | Microsoft Docs
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
ms.openlocfilehash: 6b14530bd6b4c1b6617cb1d5c88d710a32e5372c
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634726"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Bir VM'ye Uzak Masaüstü Ağ arabirimini devre dışı bırakıldığından olamaz

Bu makalede, ağ arabirimini devre dışı olduğundan Uzak Masaüstü Azure Windows sanal makinelerin (VM'ler) edilemez bir sorunun nasıl çözüleceği gösterilmektedir.

> [!NOTE] 
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modelini kullanarak kapsar. 

## <a name="symptoms"></a>Belirtiler 

Sanal makine içindeki ağ arabirimini devre dışı olduğundan azure'da bir VM ile RDP bağlantısı veya başka türde bir bağlantı diğer bağlantı noktalarına yapamazsınız.

## <a name="solution"></a>Çözüm 

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

VM için arabirimi etkinleştirmek için seri denetimi kullanın veya [sıfırlama ağ arabirimi](##reset-network-interface) VM için.

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md#open-cmd-or-powershell-in-serial-console
). Seri konsol sanal makinenizde etkinleştirilmemiş olmadığını [sıfırlama ağ arabirimi](#reset-network-interface).
2. Ağ arabirimi durumunu kontrol edin:

        netsh interface show interface

    Devre dışı ağ arabirimi adını not edin. 

3. Ağ arabirimi etkinleştirin:

        netsh interface set interface name="interface Name" admin=enabled

    Örneğin, "Ethernet 2" birlikte işlemek arabirimi adları ise aşağıdaki komutu çalıştırın:

        netsh interface set interface name=""Ethernet 2" admin=enabled
    

4.  Yeniden ağ arabiriminin etkin olduğundan emin olmak için ağ arabirimin durumunu denetleyin.

        netsh interface show interface

    Bu noktada VM yeniden başlatma gerekmez. VM'yi geri erişilebilir olacaktır.
        
5.  VM'ye bağlanın ve sorunun çözülüp çözülmediğine bakın.

## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlayın

Ağ arabirimi sıfırlamak için Azure portal veya Azure PowerShell kullanarak alt ağdaki kullanılabilir başka bir IP adresi IP adresini değiştirin. Daha fazla bilgi için [sıfırlama ağ arabirimi](reset-network-interface.md). 