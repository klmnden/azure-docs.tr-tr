---
title: Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı | Microsoft Docs
description: Ağ İzleyicisi Aracısı'nı bir sanal makine uzantısını kullanarak Linux sanal makine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: db508e2311602a66a2c252ffaa842f8bfb4f670b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076080"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Linux için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](/azure/network-watcher/) Azure ağları için izleme sağlayan bir ağ performans izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi Aracısı sanal makine (VM) uzantısı, isteğe bağlı ve diğer gelişmiş işlevler üzerindeki ağ trafiğini yakalama gibi Azure vm'lerinde Ağ İzleyicisi özelliklerinden bazıları için gerekli değildir.

Bu makalede desteklenen platformlar ve Linux için Ağ İzleyicisi Aracısı VM uzantısı için dağıtım seçeneklerini ayrıntıları. Aracı yüklemesini değil kesintiye veya VM, yeniden başlatma gerektirir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Ağ İzleyicisi Aracısı uzantısı aşağıdaki Linux dağıtımları için yapılandırılabilir:

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS ve 12.04 LTS |
| Debian | 7 ve 8 |
| RedHat | 6 ve 7 |
| Oracle Linux | 6,8 + ve 7 |
| SUSE Linux Enterprise Server | 11 ve 12 |
| OpenSUSE artık | 42.3 + |
| CentOS | 6.5 + ve 7 |
| CoreOS | 899.17.0+ |

CoreOS desteklenmiyor.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Bazı Ağ İzleyicisi Aracısı işlevlerini VM Internet'e bağlı olduğunu gerektirir. Giden bağlantıları kurmak için, Ağ İzleyicisi Aracısı özelliklerden bazıları arıza veya yeteneği kullanılamaz duruma gelir. Aracı gerektirir Ağ İzleyicisi işlevleri hakkında daha fazla bilgi için bkz:[Ağ İzleyicisi belgeleri](/azure/network-watcher/).

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması Ağ İzleyicisi Aracısı uzantısı gösterir. Uzantı gerektiren veya destek, herhangi bir kullanıcı tarafından sağlanan ayarı değil. Uzantı üzerinde varsayılan yapılandırması kullanır.

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
        "type": "NetworkWatcherAgentLinux",
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
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Şablon dağıtımı

Bir Azure Resource Manager şablonu ile Azure VM uzantıları dağıtabilirsiniz. Ağ İzleyicisi Aracısı uzantısı dağıtmak için önceki json şeması şablonunuzda kullanın.

## <a name="azure-cli-10-deployment"></a>Azure CLI 1.0 dağıtımı

Aşağıdaki örnek Ağ İzleyicisi Aracısı VM uzantısı Klasik dağıtım modeli aracılığıyla dağıtılan var olan bir VM dağıtır:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-20-deployment"></a>Azure CLI 2.0 dağıtımı

Aşağıdaki örnek Resource Manager aracılığıyla dağıtılan var olan bir VM Ağ İzleyicisi Aracısı VM uzantısı dağıtır:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Azure portalında veya Azure CLI kullanarak uzantısı dağıtımları durumuyla ilgili verileri alabilir.

Aşağıdaki örnek, Azure CLI 1.0 kullanarak Klasik dağıtım modeli aracılığıyla dağıtılan bir VM için uzantıları dağıtım durumunu gösterir:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

Aşağıdaki örnek Azure CLI 2.0 kullanan Resource Manager aracılığıyla dağıtılan bir VM için NetworkWatcherAgentLinux uzantısı dağıtım durumunu gösterir:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, başvurabilirsiniz [Ağ İzleyicisi belgelerine](/azure/network-watcher/), veya üzerinde Azure uzmanlar başvurun [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**. Azure desteği hakkında daha fazla bilgi için bkz: [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
