---
title: Windows için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı | Microsoft Docs
description: Ağ İzleyicisi Aracısı üzerinde bir sanal makine uzantısı'nı kullanarak Windows sanal makine dağıtın.
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
ms.openlocfilehash: 6e02f5a5b42da9c99a08782903cdc05ee32ec9d4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743335"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Windows için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md) Azure ağlarını izleme izin veren bir ağ performansı izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi Aracısı sanal makine uzantısı, isteğe bağlı ağ trafiğini ve diğer gelişmiş işlevleri Azure sanal makinelerinde yakalamak için bir gereksinimdir.


Bu belge, Windows için Ağ İzleyicisi Aracısı sanal makine uzantısı için dağıtım seçenekleri ve desteklenen platformlar ayrıntıları. Aracı yüklemesini değil kesintiye veya sanal makinenin yeniden başlatılmasını gerektirir. Dağıttığınız sanal makinelerine uzantısı dağıtabilirsiniz. Bir Azure hizmeti tarafından dağıtılan sanal makine, sanal makine uzantıları yükleme izin olup olmadığını belirlemek üzere hizmeti belgelerini denetleyin.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Windows, Windows Server 2008 R2 karşı çalıştırılabilir için Ağ İzleyicisi Aracısı uzantısı 2012, 2012 R2 ve 2016 serbest bırakır. Nano sunucu şu anda desteklenmiyor.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Ağ İzleyicisi Aracısı işlevlerinden bazıları hedef sanal makineyi Internet'e bağlı gerekir. Giden bağlantı özelliği, Ağ İzleyicisi Aracısı paket yakalamaları depolama hesabınıza yükleme olanağınız olmayacaktır. Daha fazla ayrıntı için lütfen bkz [Ağ İzleyicisi belgeleri](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Uzantı şeması

Ağ İzleyicisi Aracısı Uzantı Şeması aşağıdaki JSON'u göstermektedir. Uzantı ne gerektiren ya da, kullanıcı tarafından sağlanan ayarları destekler ve varsayılan yapılandırmasına dayanır.

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

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtabilirsiniz. Bir Azure Resource Manager şablon dağıtımı sırasında Ağ İzleyicisi Aracısı uzantısı çalıştırmak için bir Azure Resource Manager şablonu önceki bölümde ayrıntılı JSON Şeması'nı kullanabilirsiniz.

## <a name="powershell-deployment"></a>PowerShell dağıtım

Kullanım `Set-AzVMExtension` mevcut bir sanal makine için Ağ İzleyicisi Aracısı sanal makine uzantısını dağıtmak için komutu:

```powershell
Set-AzVMExtension `
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

Azure portalı ve PowerShell'de uzantısı dağıtımları durumuyla ilgili veri alabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure PowerShell modülü kullanarak şu komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, Ağ İzleyicisi Kullanıcı Kılavuzu belgelere başvurun veya Azure uzmanlarından ulaşın [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
