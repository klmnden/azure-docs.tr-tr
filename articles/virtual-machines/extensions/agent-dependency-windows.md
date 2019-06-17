---
title: Windows için Azure İzleyici bağımlılık sanal makine uzantısı | Microsoft Docs
description: Sanal makine uzantısı'nı kullanarak Windows sanal makinesinde Azure İzleyici bağımlılık aracısını dağıtın.
services: virtual-machines-windows
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: magoedte
ms.openlocfilehash: 34dd872db199a4c10e9f321457188b7f7642944d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120218"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-windows"></a>Windows için Azure İzleyici bağımlılık sanal makine uzantısı

Vm'leri Haritası özelliği için Azure İzleyici verilerini Microsoft Dependency Aracıdan alır. Windows için Azure VM bağımlılık Aracısı sanal makine uzantısı yayımlandı ve Microsoft tarafından desteklenmiyor. Uzantı, Azure sanal makinelerinde bağımlılık aracısını yükler. Bu belge, desteklenen platformlar, yapılandırmaları ve Windows için Azure VM bağımlılık aracısını sanal makine uzantısı için dağıtım seçenekleri açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="operating-system"></a>İşletim sistemi

Windows için Azure VM bağımlılık Aracısı uzantısı listelenen desteklenen işletim sistemlerine karşı çalıştırabilirsiniz [desteklenen işletim sistemleri](../../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) Vm'leri dağıtma makalesi için Azure İzleyici bölümü.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema Azure VM bağımlılık Aracısı uzantısı için bir Azure Windows sanal makinesinde gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Azure VM. Supported Windows Server versions:  2008 R2 and above (x64)."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değeri/örneği |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| publisher | Microsoft.Azure.Monitoring.DependencyAgent |
| türü | DependencyAgentWindows |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtabilirsiniz. Bir Azure Resource Manager şablon dağıtımı sırasında Azure VM bağımlılık Aracısı uzantısı çalıştırmak için bir Azure Resource Manager şablonu önceki bölümde ayrıntılı JSON Şeması'nı kullanabilirsiniz.

Sanal makine uzantısı için JSON içinde sanal makine kaynağı içe olabilir. Veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirebilirsiniz. Kaynak adı ve türü değeri JSON yerleşimini etkiler. Daha fazla bilgi için [ayarlamak için alt kaynakları ad ve tür](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources).

Aşağıdaki örnek, bağımlılık Aracısı uzantısı sanal makine kaynağı içinde iç içe varsayar. Uzantı kaynak, iç içe, JSON yerleştirildi `"resources": []` sanal makinenin nesne.


```json
{
    "type": "extensions",
    "name": "DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

Uzantı JSON şablonu kökünde yerleştirdiğinizde, kaynak adı üst sanal makineye bir başvuru içerir. Tür, iç içe geçmiş yapılandırma yansıtır.

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtım

Kullanabileceğiniz `Set-AzVMExtension` bağımlılık aracısını sanal makine uzantısı varolan bir sanal makineye dağıtmak için komutu. Komutu çalıştırmadan önce genel ve özel yapılandırmaları bir PowerShell karma tablosunda depolanan gerekir.

```powershell

Set-AzVMExtension -ExtensionName "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ExtensionType "DependencyAgentWindows" `
    -TypeHandlerVersion 9.5 `
    -Location WestUS 
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure PowerShell modülü kullanarak aşağıdaki komutu çalıştırın:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Monitoring.DependencyAgent\
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Veya bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **Destek**. Azure desteği kullanma hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
