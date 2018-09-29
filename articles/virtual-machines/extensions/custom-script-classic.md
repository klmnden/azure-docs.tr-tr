---
title: Bir Windows VM'de özel betik uzantısı | Microsoft Docs
description: Uzak bir Windows VM'de PowerShell betikleri çalıştırmak için özel betik uzantısı kullanarak Azure VM yapılandırma görevlerini otomatikleştirme
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: roiyz
ms.openlocfilehash: 8eb7822962988b02f09c2a2ea31b745ef01d5533
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451859"
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a>Özel betik uzantısı için Klasik dağıtım modelini kullanarak Windows

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](custom-script-windows.md) öğrenin.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../includes/virtual-machines-classic-portal.md)]

Özel betik uzantısı indirir ve Azure sanal makinelerinde betikleri çalıştırır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak da çalıştırılabilir.

Bu belge, Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma işlemi açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim Sistemi

Windows, Windows Server 2008 R2 karşı çalıştırılabilir için özel betik uzantısı 2012, 2012 R2 ve 2016 serbest bırakır.

### <a name="script-location"></a>Betik konumu

Komut dosyası Azure depolama veya herhangi başka bir konumda geçerli bir URL ile erişilebilir depolanmış olması gerekir.

### <a name="internet-connectivity"></a>Internet bağlantısı

Windows için özel betik uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema için özel betik uzantısı gösterir. Betik konumu (Azure depolama veya başka bir konuma geçerli bir URL ile) ve yürütmek için bir komut uzantısı gerektirir. Betik kaynağı Azure depolama kullanırken, bir Azure depolama hesabı adını ve hesap anahtarı gereklidir. Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi.

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Yayımcı | Microsoft.Compute |
| Uzantı | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (örn.) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (örn.) | PowerShell - ExecutionPolicy sınırsız - dosya yapılandırma-müzik-app.ps1 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde açıklanan JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısı'nı çalıştırmak için kullanılabilir. Özel betik uzantısı'nı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureVMCustomScriptExtension` Komutu, mevcut bir sanal makine için özel betik uzantısı eklemek için kullanılabilir. Daha fazla bilgi için [kümesi AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme bize hedef sanal makinede aşağıdaki dizinde bulunan dosyaları oturum çıktı.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Betik, hedef sanal makinedeki aşağıdaki dizine yüklenir.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
