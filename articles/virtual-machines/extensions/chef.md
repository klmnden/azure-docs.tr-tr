---
title: Azure Vm'leri için Chef uzantısı | Microsoft Docs
description: Chef istemci Chef VM uzantısı kullanarak bir sanal makine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: roiyz
ms.openlocfilehash: 6bd3ea4e664523fe8014be40c51d573ed5158ecf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60800272"
---
# <a name="chef-vm-extension-for-linux-and-windows"></a>Linux ve Windows için Chef VM uzantısı

Chef Software, Linux ve Windows için fiziksel ve sanal sunucu yapılandırmalarının yönetilmesine olanak sağlayan bir DevOps otomasyon platformu sunar. Chef VM uzantısı, sanal makineler üzerinde Chef sağlayan bir uzantısıdır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="operating-system"></a>İşletim sistemi

Tüm Chef VM uzantısı desteklenen [uzantısı desteklenen işletim sistemlerine](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems) azure'da.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Chef VM uzantısı, hedef sanal makine Chef istemci yükü content delivery Network'te (CDN) almak için internet'e bağlı gerektirir.  

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON şema için Chef VM uzantısı gösterir. Uzantı, Chef sunucusu için en az Chef sunucu URL'si, doğrulama istemci adını ve doğrulama anahtarı gerektirir; Bu değerleri bulunabilir `knife.rb` başlangıç-kit.zip yüklediğinizde, indirilen dosyada [Chef Automate](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate) veya tek başına [Chef sunucusu](https://downloads.chef.io/chef-server). Doğrulama anahtarı hassas verileri olarak değerlendirilip olduğundan, altında yapılandırılmalıdır **protectedSettings** öğesi, hedef sanal makinede yalnızca şifresi çözülür anlamına gelir.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'),'/', parameters('chef_vm_extension_type'))]",
  "apiVersion": "2017-12-01",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Chef.Bootstrap.WindowsAzure",
    "type": "[parameters('chef_vm_extension_type')]",
    "typeHandlerVersion": "1210.12",
    "settings": {
      "bootstrap_options": {
        "chef_server_url": "[parameters('chef_server_url')]",
        "validation_client_name": "[parameters('chef_validation_client_name')]"
      },
      "runlist": "[parameters('chef_runlist')]"
    },
    "protectedSettings": {
      "validation_key": "[parameters('chef_validation_key')]"
    }
  }
}  
```

### <a name="core-property-values"></a>Çekirdek özellik değerleri

| Ad | Değer / örnek | Veri Türü
| ---- | ---- | ---- 
| apiVersion | `2017-12-01` | dize (tarih) |
| Yayımcı | `Chef.Bootstrap.WindowsAzure` | string |
| type | `LinuxChefClient` (Linux), `ChefClient` (Windows) | string |
| typeHandlerVersion | `1210.12` | dize (çift) |

### <a name="settings"></a>Ayarlar

| Ad | Değer / örnek | Veri Türü | Gerekli mi?
| ---- | ---- | ---- | ----
| settings/bootstrap_options/chef_server_url | `https://api.chef.io/organizations/myorg` | dize (url) | E |
| settings/bootstrap_options/validation_client_name | `myorg-validator` | string | E |
| ayarlar/çalışma | `recipe[mycookbook::default]` | string | E |

### <a name="protected-settings"></a>Korumalı ayarları

| Ad | Örnek | Veri Türü | Gerekli mi?
| ---- | ---- | ---- | ---- |
| protectedSettings/validation_key | `-----BEGIN RSA PRIVATE KEY-----\nKEYDATA\n-----END RSA PRIVATE KEY-----` | string | E |

<!--
### Linux-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |

### Windows-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |
-->

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Şablonları, bir veya daha fazla sanal makine dağıtın, Chef istemciyi yüklemek, Chef sunucu ve gerçekleştirme tarafından tanımlandığı gibi ilk yapılandırma sunucusuna bağlanmak için kullanılabilir [çalıştırma listesi](https://docs.chef.io/run_lists.html)

Chef VM uzantısı içeren örnek bir Resource Manager şablonu bulunabilir [Azure hızlı başlangıç Galerisine](https://github.com/Azure/azure-quickstart-templates/tree/master/chef-json-parameters-linux-vm).

Sanal makine uzantısı için JSON yapılandırma içinde sanal makine kaynağı iç içe geçmiş veya kök veya bir Resource Manager JSON şablonunu üst düzey yerleştirilir. Kaynak adı ve türü değeri JSON yapılandırma yerleşimini etkiler. Daha fazla bilgi için [ayarlamak için alt kaynakları ad ve tür](../../azure-resource-manager/resource-manager-template-child-resource.md).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLI, Chef VM uzantısı için mevcut bir VM'yi dağıtmak için kullanılabilir. Değiştirin **validation_key** doğrulama anahtarınızı içeriğiyle (Bu dosyanın şu şekilde bir `.pem` uzantısı).  Değiştirin **validation_client_name**, **chef_server_url** ve **run_list** bu değerlerle `knife.rb` dosyasında, başlangıç Seti.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myExistingVM \
  --name LinuxChefClient \
  --publisher Chef.Bootstrap.WindowsAzure \
  --version 1210.12 --protected-settings '{"validation_key": "<validation_key>"}' \
  --settings '{ "bootstrap_options": { "chef_server_url": "<chef_server_url>", "validation_client_name": "<validation_client_name>" }, "runlist": "<run_list>" }'
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

Uzantı dağıtım durumuyla ilgili veriler, Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM'nin için uzantıları dağıtım durumunu görmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myExistingVM -o table
```

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

### <a name="linux"></a>Linux

```bash
/var/lib/waagent/Chef.Bootstrap.WindowsAzure.LinuxChefClient
```

### <a name="windows"></a>Windows

```powershell
C:\Packages\Plugins\Chef.Bootstrap.WindowsAzure.ChefClient\
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası eylemi |
| :---: | --- | --- |
| 51 | Bu uzantı sanal makinenin işletim sisteminde desteklenmiyor | |

Ek bilgiler bulunabilir [Chef VM uzantısı Benioku](https://github.com/chef-partners/azure-chef-extension).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).
