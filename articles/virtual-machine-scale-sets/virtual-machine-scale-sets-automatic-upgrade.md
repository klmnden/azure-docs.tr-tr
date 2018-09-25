---
title: Otomatik işletim sistemi yükseltmeleri ile Azure sanal makine ölçek kümeleri | Microsoft Docs
description: İşletim sisteminde bir ölçek kümesindeki sanal makine örneklerine otomatik olarak yükseltme hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 935b3ff0fe03984b02dc2e1137f48e53b06ce0c2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995118"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Otomatik işletim sistemi yükseltmelerini Azure sanal makine ölçek kümesi

Otomatik işletim sistemi görüntüsü yükseltme tüm sanal makineler en son işletim sistemi görüntüsünü otomatik olarak yükselten bir önizleme özelliği Azure sanal makine ölçek kümeleri için ' dir.

Otomatik işletim sistemi yükseltmesi, aşağıdaki özelliklere sahiptir:

- Yapılandırıldıktan sonra görüntü yayımcılar tarafından yayımlanan en son işletim sistemi görüntüsü ölçek kullanıcı müdahalesi olmadan otomatik olarak uygulanır.
- Toplu örneklerinin sıralı bir şekilde her saat yeni bir platform görüntüsü yayımcı tarafından yayımlanan yükseltir.
- Uygulama durum araştırması ile tümleşir (isteğe bağlı, ancak yüksek düzeyde güvenlik için önerilen).
- Tüm VM boyutları için çalışır.
- Çalıştığı için Windows ve Linux platform görüntüleri.
- Otomatik yükseltmeler dışında (işletim sistemi yükseltmelerini el ile yanı başlatılabilir) herhangi bir zamanda seçebilirsiniz.
- Bir VM'nin işletim sistemi diski, en son görüntü sürümü ile oluşturulan yeni işletim sistemi diski ile değiştirilir. Kalıcı veri diskleri korunur yapılandırılmış uzantıları ve özel veri betikler çalıştırılır.


## <a name="preview-notes"></a>Önizleme notları 
Önizleme sırasında aşağıdaki sınırlamalar ve kısıtlamalar geçerlidir:

