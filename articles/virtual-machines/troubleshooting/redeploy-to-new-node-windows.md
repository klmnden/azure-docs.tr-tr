---
title: Azure'da Windows sanal makineleri yeniden | Microsoft Docs
description: RDP bağlantı sorunlarını gidermek için azure'da Windows sanal makineleri yeniden yapma.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: b71ca93ac3e3e6c77c5f87b4859a2e3e0e1040d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104090"
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Windows sanal makineyi yeni Azure düğümüne yeniden dağıtma
Uzak Masaüstü (RDP) sorun giderme, sorunlarla karşılaştığı değilse sanal makine yeniden dağıtıldığında erişim bağlantısı veya uygulama Windows tabanlı Azure sanal makine (VM) için yardımcı olabilir. Bir VM'yi yeniden dağıtma, Azure sanal makineyi, VM Azure altyapısı içinde yeni bir düğüme taşıyın ve tüm yapılandırma seçenekleri ve ilişkili kaynakları yeniden koruma, güç. Bu makalede, Azure PowerShell veya Azure portalını kullanarak VM'yi yeniden dağıtma işlemini göstermektedir.

> [!NOTE]
> Bir VM'yi yeniden dağıtma sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adresleri güncelleştirildi. 


## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
En son Azure PowerShell sahip olduğunuzdan emin olun 1.x makinenizde yüklü. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Aşağıdaki örnekte adlı bir VM dağıtır `myVM` adlı kaynak grubunda `myResourceGroup`:

```powershell
Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız, üzerinde belirli Yardım bulabilirsiniz [RDP bağlantı sorunlarını giderme](troubleshoot-rdp-connection.md) veya [ayrıntılı sorun giderme adımları RDP](detailed-troubleshoot-rdp.md). Sanal makinenizde çalışan bir uygulama varsa erişilemiyor, ayrıca okuyabilirsiniz [uygulama sorunlarını giderme](../windows/troubleshoot-app-connection.md).

