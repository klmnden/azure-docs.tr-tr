---
title: "Kullanılabilirlik bölgeleri (Önizleme) kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs"
description: "Kesintilere karşı artıklığı için kullanılabilirlik bölgeleri kullanmak Azure sanal makine ölçek kümeleri oluşturma hakkında bilgi edinin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: iainfou
ms.openlocfilehash: 2de214f604469025a8a4accde44359fea0ded7e9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Kullanılabilirlik bölgeleri (Önizleme) kullanan bir sanal makine ölçek kümesi oluşturma
Sanal makine ölçek kümeleri datacenter düzeyi arızasına karşı korumak için kullanılabilirlik dilimlerinde ayarlamak ölçek oluşturabilirsiniz. Kullanılabilirlik bölgeleri destekleyen azure bölgeleri sahip en az üç ayrı bölgeler, her biri kendi bağımsız güç kaynağı, ağ ve soğutma. Daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](../availability-zones/az-overview.md).

[!INCLUDE [availability-zones-preview-statement.md](../../includes/availability-zones-preview-statement.md)]


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Tek bölge ve bölge olarak yedekli ölçek kümeleri
Bir sanal makine ölçek kümesi dağıttığınızda, tek bir kullanılabilirlik bölge bir bölgede ya da birden fazla bölge kullanmayı seçebilirsiniz.

Tek bir bölge, bölge bu tüm VM örnekleri çalıştırmak ve ölçek kümesini yönetilen kontrol ve yalnızca o bölge içinde autoscales kümesindeki bir ölçek oluşturduğunuzda. Bölge olarak yedekli ölçek birden çok bölgelere yayılan bir tek ölçek kümesi oluşturmanıza olanak tanır. VM örnekleri oluşturuldukça, varsayılan olarak bunlar eşit dilimlerinde dengeli. Bir kesinti bölgelerinden biri gerçekleşeceğini, Ölçek kümesini otomatik olarak kullanıma kapasitesini artırmak için ölçeklenmez. CPU veya bellek kullanımına dayalı otomatik ölçeklendirme kurallarını yapılandırmak için en iyi uygulama olacaktır. Otomatik ölçeklendirme kurallarını ölçeği bir bölge VM örnekleri kaybı kalan işletimsel bölgelerde yeni örnekleri ölçeğini yanıt verecek şekilde ayarla olanak tanır.

Kullanılabilirlik bölgeleri kullanmak için ölçek kümesini oluşturulmalıdır bir [Azure bölgesi desteklenen](../availability-zones/az-overview.md#regions-that-support-availability-zones). Ayrıca gerek [kullanılabilirlik bölgeleri Önizleme için kaydetme](http://aka.ms/azenroll). Aşağıdaki yöntemlerden biriyle kullanılabilirlik bölgeleri kullanan bir ölçek kümesi oluşturabilirsiniz:

- [Azure portalı](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Azure portalı kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-portal.md). Bilgisayarınızda yüklü olduğundan emin olun [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll). Desteklenen bir Azure bölgesini seçtiğinizde, aşağıdaki örnekte gösterildiği gibi bir ölçeği kullanılabilen bölgeler, birini Ayarla oluşturabilirsiniz:

![Ölçeği tek bir kullanılabilirlik bölgesinde Ayarla oluşturma](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Ölçek kümesi ve Azure yük dengeleyici ve genel IP adresi gibi kaynakları destekleyen belirttiğiniz tek bir bölge içinde oluşturulur.


## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-cli.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ölçek kümesini oluşturmak ve gerekir sahip [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll).

Ekleme `--zones` parametresi [az vmss oluşturmak](/cli/azure/vmss#az_vmss_create) komut ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*). Aşağıdaki örnek, bir tek bölge ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* bölgesinde *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```
Tek bölge ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek CLI betik](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh.)

### <a name="zone-redundant-scale-set"></a>Bölge olarak yedekli ölçek kümesi
Bölge olarak yedekli ölçek oluşturmak için kullandığınız bir *standart* SKU ortak IP adresi ve yük dengeleyici. Gelişmiş artıklığı *standart* SKU bölge olarak yedekli ağ kaynaklarına oluşturur. Daha fazla bilgi için bkz: [Azure yük dengeleyici standart genel bakış](../load-balancer/load-balancer-standard-overview.md). Bölge olarak yedekli ölçek oluşturduğunuz ilk kez ayarlayın veya yük dengeleyici, hesabınız bu Önizleme özellikleri için kaydetmek için aşağıdaki adımları tamamlamanız gerekir.

1. Hesabınız için bölge olarak yedekli ölçek kümesi ve ağ özellikleri ile kayıt [az özelliği kayıt](/cli/azure/feature#az_feature_register) gibi:

    ```azurecli
    az feature register --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. Özellikleri kaydetmek için birkaç dakika sürebilir. İşlemi durumunu denetleyebilirsiniz [az özelliği Göster](/cli/azure/feature#az_feature_show):

    ```azurecli
    az feature show --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```

    Aşağıdaki örnek, bir özellik olarak istenen durumunu gösterir. *kayıtlı*:
    
    ```json
    "properties": {
          "state": "Registered"
       },
    ```

3. Bölge olarak yedekli ölçeği hem ayarlamak ve ağ kaynakları olarak rapor *kayıtlı*, yeniden kaydettirin *işlem* ve *ağ* sağlayıcıları ile [az Sağlayıcı kaydı](/cli/azure/provider#az_provider_register) gibi:

    ```azurecli
    az provider register --namespace Microsoft.Compute
    az provider register --namespace Microsoft.Network
    ```

Bölge olarak yedekli ölçek kümesi oluşturmak için birden çok bölgede belirtin `--zones` parametresi. Aşağıdaki örnek, bir bölge olarak yedekli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* bölgeler arasında *1,2,3*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones {1,2,3}
```

Oluşturun ve tüm belirttiğiniz bölgelerini kaynaklar ve VM ölçek kümesi yapılandırmak için birkaç dakika sürer. Bölge olarak yedekli ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek CLI betik](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)


## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](virtual-machine-scale-sets-create-powershell.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ölçek kümesini oluşturmak ve gerekir sahip [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll). Ekleme `-Zone` parametresi [yeni AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) komut ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*). 

Aşağıdaki örnek, bir tek bölge ölçek kümesi config adlı oluşturur *vmssConfig* içinde *Doğu ABD 2* bölge *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Tek bölge ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek PowerShell komut dosyası](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1)

### <a name="zone-redundant-scale-set"></a>Bölge olarak yedekli ölçek kümesi
Bölge olarak yedekli ölçek oluşturmak için kullandığınız bir *standart* SKU ortak IP adresi ve yük dengeleyici. Gelişmiş artıklığı *standart* SKU bölge olarak yedekli ağ kaynaklarına oluşturur. Daha fazla bilgi için bkz: [Azure yük dengeleyici standart genel bakış](../load-balancer/load-balancer-standard-overview.md). Bölge olarak yedekli ölçek oluşturduğunuz ilk kez ayarlayın veya yük dengeleyici, hesabınız bu Önizleme özellikleri için kaydetmek için aşağıdaki adımları tamamlamanız gerekir.

1. Hesabınız için bölge olarak yedekli ölçek kümesi ve ağ özellikleri ile kayıt [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) gibi:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. Özellikleri kaydetmek için birkaç dakika sürebilir. İşlemi durumunu denetleyebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature):

    ```powershell
    Get-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute 
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Aşağıdaki örnek, bir özellik olarak istenen durumunu gösterir. *kayıtlı*:
    
    ```powershell
    RegistrationState
    -----------------
    Registered
    ```

3. Bölge olarak yedekli ölçeği hem ayarlamak ve ağ kaynakları olarak rapor *kayıtlı*, yeniden kaydettirin *işlem* ve *ağ* sağlayıcıları ile [ Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) gibi:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```

