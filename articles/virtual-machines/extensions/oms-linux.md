---
title: Linux için Azure günlük analizi sanal makine uzantısı | Microsoft Docs
description: Günlük analizi Aracısı'nı bir sanal makine uzantısını kullanarak Linux sanal makine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/21/2018
ms.author: danis
ms.openlocfilehash: cc8b3f6a4ff6b683fc4ed2777adf6ab0b17f05be
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301494"
---
# <a name="log-analytics-virtual-machine-extension-for-linux"></a>Linux için Analytics sanal makine uzantısı oturum

## <a name="overview"></a>Genel Bakış

Günlük analizi bulut izleme, uyarma ve uyarı düzeltme özellikleri sağlar ve şirket içi varlıklar. Linux için günlük analizi Aracısı sanal makine uzantısı yayımlanan ve Microsoft tarafından desteklenmiyor. Uzantı Azure sanal makinelerde günlük analizi aracısını yükler ve sanal makineleri olan bir günlük analizi çalışma kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Linux için günlük analizi sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Günlük analizi aracı uzantısı bu Linux dağıtımları karşı çalıştırabilirsiniz.

| Dağıtım | Sürüm |
|---|---|
| CentOS Linux | 5, 6 ve 7 (x86/x64) |
| Oracle Linux | 5, 6 ve 7 (x86/x64) |
| Red Hat Enterprise Linux Server | 5, 6 ve 7 (x86/x64) |
| Debian GNU/Linux | 6, 7, 8 ve 9 (x86/x64) |
| Ubuntu | 12.04 LTS, 14.04 LTS 16.04 LTS (x86/x64) |
| SUSE Linux Enterprise Server | 11 ile 12 (x86/x64) |

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM uzantısı sürüm
Aşağıdaki tabloda günlük analizi VM uzantısı ve her bir yayın için günlük analizi Aracısı paket sürümünü bir eşleme sağlar. Günlük analizi aracı Paket sürümü için sürüm notları için bir bağlantı bulunur. Sürüm Notları hata düzeltmeleri ve verilen Aracı sürüm için yeni özelliklerin ayrıntılarını içerir.  

| Günlük analizi Linux VM uzantısı sürümü | Günlük analizi aracı Paket sürümü | 
|--------------------------------|--------------------------|
| 1.6.42.0 | [1.6.0-42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3-174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14,2 125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2-124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1-123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi otomatik olarak günlük analizi aracı sağlar ve Azure aboneliğinizde ASC tarafından oluşturulan bir varsayılan günlük analizi çalışma alanı bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması, yapılandırılmış çalışma üzerine yazar ve Azure Güvenlik Merkezi ile bağlantısını keser.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için günlük analizi aracı uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şeması Log Analytics Agent uzantısı gösterir. Uzantı çalışma alanı kimliği ve hedef günlük analizi çalışma alanından bir çalışma alanı anahtarı gerektirir; Bu değerler olabilir [günlük analizi çalışma alanında bulunan](../../log-analytics/log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) Azure portalında. Çalışma alanı anahtarı hassas verileri olarak değerlendirilmesi için bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi. Unutmayın **Workspaceıd** ve **workspaceKey** büyük küçük harfe duyarlıdır.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Yayımcı | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.6 |
| Workspaceıd (örneğin) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örneğin) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla günlük analizi için ekleme gibi dağıtım yapılandırma sonrası gerektiren sanal makineler dağıtırken idealdir. Günlük analizi aracı VM uzantısı içeren bir örnek Resource Manager şablonunu bulunabilir [Azure hızlı başlangıç Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Sanal makine uzantısı JSON yapılandırması içinde sanal makine kaynağı iç içe geçmiş veya kök veya Resource Manager JSON şablonu en üst düzeyinde yerleştirilir. JSON yapılandırma yerleşimini kaynak adı ve türü değeri etkiler. Daha fazla bilgi için bkz: [Ayarla alt kaynakları için ad ve tür](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

Aşağıdaki örnek, VM uzantısı içinde sanal makine kaynağı iç içe geçmiş varsayar. Uzantı kaynak iç içe geçirme sırasında JSON yerleştirilir `"resources": []` sanal makinenin nesnesi.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

JSON uzantısı şablon kökünde yerleştirirken, kaynak adı üst sanal makine için referans içeriyor ve iç içe geçmiş yapılandırma türü yansıtır.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.6",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI günlük analizi aracı VM uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. Değiştir *Workspaceıd* ve *workspaceKey* olanla günlük analizi çalışma alanı. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.6 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Veri uzantısı dağıtımları durumuyla ilgili Azure portalından ve Azure CLI kullanarak alınabilir. İçin belirli bir VM uzantıları dağıtım durumunu görmek için Azure CLI kullanarak şu komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıktısını aşağıdaki dosyasına kaydedilir:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 9 | Erken adlı etkinleştir | [Azure Linux Aracısı güncelleştirme](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) sürüme en son kullanılabilir. |
| 10 | VM günlük analizi çalışma alanına zaten bağlı | VM uzantısı şemasında belirtilen çalışma alanına bağlanmak için stopOnMultipleConnections genel ayarları'nda false olarak ayarlayın veya bu özelliği kaldırın. Bu VM için bağlı her çalışma alanı için bir kez fatura. |
| 11 | Uzantı için sağlanan geçersiz yapılandırma | Dağıtım için gereken tüm özellik değerlerini ayarlamak için Yukarıdaki örneklerde izleyin. |
| 12 | Dpkg Paket Yöneticisi kilitli | Tamamlandı ve yeniden deneyin tüm dpkg güncelleştirme makine üzerindeki işlemleri emin olun. |
| 17 | OMS paket yükleme hatası | 
| 19 | OMI paket yükleme hatası | 
| 20 | SCX paket yükleme hatası |
| 51 | Bu uzantı sanal makinenin işletim sistemi üzerinde desteklenmiyor | |
| 55 | Microsoft Operations Management Suite hizmetine bağlanamıyor | Sistem ya da Internet erişimi veya geçerli bir HTTP proxy sağlanmış sahip denetleyin. Ayrıca, çalışma alanı kimliği doğruluğunu denetleyin |

Ek sorun giderme bilgileri bulunabilir [Linux için OMS Aracısı sorun giderme kılavuzu](../../log-analytics/log-analytics-azure-vmext-troubleshoot.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).
