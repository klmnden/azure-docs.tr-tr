---
title: Windows için OMS Azure sanal makine uzantısı | Microsoft Docs
description: OMS Aracısı'nı kullanarak bir sanal makine uzantısı Windows sanal makine dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danis
ms.openlocfilehash: c365c43eb5abb975bf77e28ad061ff091f5ec627
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942644"
---
# <a name="oms-virtual-machine-extension-for-windows"></a>Windows için OMS sanal makine uzantısı

Operations Management Suite (OMS) bulut izleme, uyarma ve uyarı düzeltme özellikleri sağlar ve şirket içi varlıklar. Windows için OMS Aracısı sanal makine uzantısı yayımlanan ve Microsoft tarafından desteklenmiyor. Uzantı Azure sanal makinelerde OMS Aracısı'nı yükler ve sanal makineleri olan bir OMS çalışma kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için OMS sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Windows, Windows Server 2008 R2 karşı çalıştırılabilir için OMS aracısının uzantısı 2012, 2012 R2 ve 2016 serbest bırakır.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi otomatik olarak OMS aracısı sağlar ve Azure aboneliğinin varsayılan günlük analizi çalışma bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması yapılandırılmış çalışma ve bağlantıyı kesme Azure Güvenlik Merkezi ile üzerine yazar.

### <a name="internet-connectivity"></a>İnternet bağlantısı
Windows için OMS aracısının uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması OMS Aracısı uzantısı gösterir. Uzantı çalışma alanı kimliği ve hedef OMS çalışma alanından bir çalışma alanı anahtarı gerektirir, bu OMS Portalı'nda bulunabilir. Çalışma alanı anahtarı hassas verileri olarak değerlendirilmesi için bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi. Unutmayın **Workspaceıd** ve **workspaceKey** büyük küçük harfe duyarlıdır.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Yayımcı | Microsoft.EnterpriseCloud.Monitoring |
| type | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| Workspaceıd (örneğin) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örneğin) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonu OMS Aracısı uzantısı bir Azure Resource Manager şablon dağıtımı sırasında çalıştırmak için kullanılabilir. OMS Aracısı VM uzantısı içeren bir örnek şablonu bulunabilir [Azure hızlı başlangıç Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

Bir sanal makine uzantısı için JSON içinde sanal makine kaynağı iç içe geçmiş veya kök veya Resource Manager JSON şablonu en üst düzeyinde yerleştirilir. JSON yerleşimini kaynak adı ve türü değeri etkiler. Daha fazla bilgi için bkz: [Ayarla alt kaynakları için ad ve tür](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

Aşağıdaki örnek, OMS uzantısı içinde sanal makine kaynağı iç içe geçmiş varsayar. Uzantı kaynak iç içe geçirme sırasında JSON yerleştirilir `"resources": []` sanal makinenin nesnesi.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

JSON uzantısı şablon kökünde yerleştirirken, kaynak adı üst sanal makine için referans içeriyor ve iç içe geçmiş yapılandırma türü yansıtır. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureRmVMExtension` Komutu, OMS Aracısı sanal makine uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanması gerekir. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure PowerShell modülünü kullanarak şu komutu çalıştırın.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
