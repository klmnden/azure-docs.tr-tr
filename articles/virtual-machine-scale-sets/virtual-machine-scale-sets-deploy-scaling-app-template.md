---
title: Azure şablonuyla Sanal Makine Ölçek Kümesi oluşturma | Microsoft Docs
description: Azure Resource Manager şablonuyla hızlıca sanal makine ölçek kümesi oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2017
ms.author: iainfou
ms.openlocfilehash: fc5c4166464ec43833bd49ac68fa52e8da8892e3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Azure CLI 2.0 ile Sanal Makine Ölçek Kümesi oluşturma
Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki VM sayısını el ile ölçeklendirebilir veya CPU, bellek isteği ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Bu başlangıç makalesinde bir Azure Resource Manager şablonu ile bir sanal makine ölçek kümesi oluşturacaksınız. Ölçek kümesi oluşturmak için [Azure CLI 2.0](quick-create-cli.md), [Azure PowerShell](quick-create-powershell.md) veya [Azure portalı](quick-create-portal.md) da kullanabilirsiniz.


## <a name="overview-of-templates"></a>Şablonlara genel bakış
Azure Resource Manager şablonları, ilgili kaynak gruplarını dağıtmanızı sağlar. Şablonlar JavaScript Nesne Gösterimi (JSON) ile yazılmıştır ve uygulamanıza ait Azure altyapısı ortamının tamamını tanımlar. Tek bir şablonda sanal makine ölçek kümesi oluşturabilir, uygulamaları yükleyebilir ve otomatik ölçeklendirme kurallarını yapılandırabilirsiniz. Değişkenleri ve parametreleri kullanarak bu şablonu var olan ölçek kümelerini güncelleştirme veya yenilerini oluşturma amacıyla tekrar kullanabilirsiniz. Şablonları Azure portalı, Azure CLI 2.0 veya Azure PowerShell aracılığıyla dağıtabilir ya da sürekli tümleştirme/sürekli teslim (CI/CD) işlem hatlarından çağırabilirsiniz.

Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment)


## <a name="define-a-scale-set"></a>Ölçek kümesi tanımlama
Şablon, her bir kaynak türü için yapılandırma tanımlar. Sanal makine ölçek kümesi kaynak türü, tek bir VM ile benzerlik gösterir. Sanal makine ölçek kümesi kaynak türünün ana bölümleri şunlardır:

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

 Aşağıdaki kod parçacığında bir şablon içindeki temel ölçek kümesi kaynak tanımı gösterilmektedir. Örneği kısa tutmak için sanal ağ arabirim kartı (NIC) yapılandırılması gösterilmemiştir. Bir ölçek kümesi şablonunu özelleştirmek için VM boyutunu veya başlangıç kapasitesini değiştirebilir ya da farklı bir platform veya özel görüntü kullanabilirsiniz.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2016-04-30-preview",
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


## <a name="install-an-application"></a>Uygulama yükleme
Bir ölçek kümesini dağıttığınızda VM uzantıları uygulama yükleme gibi dağıtım sonrası yapılandırma ve otomasyon görevlerini gerçekleştirebilir. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Ölçek kümenize uzantı uygulamak için önceki kaynak örneğine *extensionProfile* bölümünü eklemeniz gerekir. Uzantı profili temelde aşağıdaki özellikleri tanımlar:

- Uzantı türü
- Uzantı yayımcısı
- Uzantı sürümü
- Yapılandırma veya yükleme betiklerinin konumu
- VM örneklerinde yürütülecek komutlar

Uzantı ile uygulama yüklemek için kullanabileceğiniz iki farklı yönteme göz atalım: Özel Betik Uzantısı ile Linux üzerinde bir Python uygulaması yükleme veya PowerShell DSC uzantısı ile Windows üzerinde bir ASP.NET uygulaması yükleme.

### <a name="python-http-server-on-linux"></a>Linux’ta Python HTTP sunucusu
[Linux'ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale), Özel Betik Uzantısı'nı kullanarak bir Python web çerçevesi olan [Bottle](http://bottlepy.org/docs/dev/) uygulamasını ve basit bir HTTP sunucusu yükler. 

İki betik *fileUris* - *installserver.sh* ve *workserver.py* içinde tanımlanmıştır. Bu dosyalar GitHub'dan indirildikten sonra *commandToExecute*, yüklenecek ve yapılandırılacak uygulama için `bash installserver.sh` tanımlaması yapar:

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

### <a name="aspnet-application-on-windows"></a>Windows'da ASP.NET uygulaması
[ASP.NET application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) örnek şablonunda PowerShell DSC uzantısı kullanılarak IIS içinde çalışan bir ASP.NET MVC uygulaması yüklenmektedir. 

*url* ile tanımlandığı üzere GitHub'dan bir yükleme betiği indirilir. Uzantı ardından *function* ve *Script* ile tanımlandığı üzere *IISInstall.ps1* betiğinden *InstallIIS* komutunu çalıştırır. ASP.NET uygulaması da *WebDeployPackagePath* ile tanımlandığı üzere GitHub'dan indirilen bir Web Dağıtımı paketine sahiptir:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Powershell.DSC",
      "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.9",
        "autoUpgradeMinorVersion": true,
        "forceUpdateTag": "1.0",
        "settings": {
          "configuration": {
            "url": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/DSC/IISInstall.ps1.zip",
            "script": "IISInstall.ps1",
            "function": "InstallIIS"
          },
          "configurationArguments": {
            "nodeName": "localhost",
            "WebDeployPackagePath": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/WebDeploy/DefaultASPWebApp.v1.0.zip"
          }
        }
      }
    }
  ]
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma
[Linux'ta Python HTTP sunucusu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) veya [Windows'da ASP.NET MVC uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) şablonunu dağıtmanın en basit yolu, GitHub'daki benioku dosyalarında bulunan **Azure'a dağıtın** düğmesini kullanmaktır.  Örnek şablonları dağıtmak için PowerShell veya Azure CLI aracını da kullanabilirsiniz.

### <a name="azure-cli-20"></a>Azure CLI 2.0
Linux üzerine Python HTTP sunucusunu yüklemek için Azure CLI 2.0 kullanabilirsiniz:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location EastUS

# Deploy template into resource group
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/azuredeploy.json
```

Uygulamanızı çalışır halde görmek için [az network public-ip list](/cli/azure/network/public-ip#az_network_public_ip_show) ile yük dengeleyicisinin genel IP adresini alın:

```azurecli-interactive
az network public-ip list \
    --resource-group myResourceGroup \
    --query [*].ipAddress -o tsv
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına şu biçimde girin: *http://<publicIpAddress>:9000/do_work*. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![NGINX varsayılan web sayfası](media/virtual-machine-scale-sets-create-template/running-python-app.png)


### <a name="azure-powershell"></a>Azure PowerShell
Windows üzerine ASP.NET uygulamasını yüklemek için Azure PowerShell kullanabilirsiniz:

```azurepowershell-interactive
# Create a resource group
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS

# Deploy template into resource group
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName myResourceGroup `
    -TemplateFile https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/azuredeploy.json
```

Uygulamanızı çalışır halde görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile yük dengeleyicinizin genel IP adresini alın:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına şu biçimde girin: *http://<publicIpAddress>/Uygulamam*. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![Çalışan IIS sitesi](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse, aşağıdaki gibi [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, ölçek kümesini tüm ilgili kaynakları kaldırabilirsiniz:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar
