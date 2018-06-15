---
title: Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı | Microsoft Docs
description: Ağ İzleyicisi Aracısı'nı kullanarak bir sanal makine uzantısı Windows sanal makine dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 29f346b2a42f8d12e26bd59fbab86d763d3f29f0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942637"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Windows için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md) Azure ağlarını izleme izin veren bir ağ performans izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi Aracısı sanal makine uzantısı isteğe bağlı ağ trafiği ve diğer gelişmiş işlevselliği Azure sanal makinelerde yakalamak için gerekli değildir.


Bu belge, Windows için Ağ İzleyicisi Aracısı sanal makine uzantısı için dağıtım seçenekleri ve desteklenen platformlar ayrıntıları. Aracı yüklemesini değil kesintiye veya sanal makinenin yeniden başlatılması gerekir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Windows, Windows Server 2008 R2 karşı çalıştırılabilir için Ağ İzleyicisi Aracısı uzantısı 2012, 2012 R2 ve 2016 serbest bırakır. Nano Server şu anda desteklenmiyor.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Bazı Ağ İzleyicisi Aracısı işlevlerini hedef sanal makine Internet'e bağlı olması gerekir. Giden bağlantıları kurmak özelliği, Ağ İzleyicisi Aracısı depolama hesabınıza paket yakalamaları yükleyebildiğini olmaz. Daha fazla ayrıntı için lütfen bkz. [Ağ İzleyicisi belgeleri](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması Ağ İzleyicisi Aracısı uzantısı gösterir. Uzantı ne gerektirir, ya da, tüm kullanıcı tarafından sağlanan ayarları destekler ve varsayılan yapılandırmasına dayanır.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Yayımcı | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtabilirsiniz. Bir Azure Resource Manager şablon dağıtımı sırasında Ağ İzleyicisi Aracısı uzantısı çalıştırmak için bir Azure Resource Manager şablonu önceki bölümde ayrıntılı JSON şeması kullanabilirsiniz.

## <a name="powershell-deployment"></a>PowerShell dağıtım

Kullanım `Set-AzureRmVMExtension` Ağ İzleyicisi Aracısı sanal makine uzantısı olan bir sanal makineyi dağıtmak için komutu:

```powershell
Set-AzureRmVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Azure portalı ve PowerShell uzantısı dağıtımları durumuyla ilgili verileri alabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure PowerShell modülünü kullanarak şu komutu çalıştırın:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, Ağ İzleyicisi Kullanıcı Kılavuzu belgelere bakın veya üzerinde Azure uzmanlar başvurun [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
