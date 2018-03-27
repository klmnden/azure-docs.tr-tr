---
title: "Azure şablonuyla Linux sanal makine ölçek kümesi oluşturma | Microsoft Docs"
description: "Örnek uygulama dağıtan ve otomatik ölçeklendirme kurallarını yöneten bir Azure Resource Manager şablonuyla hızlıca bir Linux sanal makine ölçek kümesi oluşturmayı öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: 16e9c0b30710d711ef2789f7781b17e72889d4da
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-linux-virtual-machine-scale-set-with-an-azure-template"></a>Azure şablonuyla Linux sanal makine ölçek kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu başlangıç makalesinde bir Azure Resource Manager şablonu ile bir Linux sanal makinesi ölçek kümesi oluşturacaksınız. Ölçek kümesi oluşturmak için [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md), [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) veya [Azure portalı](virtual-machine-scale-sets-create-portal.md) da kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="define-a-scale-set-in-a-template"></a>Şablonda ölçek kümesi tanımlama
Azure Resource Manager şablonları, ilgili kaynak gruplarını dağıtmanızı sağlar. Şablonlar JavaScript Nesne Gösterimi (JSON) ile yazılmıştır ve uygulamanıza ait Azure altyapısı ortamının tamamını tanımlar. Tek bir şablonda sanal makine ölçek kümesi oluşturabilir, uygulamaları yükleyebilir ve otomatik ölçeklendirme kurallarını yapılandırabilirsiniz. Değişkenleri ve parametreleri kullanarak bu şablonu var olan ölçek kümelerini güncelleştirme veya yenilerini oluşturma amacıyla tekrar kullanabilirsiniz. Şablonları Azure portalı, Azure CLI 2.0 veya Azure PowerShell aracılığıyla ya da sürekli tümleştirme/sürekli teslim (CI/CD) işlem hatlarından dağıtabilirsiniz.

Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment)

Şablonla ölçek kümesi oluşturmak için gerekli kaynakları tanımlamanız gerekir. Sanal makine ölçek kümesi kaynak türünün ana bölümleri şunlardır:

