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
ms.openlocfilehash: 5a33f183470ec3879344f0cfe335bab38f9ff30f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Linux için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi](/azure/network-watcher/) Azure ağları için izleme sağlayan bir ağ performans izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi Aracısı sanal makine uzantısı, Azure sanal makinelerde Ağ İzleyicisi özelliklerinden bazıları için bir gereksinimdir. Bu, isteğe bağlı ve diğer gelişmiş işlevler üzerindeki ağ trafiğini yakalama içerir.

Bu belge desteklenen platformlar ve Linux için Ağ İzleyicisi Aracısı sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları. Aracı yüklemesini değil kesintiye veya sanal makinenin yeniden başlatılması gerekir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Ağ İzleyicisi Aracısı uzantısı bu Linux dağıtımları karşı çalıştırabilirsiniz:

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

CoreOS şu anda desteklenmediğini unutmayın.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Bazı Ağ İzleyicisi Aracısı işlevlerini hedef sanal makine Internet'e bağlı olması gerekir. Giden bağlantıları kurmak için özelliği olmadan Ağ İzleyicisi Aracısı özelliklerden bazıları arıza veya kullanılamıyor. Daha fazla ayrıntı için lütfen bkz. [Ağ İzleyicisi belgeleri](/azure/network-watcher/).

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması Ağ İzleyicisi Aracısı uzantısı gösterir. Uzantı ne gerektirir ya da şu anda herhangi bir kullanıcı tarafından sağlanan ayarını destekler ve kendi varsayılan yapılandırmasına dayanır.

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

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Önceki bölümde ayrıntılı JSON şeması bir Azure Resource Manager şablonunda bir Azure Resource Manager şablon dağıtımı sırasında Ağ İzleyicisi Aracısı uzantısı çalıştırmak için kullanılabilir.

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI Ağ İzleyicisi Aracısı VM uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure CLI kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure CLI kullanarak şu komutu çalıştırın.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, Ağ İzleyicisi belgelere bakın veya üzerinde Azure uzmanlar başvurun [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
