---
title: Windows için Azure özel betik uzantısı | Microsoft Docs
description: Özel betik uzantısı kullanarak Windows VM yapılandırma görevleri otomatikleştirme
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: danis
ms.openlocfilehash: 3a979df6148ae396cf5d9a34dc5f2eb0d455b736
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32191926"
---
# <a name="custom-script-extension-for-windows"></a>Windows için özel betik uzantısı

Özel betik uzantısının indirir ve Azure sanal makinelerde komut dosyaları çalıştırılır. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak da çalıştırılabilir.

Bu belge Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma ayrıntılarını verir.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]  
> Özel betik uzantısının kendisini bekleyeceği olduğundan, parametre olarak aynı VM ile güncelleştirme-AzureRmVM çalıştırmak için kullanmayın.  
>   
> 

### <a name="operating-system"></a>İşletim Sistemi

Windows için özel betik uzantısı çalıştırılabilir 2012, 2012 R2 ve 2016 Windows Server 2008 R2, Windows 10 istemci karşı serbest bırakır.

### <a name="script-location"></a>Komut dosyası konumu

Komut dosyası, Azure Blob Depolama veya başka bir konuma geçerli bir URL ile erişilebilir depolanması gerekir.

### <a name="internet-connectivity"></a>Internet bağlantısı

Windows için özel betik uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması özel betik uzantısı gösterir. Uzantısı için bir komut dosyası konumuna (Azure Storage veya başka bir konuma geçerli bir URL ile) ve bir komutun yürütülmesi için gerekiyor. Azure Storage komut dosyası kaynağı olarak kullanıyorsanız, bir Azure depolama hesabı adı ve hesap anahtarı gereklidir. Bu öğeler hassas verisi olarak kabul edilir ve uzantıları korumalı ayarı yapılandırmasında belirtilen. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Yayımcı | Microsoft.Compute |
| type | Uzantıları |
| typeHandlerVersion | 1.9 |
| fileUris (örneğin) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (örneğin) | PowerShell - ExecutionPolicy Unrestricted - dosya yapılandırma-müzik-app.ps1 |
| storageAccountName (örneğin) | examplestorageacct |
| storageAccountKey (örneğin) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== |

**Not** -bu özellik adları büyük/küçük harfe duyarlıdır. Dağıtım sorunlarını önlemek için yukarıda görülen adları kullanın.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında özel betik uzantısının çalıştırmak için kullanılabilir. Özel betik uzantısı içeren bir örnek şablonu burada bulunabilir [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell dağıtım

`Set-AzureRmVMCustomScriptExtension` Komutu, varolan bir sanal makineye özel betik uzantısı eklemek için kullanılabilir. Daha fazla bilgi için bkz: [kümesi AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure PowerShell modülü kullanılarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için aşağıdaki komutu çalıştırın.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Uzantı yürütme çıktısını hedef sanal makinede aşağıdaki dizini altında bulunan dosyaları için günlüğe kaydedilir.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Belirtilen dosyalar hedef sanal makinedeki şu dizine yüklenir.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Burada `<n>` hangi uzantısı yürütmeleri arasında değişebilir ondalık bir tamsayıdır.  `1.*` Değerle gerçek, geçerli `typeHandlerVersion` uzantının değerini.  Örneğin, gerçek dizin olabilir `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Yürütülürken `commandToExecute` komutunu uzantısına ayarlayacak bu dizin (örneğin, `...\Downloads\2`) geçerli çalışma dizini olarak. Bu aracılığıyla indirilen dosyaları bulmak için göreli yolların kullanımını etkinleştirir `fileURIs` özelliği. Örnekler için aşağıdaki tabloya bakın.

Mutlak indirme yolunu zaman içinde değişebildiğinden göreli komut dosyası yolları için kabul etmek daha iyi `commandToExecute` , mümkün olduğunda dize. Örneğin:
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

İlk URI segmenti aracılığıyla karşıdan yüklenen dosyalar için tutulmaktadır sonra yol bilgisi `fileUris` özellik listesi.  Aşağıdaki tabloda gösterildiği gibi karşıdan yüklenen dosyalar indirme alt yapısını yansıtacak şekilde eşlenir `fileUris` değerleri.  

#### <a name="examples-of-downloaded-files"></a>Karşıdan yüklenen dosyalar örnekleri

| FileUris URI | Göreli indirilen konumu | Mutlak konum indirilen * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Olarak yukarıdaki mutlak dizin yolları VM ancak CustomScript uzantısını tek yürütülmesi içinde değil dağıtımınızın ömrü boyunca değiştirir.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