| Özellik                     | Özellik açıklaması                                  | Örnek şablon değeri                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| type                         | Oluşturulacak Azure kaynağı türü                            | Microsoft.Compute/virtualMachineScaleSets |
| ad                         | Ölçek kümesi adı                                       | myScaleSet                                |
| location                     | Ölçek kümesinin oluşturulacağı konum                     | Doğu ABD                                   |
| sku.name                     | Her bir ölçek kümesi örneği için VM boyutu                  | Standard_A1                               |
| sku.capacity                 | Başlangıçta oluşturulacak VM örneği sayısı           | 2                                         |
| upgradePolicy.mode           | Değişiklik yapıldığında kullanılacak VM örneği yükseltme modu              | Automatic                                 |
| imageReference               | VM örnekleri için kullanılacak platform veya özel görüntü | Canonical Ubuntu Server 16.04-LTS         |
| osProfile.computerNamePrefix | Her bir VM örneği için ad ön eki                     | myvmss                                    |
| osProfile.adminUsername      | Her bir VM örneği için kullanıcı adı                        | azureuser                                 |
| osProfile.adminPassword      | Her bir VM örneği için parola                        | P@ssw0rd!                                 |

 Aşağıdaki örnekte temel ölçek kümesi kaynak tanımı gösterilmektedir. Bir ölçek kümesi şablonunu özelleştirmek için VM boyutunu veya başlangıç kapasitesini değiştirebilir ya da farklı bir platform veya özel görüntü kullanabilirsiniz.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

 Örneği kısa tutmak için sanal ağ arabirim kartı (NIC) yapılandırılması gösterilmemiştir. Yük dengeleyici gibi ek bileşenler de gösterilmemiştir. Ölçek kümesi şablonunun tamamı [bu makalenin sonunda](#deploy-the-template) gösterilmiştir.


## <a name="install-an-application"></a>Uygulama yükleme
Bir ölçek kümesini dağıttığınızda VM uzantıları uygulama yükleme gibi dağıtım sonrası yapılandırma ve otomasyon görevlerini gerçekleştirebilir. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Ölçek kümenize uzantı uygulamak için önceki kaynak örneğine *extensionProfile* bölümünü eklemeniz gerekir. Uzantı profili temelde aşağıdaki özellikleri tanımlar:

- Uzantı türü
- Uzantı yayımcısı
- Uzantı sürümü
- Yapılandırma veya yükleme betiklerinin konumu
- VM örneklerinde yürütülecek komutlar

[Linux'ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) şablonu, Özel Betik Uzantısı'nı kullanarak bir Python web çerçevesi olan [Bottle](http://bottlepy.org/docs/dev/) uygulamasını ve basit bir HTTP sunucusu yükler. 

İki betik **fileUris** - *installserver.sh* ve *workserver.py* içinde tanımlanmıştır. Bu dosyalar GitHub'dan indirildikten sonra *commandToExecute*, uygulamayı yüklemek ve yapılandırmak için `bash installserver.sh` komutunu çalıştırır:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "AppInstall",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
          ],
          "commandToExecute": "bash installserver.sh"
        }
      }
    }
  ]
}
```


## <a name="deploy-the-template"></a>Şablonu dağıtma
[Linux'ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) şablonunu aşağıdaki **Azure'a dağıtın** düğmesiyle dağıtabilirsiniz. Bu düğme Azure portalını açar, şablonun tamamını yükler ve ölçek kümesi adı, örnek sayısı ve yönetici kimlik bilgileri gibi birkaç parametreyi sorar.

[![Şablonu Azure'a dağıtma](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-bottle-autoscale%2Fazuredeploy.json)

Azure CLI 2.0 ile [az group deployment create](/cli/azure/group/deployment#az_group_deployment_create) komutunu kullanarak da Python HTTP sunucusunu Linux'ta dağıtabilirsiniz:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location EastUS

# Deploy template into resource group
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/azuredeploy.json
```

VM örnekleri için ölçek kümesi adı, örnek sayısı ve yönetici kimlik bilgileri istemlerini yanıtlayın. Ölçek kümesinin ve yardımcı kaynakların oluşturulması birkaç dakika sürer.


## <a name="test-your-sample-application"></a>Örnek uygulamanızı test etme
Uygulamanızı çalışır halde görmek için [az network public-ip list](/cli/azure/network/public-ip#az_network_public_ip_show) ile yük dengeleyicisinin genel IP adresini alın:

```azurecli-interactive
az network public-ip list \
    --resource-group myResourceGroup \
    --query [*].ipAddress -o tsv
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına şu biçimde girin: *http://genelIPadresi:9000/do_work*. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![NGINX varsayılan web sayfası](media/virtual-machine-scale-sets-create-template/running-python-app.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, aşağıdaki gibi [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, ölçek kümesini tüm ilgili kaynakları kaldırabilirsiniz:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar
Bu başlangıç makalesinde Azure şablonuyla bir Linux ölçek kümesi oluşturdunuz ve Özel Betik Uzantısı'nı kullanarak VM örneklerine basit bir Python web sunucusu yüklediniz. Daha fazla ölçeklenebilirlik ve otomasyon için ölçek kümenizi aşağıdaki nasıl yapılır makaleleriyle genişletin:

- [Uygulamanızı sanal makine ölçek kümelerine dağıtma](virtual-machine-scale-sets-deploy-app.md)
- [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) veya [Azure portalı](virtual-machine-scale-sets-autoscale-portal.md) ile otomatik ölçeklendirme
- [Ölçek kümesi VM örnekleriniz için otomatik işletim sistemi yükseltmelerini kullanma](virtual-machine-scale-sets-automatic-upgrade.md)
