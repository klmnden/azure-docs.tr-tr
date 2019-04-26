---
title: Desired State Configuration sanal makine ölçek kümeleri ile kullanma | Microsoft Docs
description: Sanal makine ölçek kümeleri ile Azure DSC uzantısı
services: virtual-machine-scale-sets
documentationcenter: ''
author: zjalexander
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
keywords: ''
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: 24a37d352413ff9ac55ce8e189691988383950f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60204001"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Sanal makine ölçek kümeleri ile Azure DSC uzantısı
[Sanal makine ölçek kümeleri](virtual-machine-scale-sets-overview.md) kullanılabilir [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) uzantısı işleyicisi. Sanal makine ölçek kümeleri dağıtmak ve çok sayıda sanal makineleri yönetmek için bir yol sağlar ve giriş ve çıkış yanıt olarak yüklenecek şekilde ölçeklendirebilir. DSC Vm'leri için üretim yazılımı çalıştıran çevrimiçine bunlar yapılandırmak için kullanılır.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Sanal makineler ve sanal makine ölçek kümeleri dağıtma arasındaki farklar
Şablon temelindeki bir sanal makine ölçek kümesi için tek bir VM'den biraz farklıdır. Özellikle, tek bir VM uzantılarını "virtualMachines" düğümünün altında dağıtır. DSC şablona nereden eklenir "uzantıları" türünde bir giriş olan

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Bir "Özellikler" bölümüne "VirtualMachineProfile", "extensionProfile" özniteliği ile bir sanal makine ölçek kümesi düğümü vardır. DSC "uzantıları" altında eklenir

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi için davranışı
Bir sanal makine ölçek kümesi için davranışı davranış tek bir VM için aynıdır. Yeni bir VM oluşturulduğunda DSC uzantısı ile otomatik olarak sağlanır. WMF daha yeni bir sürümü uzantısı tarafından gerekiyorsa çevrimiçine önce VM'yi yeniden başlatır. Çevrimiçi olduktan sonra DSC yapılandırması .zip indirir ve VM sağlama. Daha fazla ayrıntı bulunabilir [Azure DSC uzantısına genel bakış](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar
İnceleme [DSC uzantısı için Azure Resource Manager şablonu](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Bilgi nasıl [DSC uzantısı kimlik bilgilerini güvenli bir şekilde işler](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Azure DSC uzantısı işleyicisine hakkında daha fazla bilgi için bkz. [Azure Desired State Configuration uzantısı işleyicisi giriş](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

PowerShell DSC hakkında daha fazla bilgi için [PowerShell belge merkezini ziyaret edin](https://msdn.microsoft.com/powershell/dsc/overview). 

