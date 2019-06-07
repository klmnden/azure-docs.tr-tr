---
title: Linux için Azure İzleyici sanal makine uzantısı | Microsoft Docs
description: Log Analytics aracısını sanal makine uzantısını kullanarak Linux sanal makinesine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/06/2019
ms.author: roiyz
ms.openlocfilehash: 8b24af016349db0fcfb4106a1e69da395e3d0150
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755145"
---
# <a name="azure-monitor-virtual-machine-extension-for-linux"></a>Linux için Azure İzleyici sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

Azure İzleyici günlüklerine, Bulut ve şirket içi varlıkları arasında izleme, uyarı ve uyarı düzeltme özellikleri sağlar. Linux için Log Analytics aracısını sanal makine uzantısı yayımlandı ve Microsoft tarafından desteklenmiyor. Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve sanal makinelerin mevcut bir Log Analytics çalışma alanına kaydeder. Bu belge, desteklenen platformlar, yapılandırmaları ve Linux için Azure İzleyici sanal makine uzantısı için dağıtım seçenekleri açıklanmaktadır.

>[!NOTE]
>Azure İzleyici devam eden Microsoft Operations Management Suite (OMS) gelen geçiş işleminin bir parçası olarak için OMS aracısını Windows veya Linux için Windows ve Log Analytics aracısını Linux için Log Analytics aracısını olarak adlandırılır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Log Analytics aracısını uzantısı bu Linux dağıtımları karşı çalıştırabilirsiniz.

| Dağıtım | Sürüm |
|---|---|
| CentOS Linux | (x86/x64) 6 ve 7 (x 64) |
| Amazon Linux | 2017.09 (x64) | 
| Oracle Linux | 6 ve 7 (x86/x64) |
| Red Hat Enterprise Linux Server | (x86/x64) 6 ve 7 (x 64) |
| Debian GNU/Linux | 8 ve 9 (x86/x64) |
| Ubuntu | 14.04 (x86/x64) LTS, 16.04 LTS (x86/x64) ve 18.04 LTS (x64) |
| SUSE Linux Enterprise Server | 12 (x 64) |

>[!NOTE]
>OpenSSL sürümünden daha düşük 1.x herhangi bir platformda desteklenmiyor ve sürüm 1.10 (64-bit) x86_64 platformlarda desteklenir.  
>

### <a name="agent-prerequisites"></a>Aracı önkoşulları

Aşağıdaki tabloda, aracının yüklü olması desteklenen Linux dağıtımları için gereken paketleri vurgulanmaktadır.

|Gerekli paket |Açıklama |En düşük sürüm |
|-----------------|------------|----------------|
|Glibc |    GNU C Kitaplığı | 2.5-12 
|openssl    | OpenSSL kitaplıkları | 1.0.x veya 1.1.x |
|Curl | cURL web istemcisi | 7.15.5 |
|Python ctypes | | 
|PAM | Eklenebilir kimlik doğrulaması modülleri | | 

>[!NOTE]
>Rsyslog veya syslog-ng, syslog iletileri toplamak için gereklidir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünde varsayılan syslog daemon'u syslog olay toplaması için desteklenmiyor. Bu dağıtımları sürümünden Syslog verilerini toplamak için rsyslog daemon'u yüklenmeli ve sysklog değiştirmek için yapılandırılmış.

### <a name="agent-and-vm-extension-version"></a>Aracı ve VM uzantısı sürümü
Aşağıdaki tabloda, her sürüm için Log Analytics aracısını paketi ve Azure İzleyicisi VM uzantısı sürümünü bir eşleme sağlar. Log Analytics aracı Paket sürümü için sürüm notları için bir bağlantı bulunur. Sürüm Notları, hata düzeltmeleri ve belirli bir aracı sürüm için yeni özellikler hakkında ayrıntılı bilgi içerir.  

| Azure İzleyici Linux VM uzantısı sürümü | Log Analytics aracısını Paket sürümü | 
|--------------------------------|--------------------------|
|1.10.0 | [1.10.0-1](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.10.0-1) |
| 1.9.1 | [1.9.0-0](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.9.0-0) |
| 1.8.11 | [1.8.1-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.8.1.256)| 
| 1.8.0 | [1.8.0-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/1.8.0-256)| 
| 1.7.9 | [1.6.1-3](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.1.3)| 
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