Bölge olarak yedekli ölçek kümesi oluşturmak için birden çok bölgede belirtin `-Zone` parametresi. Aşağıdaki örnek adlı bir bölge olarak yedekli ölçek kümesi config oluşturur *myScaleSet* arasında *Doğu ABD 2* bölgeleri *1, 2, 3*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1", "2", "3"
```

Bir ortak IP adresiyle oluşturursanız [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) ya da bir yük dengeleyici ile [yeni AzureRmLoadBalancer](/powershell/module/AzureRM.Network/New-AzureRmLoadBalancer), belirtin *- SKU "Standard"* oluşturmak için bölge olarak yedekli ağ kaynakları. Ayrıca, bir ağ güvenlik grubu ve kuralları tüm trafiğe izin verecek şekilde oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure yük dengeleyici standart genel bakış](../load-balancer/load-balancer-standard-overview.md).

Bölge olarak yedekli ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek PowerShell komut dosyası](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1)


## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma
Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturma işlemi için alma başlatılan makalesinde ayrıntılı olarak aynıdır [Linux](virtual-machine-scale-sets-create-template-linux.md) veya [Windows](virtual-machine-scale-sets-create-template-windows.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ölçek kümesini oluşturmak ve gerekir sahip [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll). Ekleme `zones` özelliğine *Microsoft.Compute/virtualMachineScaleSets* kaynak türü şablonunuzda ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*).

Aşağıdaki örnek, bir Linux tek bölge ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *Doğu ABD 2* bölge *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
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

Tek bölge ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json)

### <a name="zone-redundant-scale-set"></a>Bölge olarak yedekli ölçek kümesi
Bölge olarak yedekli ölçek kümesi oluşturmak için birden fazla değer belirtin `zones` özelliği için *Microsoft.Compute/virtualMachineScaleSets* kaynak türü. Aşağıdaki örnek, bir bölge olarak yedekli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* arasında *Doğu ABD 2* bölgeleri *1,2,3*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": [
        "1",
        "2",
        "3"
      ]
}
```

Bir ortak IP adresi veya bir yük dengeleyici oluşturursanız belirtmeniz *"sku": {"name": "Standart"} "* bölge olarak yedekli ağ kaynaklarına oluşturmak için özellik. Ayrıca, bir ağ güvenlik grubu ve kuralları tüm trafiğe izin verecek şekilde oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure yük dengeleyici standart genel bakış](../load-balancer/load-balancer-standard-overview.md).

Bölge olarak yedekli ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json)


## <a name="next-steps"></a>Sonraki adımlar
Ölçeği bir kullanılabilirlik bölgesinde Ayarla oluşturduğunuza göre öğrenebilirsiniz nasıl [dağıtma uygulamaları sanal makineye ölçek kümeleri](virtual-machine-scale-sets-deploy-app.md) veya [ile sanal makine ölçek kümeleri otomatik ölçeklendirme kullanmak](virtual-machine-scale-sets-autoscale-overview.md).
