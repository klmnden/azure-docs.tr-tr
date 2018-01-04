---
title: "Linux için OMS Azure sanal makine uzantısı | Microsoft Docs"
description: "OMS Aracısı'nı bir sanal makine uzantısını kullanarak Linux sanal makine dağıtın."
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
<<<<<<< HEAD
ms.openlocfilehash: dcb7a777c66200c5046a6ad34dc4ff5d346f13e0
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
=======
ms.openlocfilehash: 8aa29dfb46a1aafb9e7b713456e1006af423a2b2
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>Linux için OMS sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

Operations Management Suite (OMS) bulut izleme, uyarma ve uyarı düzeltme özellikleri sağlar ve şirket içi varlıklar. Linux için OMS Aracısı sanal makine uzantısı yayımlanan ve Microsoft tarafından desteklenmiyor. Uzantı Azure sanal makinelerde OMS Aracısı'nı yükler ve sanal makineleri olan bir OMS çalışma kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Linux için OMS sanal makine uzantısı için dağıtım seçeneklerini ayrıntıları.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

OMS Aracısı uzantısı bu Linux dağıtımları karşı çalıştırabilirsiniz.

| Dağıtım | Sürüm |
|---|---|
| CentOS Linux | 5, 6 ve 7 |
| Oracle Linux | 5, 6 ve 7 |
| Red Hat Enterprise Linux Server | 5, 6 ve 7 |
| Debian GNU/Linux | 6, 7 ve 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 ve 12 |

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi otomatik olarak OMS aracısı sağlar ve Azure aboneliğinin varsayılan günlük analizi çalışma bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması yapılandırılmış çalışma ve bağlantıyı kesme Azure Güvenlik Merkezi ile üzerine yazar.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için OMS aracısının uzantısı hedef sanal makine internet'e bağlı olduğunu gerektirir. 

## <a name="extension-schema"></a>Uzantı Şeması

Aşağıdaki JSON şeması OMS Aracısı uzantısı gösterir. Uzantı çalışma alanı kimliği ve hedef OMS çalışma alanından bir çalışma alanı anahtarı gerektirir; Bu değerleri OMS Portalı'nda bulunabilir. Çalışma alanı anahtarı hassas verileri olarak değerlendirilmesi için bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veri şifrelenir ve yalnızca hedef sanal makineye şifresi. Unutmayın **Workspaceıd** ve **workspaceKey** büyük küçük harfe duyarlıdır.

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
    "typeHandlerVersion": "1.4",
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
| typeHandlerVersion | 1.4 |
| Workspaceıd (örneğin) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örneğin) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla OMS ekleme gibi dağıtım yapılandırma sonrası gerektiren sanal makineler dağıtırken idealdir. OMS Aracısı VM uzantısı içeren bir örnek Resource Manager şablonunu bulunabilir [Azure hızlı başlangıç Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Sanal makine uzantısı JSON yapılandırması içinde sanal makine kaynağı iç içe geçmiş veya kök veya Resource Manager JSON şablonu en üst düzeyinde yerleştirilir. JSON yapılandırma yerleşimini kaynak adı ve türü değeri etkiler. Daha fazla bilgi için bkz: [Ayarla alt kaynakları için ad ve tür](../../azure-resource-manager/resource-manager-templates-resources.md#child-resources). 

Aşağıdaki örnek, OMS uzantısı içinde sanal makine kaynağı iç içe geçmiş varsayar. Uzantı kaynak iç içe geçirme sırasında JSON yerleştirilir `"resources": []` sanal makinenin nesnesi.

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
    "typeHandlerVersion": "1.4",
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
    "typeHandlerVersion": "1.4",
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

Azure CLI OMS Aracısı VM uzantısı olan bir sanal makineyi dağıtmak için kullanılabilir. OMS anahtarı ve OMS kimliği bu OMS çalışma alanınız ile değiştirin. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
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

| Hata kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 10 | VM zaten bir OMS çalışma alanına bağlı | VM uzantısı şemasında belirtilen çalışma alanına bağlanmak için stopOnMultipleConnections genel ayarları'nda false olarak ayarlayın veya bu özelliği kaldırın. Bu VM için bağlı her çalışma alanı için bir kez fatura. |
| 11 | Uzantı için sağlanan geçersiz yapılandırma | Dağıtım için gereken tüm özellik değerlerini ayarlamak için Yukarıdaki örneklerde izleyin. |
| 12 | Dpkg Paket Yöneticisi kilitli | Tamamlandı ve yeniden deneyin tüm dpkg güncelleştirme makine üzerindeki işlemleri emin olun. |
| 20 | Erken adlı etkinleştir | [Azure Linux Aracısı güncelleştirme](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) sürüme en son kullanılabilir. |
| 51 | Bu uzantı sanal makinenin işletim sistemi üzerinde desteklenmiyor | |
| 55 | Microsoft Operations Management Suite hizmetine bağlanamıyor | Sistem ya da Internet erişimi veya geçerli bir HTTP proxy sağlanmış sahip denetleyin. Ayrıca, çalışma alanı kimliği doğruluğunu denetleyin |

Ek sorun giderme bilgileri bulunabilir [Linux için OMS Aracısı sorun giderme kılavuzu](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/en-us/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/en-us/support/options/) ve Get destek seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/en-us/support/faq/).
