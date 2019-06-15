---
title: Uzantı sıralama Azure Disk şifrelemesi ve Azure sanal makine ölçek kümeleri
description: Bu makalede, Linux Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi etkinleştirme hakkında yönergeler sağlar.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/21/2019
ms.openlocfilehash: e98e501806971f3cf1bec29960ad15ef9c0024fc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60611263"
---
# <a name="use-azure-disk-encryption-with-virtual-machine-scale-set-extension-sequencing"></a>Uzantı sıralama kullanımı Azure Disk şifrelemesi ile sanal makine ölçek kümesi

Azure disk şifrelemesi gibi uzantıları için bir Azure sanal makineler ölçek eklenebilir belirli bir sırada ayarlayın. Bunu yapmak için [uzantı sıralama](../virtual-machine-scale-sets/virtual-machine-scale-sets-extension-sequencing.md). 

Genel olarak, şifreleme için bir disk uygulanmalıdır:

- Uzantıları veya özel betikler sonra disklerin veya birimlerin hazırlayın.
- Uzantıları veya özel betikler önce erişmek veya şifrelenmiş disklerin veya birimlerin üzerinde verileri kullanır.

Her iki durumda da `provisionAfterExtensions` özelliği atayan hangi uzantısı dizisine eklenmesi gerekir.

## <a name="sample-azure-templates"></a>Örnek Azure şablonları

Sahip olmasını isterseniz, Azure Disk şifrelemesi sonra başka bir uzantı uygulanır, put `provisionAfterExtensions` AzureDiskEncryption uzantısı bloğundaki özelliği. 

"CustomScriptExtension", başlatır ve ardından "Tarafından AzureDiskEncryption" bir Windows diski biçimlendirir bir Powershell betiği kullanarak bir örnek aşağıda verilmiştir:

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "CustomScriptExtension",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.9",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "fileUris": [
              "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/ade-vmss/FormatMBRDisk.ps1"
            ]
          },
          "protectedSettings": {
           "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File FormatMBRDisk.ps1"
          }
        }
      },
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "AzureDiskEncryption",
        "location": "[resourceGroup().location]",
        "properties": {
          "provisionAfterExtensions": [
            "CustomScriptExtension"
          ],
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
    ]
  }
}
```

Olmasını istiyorsanız, önce başka bir uzantı Azure Disk şifrelemesi uygulandı, put `provisionAfterExtensions` bloğundaki izlemek için uzantının özelliği.

"Windows tabanlı Azure sanal makinesinde ardından"VMDiagnosticsSettings", izleme sağlayan bir uzantı ve tanılama özellikleri AzureDiskEncryption" kullanan bir örnek aşağıda verilmiştir:


```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "AzureDiskEncryption",
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "provisionAfterExtensions": [
            "AzureDiskEncryption"
          ],
        "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
    ]
  }
}
```

Daha fazla ayrıntılı şablonları için bkz:
* Azure Disk şifrelemesi uzantısını (Linux) diski biçimlendirir sonra bir özel Kabuk betiği Uygula: [deploy-extseq-linux-ADE-after-customscript.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-linux-ADE-after-customscript.json)
* Azure Disk şifrelemesi uzantısı'nı başlatır ve (Windows) diski biçimlendirir özel bir Powershell betiği sonra uygulayın: [deploy-extseq-linux-ADE-after-customscript.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-windows-ADE-after-customscript.json)
* Azure Disk şifrelemesi uzantısını başlatır ve (Windows) diski biçimlendirir özel bir Powershell betiği önce Uygula: [deploy-extseq-windows-CustomScript-after-ADE.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-windows-CustomScript-after-ADE.json)

## <a name="next-steps"></a>Sonraki adımlar
- Uzantı sıralama hakkında daha fazla bilgi edinin: [Sanal makine ölçek kümelerinde uzantı sağlama sıra](../virtual-machine-scale-sets/virtual-machine-scale-sets-extension-sequencing.md).
- Daha fazla bilgi edinin `provisionAfterExtensions` özelliği: [Microsoft.Compute virtualMachineScaleSets/uzantıları şablon başvurusu](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions).