- Otomatik işletim sistemi yükseltmeleri yalnızca Destek [dört işletim sistemi'SKU ' ları](#supported-os-images). SLA'sı veya garantisi yoktur. Önizleme süresince üretim kritik iş yükleri üzerinde otomatik yükseltmeler kullanmayın öneririz.
- Azure disk şifrelemesi **değil** sanal makine ölçek kümesinin otomatik işletim sistemi yükseltmesi ile şu anda desteklenmiyor.


## <a name="register-to-use-automatic-os-upgrade"></a>Otomatik işletim sistemi yükseltme kullanmak üzere kaydolmanız
Otomatik işletim sistemi yükseltme özelliği kullanmak için Azure Powershell veya Azure CLI 2.0 ile Önizleme sağlayıcısını kaydedin.

### <a name="powershell"></a>PowerShell

1. Kaydolmalı [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. Rapor kayıt durumu için yaklaşık 10 dakika sürer *kayıtlı*. Geçerli kayıt durumu ile kontrol edebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). 

3. Kaydedildikten sonra onaylayın *Microsoft.Compute* sağlayıcı kayıtlı. Aşağıdaki örnek, Azure Powershell ile kullanır [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>CLI 2.0

1. Kaydolmalı [az özelliği kayıt](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. Rapor kayıt durumu için yaklaşık 10 dakika sürer *kayıtlı*. Geçerli kayıt durumu ile kontrol edebilirsiniz [az özelliği show](/cli/azure/feature#az-feature-show). 
 
3. Kaydedildikten sonra emin *Microsoft.Compute* sağlayıcı kayıtlı. Aşağıdaki örnek, Azure CLI'yı kullanır (2.0.20 veya sonraki) ile [az provider register](/cli/azure/provider#az-provider-register):

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> Service Fabric kümeleri kendi uygulama durumunu kavramı vardır, ancak Service Fabric olmadan ölçek kümeleri, uygulama durumunu izlemek için yük dengeleyici durum araştırması kullanır. 
>
> ### <a name="azure-powershell"></a>Azure PowerShell
>
> 1. İle sistem durumu araştırmaları için sağlayıcı özelliğini Kaydet [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. Yeniden raporlanacak kayıt durumu için yaklaşık 10 dakika sürer *kayıtlı*. Geçerli kayıt durumu ile kontrol edebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature)
>
> 3. Bir kez kayıtlı olduğundan emin olun *Microsoft.Network* sağlayıcısı kullanarak kayıtlı [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>CLI 2.0
>
> 1. İle sistem durumu araştırmaları için sağlayıcı özelliğini Kaydet [az özelliği kayıt](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. Yeniden raporlanacak kayıt durumu için yaklaşık 10 dakika sürer *kayıtlı*. Geçerli kayıt durumu ile kontrol edebilirsiniz [az özelliği show](/cli/azure/feature#az-feature-show). 
>
> 3. Bir kez kayıtlı olduğundan emin olun *Microsoft.Network* sağlayıcısı kullanarak kayıtlı [az provider register](/cli/azure/provider#az-provider-register) gibi:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Portal deneyimi
Yukarıdaki kayıt adımları izledikten sonra gidebilirsiniz [Azure portalında](https://aka.ms/managed-compute) , Ölçek kümelerinde otomatik işletim sistemi yükseltmelerini etkinleştirmek ve yükseltmeleri ilerlemesini görmek için:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Yalnızca belirli işletim sistemi platform görüntüleri şu anda desteklenmiyor. Kendiniz oluşturduğunuz özel görüntüler şu anda kullanamazsınız. *Sürüm* platform görüntüsü özelliği ayarlanmalıdır *son*.

Aşağıdaki SKU'ları şu anda desteklenen (daha fazla bilgi eklenir):
    
| Yayımcı               | Sunduğu         |  Sku               | Sürüm  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter    | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2016 Datacenter Smalldisk | en son   |
| MicrosoftWindowsServer  | WindowsServer | Kapsayıcılar ile 2016 Datacenter | en son   |



## <a name="application-health-without-service-fabric"></a>Uygulama durumunu Service Fabric olmadan
> [!NOTE]
> Bu bölüm, yalnızca Service Fabric olmadan ölçek kümeleri için geçerlidir. Service Fabric uygulama durumunu kendi kavramı vardır. Otomatik işletim sistemi yükseltmelerini Service Fabric ile kullanırken, yeni işletim sistemi görüntüsü güncelleştirme etki alanı güncelleştirme Service Fabric'te çalışan hizmetler yüksek kullanılabilirliğini sürdürmek için etki alanı tarafından kullanıma alınır. Service Fabric kümeleri dayanıklılık özellikleri hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

İşletim sistemi yükseltme sırasında bir ölçek kümesindeki sanal makine örnekleri yükseltilir aynı anda tek bir toplu. Yükseltme işlemi, yalnızca müşteri Uygulamayı yükseltilmiş kullanarak VM örneklerine sağlıklı olup olmadığını devam etmelidir. Bu nedenle, uygulama, Ölçek kümesinin işletim sistemi yükseltme altyapısı durum sinyallerini sağlamanız gerekir. VM güç durumunu ve uzantı sağlama durumu, bir VM örneği yükseltme sonrasında iyi durumda olup olmadığını belirlemek için platform işletim sistemi yükseltmeleri sırasında dikkate alır. Bir sanal makine örneği işletim sistemi yükseltme sırasında işletim sistemi diski bir sanal makine örneğindeki son görüntü sürümüne dayanan yeni bir disk ile değiştirilir. İşletim sistemi yükseltme tamamlandıktan sonra yapılandırılmış uzantıları bu Vm'lerde çalıştırılır. Uygulama, yalnızca bir VM üzerindeki tüm uzantıları başarıyla sağlandığında, sağlıklı olarak değerlendirilir. 

Ayrıca, Ölçek kümesi *gerekir* uygulama platformu üzerinde devam eden durumunu doğru bilgileri sağlamak için sistem durumu Araştırmalarının ile yapılandırılabilir. Uygulama sistem durumu Araştırmalarının yük özel dengeleyici sistem durumu sinyal kullanılan araştırmalar olan. Bir ölçek kümesi VM örneğine üzerinde çalışan uygulama durumunun iyi olup olmadığını belirten dış HTTP veya TCP isteklerine yanıt verebilirsiniz. Özel yük dengeleyici araştırmalarını nasıl çalıştığı hakkında daha fazla bilgi için bkz [yük dengeleyici araştırmalarını anlama](../load-balancer/load-balancer-custom-probe-overview.md).

Kullanarak ölçek kümesi, birden fazla yerleştirme grubundan kullanmak için yapılandırılmışsa, araştırmaları bir [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanılması gerekir.

### <a name="important-keep-credentials-up-to-date"></a>Önemli: Keep kimlik bilgileri güncel
Ölçek kümenizi dış kaynaklara erişmek için herhangi bir kimlik bilgisi kullanıyorsa, kimlik bilgileri güncel tutulduğundan emin olmanız gerekir. Örneğin, VM uzantısı, depolama hesabı için bir SAS belirteci kullanmak için yapılandırılabilir. Sertifikalar ve simgeleri dahil olmak üzere tüm kimlik bilgilerinin süresi dolduysa yükseltme başarısız olur ve sanal makinelerin ilk batch başarısız durumda kalır.

Vm'leri geri yükleme ve bir kaynak kimlik doğrulama hatası varsa, otomatik işletim sistemi yükseltmesi yeniden etkinleştirmek için önerilen adımlar şunlardır:

* Yeniden oluşturma, uzantısı geçirilen belirteci (veya diğer kimlik bilgileri).
* Gelen dış varlıklar için anlaşmak için sanal makine içinde kullanılan tüm kimlik bilgilerini güncel olduğundan emin olun.
* Ölçek kümesi modelinde uzantısı yeni tarafından istenen belirteçleri ile güncelleştirin.
* Başarısız olanlar da dahil olmak üzere tüm sanal makine örnekleri güncelleştirecek güncelleştirilmiş ölçek kümesi dağıtın. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Bir özel yük dengeleyici yoklama uygulama sistem durumu araştırma yapılandırma üzerinde bir ölçek kümesi
*Gerekir* sistem durumu ölçek kümesi için açık olarak bir yük dengeleyici araştırması oluşturun. Aynı uç noktasına bir HTTP araştırması mevcut veya TCP araştırması için kullanılabilir ancak bir durum araştırması geleneksel yük dengeleyici araştırması farklı davranış gerektirebilir. Örneğin, geleneksel bir yük dengeleyici araştırması örneğinde yük çok yüksekse, sağlıksız döndürebilir. Buna karşılık, otomatik işletim sistemi yükseltme sırasında örnek sistem durumu belirlemek için uygun olmayabilir. İki dakikadan az araştırma oranı yüksek olan ve araştırma yapılandırın.

Yük Dengeleyici araştırması içinde başvurulabilir *networkProfile* ölçek kümesidir ve şu şekilde ya da bir dahili veya genel kullanıma yönelik Yük Dengeleyici ile ilişkili olabilir:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Aboneliğiniz bir işletim sistemi görüntüsü yükseltme ilkesi zorlama
Güvenli yükseltmeler için bir yükseltme ilkesi uygulamak önemle tavsiye edilir. Bu ilke, aboneliğiniz uygulama sistem durumu araştırmaları gerektirebilir. Aşağıdaki Azure Resource Manager İlkesi yapılandırılan ayarları yükseltme, otomatik işletim sistemi görüntüsü olmayan dağıtımlar reddeder:

### <a name="powershell"></a>PowerShell
1. Yerleşik Azure Resource Manager ilke tanımıyla elde [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) gibi:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Bir abonelik ilkesi atama [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) gibi:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```

### <a name="cli-20"></a>CLI 2.0
İlke, yerleşik Azure Resource Manager İlkesi ile bir aboneliğe atayın:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Otomatik Güncelleştirmeleri Yapılandır
Otomatik güncelleştirmeleri yapılandırmak için emin olun *automaticosupgrade işlemini* özelliği *true* model tanımı ölçek kümesi. Bu özellik, Azure PowerShell veya Azure CLI ile yapılandırabilirsiniz.

### <a name="powershell"></a>PowerShell
Aşağıdaki örnek, Azure PowerShell kullanır (4.4.1 veya sonraki) adlı ölçek kümesi için otomatik güncelleştirmeleri yapılandırmak için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>CLI 2.0
Aşağıdaki örnek, Azure CLI'yı kullanır (2.0.20 veya sonraki) adlı ölçek kümesi için otomatik güncelleştirmeleri yapılandırmak için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Otomatik işletim sistemi yükseltme durumunu denetleme
Azure PowerShell, Azure CLI veya REST API'leri ile ölçek kümenizde gerçekleştirilen en son işletim sistemi yükseltme durumunu kontrol edebilirsiniz.

### <a name="powershell"></a>PowerShell
Aşağıdaki örnek için Azure PowerShell kullanır (4.4.1 veya sonraki) adlı ölçek kümesi durumunu denetlemek için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnek, Azure CLI'yı kullanır (2.0.20 veya sonraki) adlı ölçek kümesi durumunu denetlemek için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte adlı ölçek kümesi durumunu denetlemek için REST API kullanan *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

GET çağrı özellikleri, aşağıdaki örnek çıktıya benzer döndürür:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Otomatik işletim sistemi yükseltmesi yürütme
Uygulama sistem durumu araştırmalarının kullanımını genişletmek için ölçek kümesinin işletim sistemi yükseltmeleri aşağıdaki adımları yürütün:

1. Sağlıksız % 20'den fazla örnek varsa yükseltme Durdur; Aksi takdirde devam edin.
2. Sonraki toplu VM örnekleri, en fazla % 20'si toplam örnek sayısı olan batch ile yükseltmek için belirleyin.
3. Sanal makine örnekleri sonraki toplu işletim sistemini yükseltin.
4. % 20'den yükseltilmiş örneklerinin sağlıksız almaktadır yükseltme durması; Aksi takdirde devam edin.
5. Bir Service Fabric kümesinin parçası olmayan ölçek kümeleri için yükseltme sağlıklı duruma için araştırmalar için 5 dakika kadar bekler ve sonra hemen sonraki toplu devam eder. Bir Service Fabric kümesinin parçası olan ölçek kümeleri için sonraki toplu geçmeden önce 30 dakika bekler ölçek kümesi.
6. Yükseltmek için örnekleri kaldığı, goto 1. adım) sonraki toplu işlem için; Aksi takdirde yükseltme işlemi tamamlanmış olur.

Her batch yükseltmeden önce işletim sistemi yükseltme altyapısı denetimleri genel VM örneği durumu için ölçek kümesi. Bir batch yükseltirken, olabilir diğer eş zamanlı planlanmış veya planlanmamış bakım Azure sanal makinelerinizin kullanılabilirliğini etkileyebilecek veri merkezlerinde'olmuyor. Bu nedenle % 20'den fazla geçici olarak örnekleri kapalı olabilir mümkündür. Bu gibi durumlarda, geçerli toplu işlem, sonunda yükseltme durdurur ölçek kümesi.


## <a name="deploy-with-a-template"></a>Bir şablon ile dağıtım

Aşağıdaki şablonu otomatik yükseltmeler kullanan bir ölçek kümesini dağıtmak için kullanabileceğiniz <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>otomatik çalışırken yükseltme - Ubuntu 16.04 LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleriyle otomatik işletim sistemi yükseltmelerini kullanma hakkında daha fazla örnek için bkz. [Önizleme özellikleri için GitHub deposunu](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
