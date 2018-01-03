---
title: "Bir Windows VM özel betik uzantısı | Microsoft Docs"
description: "Uzak bir Windows VM PowerShell betikleri çalıştırmak için özel betik uzantısı kullanarak Azure VM yapılandırma görevleri otomatikleştirme"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: danis
ms.openlocfilehash: b62cf38b2b16bd54b4df38402e8b7d75f5fd5e68
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a>Özel betik uzantısı Klasik dağıtım modeli kullanılarak Windows için

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. [Bu adımları Resource Manager modeli kullanarak gerçekleştirmeyi](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) öğrenin.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Özel betik uzantısının indirir ve Azure sanal makinelerde komut dosyaları çalıştırılır. Bu dağıtım yapılandırmaları, yazılım yükleme veya başka bir yapılandırma için yararlı bir uzantısıdır / yönetim görevi. Komut dosyaları Azure depolama veya GitHub indirilen veya çalışma zamanı uzantısı Azure portalında sağlanmaktadır. Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalında veya Azure sanal makine REST API'sini kullanarak da çalıştırılabilir.

Bu belge Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma ayrıntılarını verir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim Sistemi

Windows, Windows Server 2008 R2 karşı çalıştırılabilir için özel betik uzantısının 2012, 2012 R2 ve 2016 serbest bırakır.

### <a name="script-location"></a>Komut dosyası konumu

Komut dosyası Azure depolama veya başka bir konuma geçerli bir URL ile erişilebilir depolanması gerekir.

### <a name="internet-connectivity"></a>Internet bağlantısı

Windows için özel betik uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı Şeması

Aşağıdaki JSON şeması özel betik uzantısı gösterir. Uzantısı için bir komut dosyası konumuna (Azure Storage veya başka bir konuma geçerli bir URL ile) ve bir komutun yürütülmesi için gerekiyor. Azure Storage komut dosyası kaynağı olarak kullanıyorsanız, bir Azure depolama hesabı adı ve hesap anahtarı gereklidir. Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi.

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
| Uzantısı | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (örneğin) | https://RAW.githubusercontent.com/Microsoft/dotnet-Core-Sample-Templates/master/dotnet-Core-Music-Windows/scripts/Configure-Music-App.ps1 |
| commandToExecute (örneğin) | PowerShell - ExecutionPolicy Unrestricted - dosya yapılandırma-müzik-app.ps1 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısının çalıştırmak için kullanılabilir. Özel betik uzantısı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureVMCustomScriptExtension` Komutu, varolan bir sanal makineye özel betik uzantısı eklemek için kullanılabilir. Daha fazla bilgi için bkz: [kümesi AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

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

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme bize hedef sanal makinede aşağıdaki dizinde bulunan dosyaları oturum çıktı.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Betik hedef sanal makinedeki şu dizine yüklenir.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/en-us/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/en-us/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/en-us/support/faq/).