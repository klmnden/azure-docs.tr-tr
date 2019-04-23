---
title: Linux için Azure Ağ İzleyicisi Aracısı sanal makine uzantısı | Microsoft Docs
description: Ağ İzleyicisi Aracısı bir sanal makine uzantısını kullanarak Linux sanal makinesine dağıtın.
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
ms.openlocfilehash: 5ed5e791cd6e611218769650115c78afd1869f67
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798785"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Linux için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](/azure/network-watcher/) Azure ağları için izleme sağlayan bir ağ performansı izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi Aracısı sanal makine (VM) uzantısını Azure Vm'leri üzerinde Ağ İzleyicisi özelliklerinden bazıları için isteğe bağlı ve diğer gelişmiş işlevler üzerindeki ağ trafiğini yakalama gibi zorunludur.

Bu makalede, Linux için Ağ İzleyicisi Aracısı VM uzantısı için dağıtım seçenekleri ve desteklenen platformlar ayrıntıları. Aracı yüklemesini değil kesintiye veya sanal makinenin yeniden başlatılmasını gerektirir. Dağıttığınız sanal makinelerine uzantısı dağıtabilirsiniz. Bir Azure hizmeti tarafından dağıtılan sanal makine, sanal makine uzantıları yükleme izin olup olmadığını belirlemek üzere hizmeti belgelerini denetleyin.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Ağ İzleyicisi Aracısı uzantısı aşağıdaki Linux dağıtımları için yapılandırılabilir:

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 12+ |
| Debian | 7 ve 8 |
| Red Hat | 6 ve 7 |
| Oracle Linux | 6,8 + ve 7 |
| SUSE Linux Enterprise Server | 11 ve 12 |
| OpenSUSE artık | 42.3+ |
| CentOS | 6.5 + ve 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>İnternet bağlantısı

Bazı Ağ İzleyicisi Aracısı işlevlerini bir VM, Internet'e bağlı olduğundan emin gerektirir. Giden bağlantı özelliği, Ağ İzleyicisi Aracısı özelliklerinden bazıları düzgün veya kullanılamıyor. Aracı gerektiren Ağ İzleyicisi işlevleri hakkında daha fazla bilgi için bkz.[Ağ İzleyicisi belgeleri](/azure/network-watcher/).

## <a name="extension-schema"></a>Uzantı şeması

Ağ İzleyicisi Aracısı Uzantı Şeması aşağıdaki JSON'u göstermektedir. Uzantı gerektiren veya desteği, kullanıcı tarafından sağlanan herhangi bir ayarı değil. Uzantı varsayılan yapılandırmasıyla dayalıdır.

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

Azure VM uzantıları Azure Resource Manager şablonu ile dağıtabilirsiniz. Ağ İzleyicisi Aracısı uzantısını dağıtmak için önceki json şeması şablonunuzda kullanın.

## <a name="azure-classic-cli-deployment"></a>Azure Klasik CLI dağıtım

Aşağıdaki örnek, Ağ İzleyicisi Aracısı VM uzantısını Klasik dağıtım modeliyle dağıtılan var olan bir sanal makineye dağıtır:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Aşağıdaki örnek Resource Manager üzerinden dağıtılan var olan bir sanal makineye bir Ağ İzleyicisi Aracısı VM uzantısı dağıtır:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Azure portal veya Azure CLI kullanarak uzantı dağıtımları durumuyla ilgili veri alabilir.

Aşağıdaki örnek, Klasik Azure CLI kullanarak Klasik dağıtım modeli üzerinden dağıtılmış bir VM için uzantıları dağıtım durumunu gösterir:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

```
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
```

Aşağıdaki örnekte Azure CLI kullanarak Resource Manager üzerinden dağıtılan bir sanal makine için NetworkWatcherAgentLinux uzantısı dağıtım durumunu gösterir:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa başvurabilirsiniz [Ağ İzleyicisi belgeleri](/azure/network-watcher/), ya da Azure uzmanlarından ulaşın [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **Destek**. Azure desteği hakkında daha fazla bilgi için bkz: [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
