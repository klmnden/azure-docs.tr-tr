---
title: Azure'da Windows sanal makineler dağıtmanız | Microsoft Docs
description: RDP bağlantı sorunlarını azaltmak için azure'da Windows sanal makineleri yeniden Dağıt yapma.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: f0bda14634e6c8bea5800b9798086fc38279030a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Windows sanal makineyi yeni Azure düğüme yeniden dağıtın
Uzak Masaüstü (RDP) sorunlarını giderme, sorunlar karşılıklı durumunda VM dağıtarak bağlantı ya da uygulama erişimi Windows tabanlı Azure sanal makine (VM) yardımcı olabilir. Bir VM yeniden dağıtırken VM Azure altyapısı içinde yeni bir düğüme taşır ve tüm yapılandırma seçenekleri ve ilişkili kaynakları geri korur, çalıştırır. Bu makale Azure PowerShell veya Azure portalını kullanarak bir VM'i yeniden gösterilmiştir.

> [!NOTE]
> Bir VM dağıtmanız sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkilendirilmiş dinamik IP adreslerini güncelleştirilir. 


## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
En son Azure PowerShell olduğundan emin olun, makinenizde yüklü 1.x. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Aşağıdaki örnek adlı VM dağıtır `myVM` kaynak grubunda adlı `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, özel Yardım bulabilirsiniz [RDP bağlantı sorunlarını giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [sorun giderme adımları RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). VM üzerinde çalışan bir uygulama, erişemiyor, ayrıca okuyabilirsiniz [uygulama sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