Azure Güvenlik Merkezi, otomatik olarak Log Analytics aracısını sağlar ve Azure aboneliğinizde ASC tarafından oluşturulan varsayılan Log Analytics çalışma alanına bağlanır. Azure Güvenlik Merkezi kullanıyorsanız, bu belgedeki adımları çalıştırmayın. Bunun yapılması, yapılandırılmış çalışma alanı üzerine yazar ve Azure Güvenlik Merkezi ile bağlantısını keser.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Log Analytics aracısını uzantısı, hedef sanal makineyi internet'e bağlı olduğundan emin gerektirir. 

## <a name="extension-schema"></a>Uzantı şeması

Log Analytics aracısını Uzantı Şeması aşağıdaki JSON'u göstermektedir. Çalışma alanı kimliği ve hedef Log Analytics çalışma alanından bir çalışma alanı anahtarı uzantısı gerektirir; Bu değerleri [Log Analytics çalışma alanınızda bulunan](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) Azure portalında. Çalışma alanı anahtarı hassas verileri olarak değerlendirilip olduğundan, bir korumalı ayarı yapılandırmasında depolanması gerekir. Azure VM uzantısının korumalı ayarı veriler şifrelenir ve yalnızca hedef sanal makinede şifresi. Unutmayın **Workspaceıd** ve **workspaceKey** büyük küçük harfe duyarlıdır.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

>[!NOTE]
>Yukarıdaki şemayı şablonunun kök düzeyinde yerleştirilir varsayar. Şablonunda, sanal makine kaynağı içine koyarsanız `type` ve `name` özellikleri değiştirilmesi, açıklandığı [birazcık daha indiğimde](#template-deployment).
>

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| Yayımcı | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| Çalışma alanı kimliği (örn.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (örn.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla Azure İzleyici günlüklerine ekleme gibi dağıtım sonrası yapılandırma gerektiren sanal makineler dağıtırken idealdir. Log Analytics aracısını VM uzantısı içeren örnek bir Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Galerisine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Sanal makine uzantısı için JSON yapılandırma içinde sanal makine kaynağı iç içe geçmiş veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yapılandırma yerleşimini etkiler. Daha fazla bilgi için [ayarlamak için alt kaynakları ad ve tür](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources). 

Aşağıdaki örnekte, VM uzantısını sanal makine kaynağı içinde iç içe varsayılır. İç içe uzantısı kaynak, JSON yerleştirildi `"resources": []` sanal makinenin nesne.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Uzantı JSON şablonu kökünde yerleştirilirken, kaynak adı üst sanal makineye bir başvuru içerir ve iç içe geçmiş yapılandırma türü yansıtır.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
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

Azure CLI, mevcut bir sanal makine için Log Analytics Aracısı VM uzantısını dağıtmak için kullanılabilir. Değiştirin *Workspaceıd* ve *workspaceKey* Log Analytics çalışma alanınızın değerlerle. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.7 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 9 | Etkinleştirme beklenenden önce çağırılır | [Azure Linux aracısını güncelleştirme](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) kullanılabilir en son sürüme için. |
| 10 | VM, Log Analytics çalışma alanınıza zaten bağlı | VM uzantısı şemasında belirtilen çalışma alanına bağlanmak için stopOnMultipleConnections genel ayarları false olarak ayarlayın veya bu özelliği kaldırın. Bu VM için bağlı her bir çalışma alanı için bir kez faturalandırılır. |
| 11 | Uzantı için sağlanan geçersiz yapılandırma | Dağıtım için gerekli tüm özellik değerlerini ayarlamak için Yukarıdaki örneklerde izleyin. |
| 17 | Günlük analizi paket yükleme hatası | 
| 19 | OMI paket yükleme hatası | 
| 20 | SCX paket yükleme hatası |
| 51 | Bu uzantı sanal makinenin işletim sistemi üzerinde desteklenmiyor | |
| 55 | Bağlantı kurulamıyor Azure İzleyici hizmeti veya gerekli paketleri eksik veya dpkg Paket Yöneticisi kilitli| Sistem ya da Internet erişimi veya geçerli bir HTTP proxy'sinin sağlanan sahip olmadığını denetleyin. Ayrıca, çalışma alanı kimliği doğruluğunu denetleyin ve curl ve tar yardımcı programları yüklü olmadığını doğrulayın. |

Ek bilgiler bulunabilir [Log Analytics aracısı için Linux sorun giderme kılavuzu](../../azure-monitor/platform/vmext-troubleshoot.md).

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
