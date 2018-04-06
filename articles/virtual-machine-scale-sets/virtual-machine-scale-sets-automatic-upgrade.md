---
title: Otomatik işletim sistemi yükseltme Azure sanal makine ölçek kümeleri | Microsoft Docs
description: İşletim sisteminde bir ölçek kümesi VM örnekleri otomatik olarak Yükselt öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: negat
ms.openlocfilehash: 28a9b3d68037aac0c1198da4232c045487b01174
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Otomatik işletim sistemi yükseltme Azure sanal makine ölçek kümesi

Otomatik işletim sistemi görüntüsü en son işletim sistemi görüntüsü için tüm sanal makineleri otomatik olarak yükseltir bir önizleme özelliği Azure sanal makine ölçek kümeleri için yükseltmedir.

Otomatik işletim sistemi yükseltme, aşağıdaki özelliklere sahiptir:

- Bir kere yapılandırıldığında, görüntü yayımcılar tarafından yayımlanan en son işletim sistemi görüntüsü ölçeği kullanıcı müdahalesi olmadan Ayarla otomatik olarak uygulanır.
- Toplu işlemleri örneklerinin çalışırken bir şekilde yeni bir platform görüntüsü yayımcı tarafından yayımlanan her zaman yükseltir.
- Uygulama durumu araştırması ile tümleştirilir (isteğe bağlı, ancak koruması önemle önerilir).
- Tüm VM boyutları için çalışır.
- Windows için çalışır ve Linux platform görüntüler.
- Otomatik yükseltmeler dışında (işletim sistemi yükseltmeleri de el ile başlatılabilir) herhangi bir zamanda tercih edebilirsiniz.
- Bir VM'nin işletim sistemi diski son görüntü sürümü ile oluşturulmuş yeni işletim sistemi diski ile değiştirilir. Kalıcı veri diskleri korunur sırasında yapılandırılmış uzantılarını ve özel veri komut dosyaları çalıştırılır.


## <a name="preview-notes"></a>Önizleme notları 
Önizleme sırasında aşağıdaki sınırlamalar ve kısıtlamalar geçerlidir:

- Otomatik işletim sistemi yükseltmeleri yalnızca Destek [dört OS SKU'ları](#supported-os-images). SLA veya garanti yoktur. Otomatik yükseltme üretim kritik iş yükleri üzerinde Önizleme sırasında kullanmamanızı öneririz.
- Azure disk şifrelemesi (şu anda önizlemede) **değil** sanal makine ölçek kümesi otomatik işletim sistemi yükseltme ile şu anda desteklenmiyor.


## <a name="register-to-use-automatic-os-upgrade"></a>Otomatik işletim sistemi yükseltme kullanmak için kaydolun
Otomatik işletim sistemi yükseltme özelliği kullanmak için Önizleme sağlayıcı ile kayıt [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) gibi:

```powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
```

Bir rapor olarak kayıt durumu için yaklaşık 10 dakika sürer *kayıtlı*. Geçerli kayıt durumuyla denetleyebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Kayıtlı sonra şunları *Microsoft.Compute* sağlayıcısı ile kayıtlı [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) gibi:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

> [!NOTE]
> Service Fabric kümeleri kendi uygulama sağlığını kavramı sahip ancak uygulamasının durumunu izlemek için yük dengeleyici durum araştırması ölçek kümeleri Service Fabric olmadan kullanın. Sistem durumu araştırmalarının sağlayıcı özelliği kaydetmek için kullanın [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) gibi:
>
> ```powershell
> Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
> ```
>
> Yeniden kayıt durumu için yaklaşık 10 dakika rapor sürer *kayıtlı*. Geçerli kayıt durumuyla denetleyebilirsiniz [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Kez kayıtlı emin *Microsoft.Network* sağlayıcısı ile kayıtlı [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) gibi:
>
> ```powershell
> Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
> ```

## <a name="portal-experience"></a>Portal deneyimi
Yukarıdaki kayıt adımları izledikten sonra gidebilirsiniz [Azure portalı](https://aka.ms/managed-compute) ölçek kümeleri üzerinde otomatik işletim sistemi yükseltme etkinleştirmek ve yükseltme sürecini görmek için:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Yalnızca belirli işletim sistemi platform görüntüleri şu anda desteklenmiyor. Kendi oluşturduğunuz sahip özel resimler şu anda kullanamazsınız. *Sürüm* platform görüntüsü özelliği ayarlanmalıdır *son*.

Aşağıdaki SKU'ları şu anda desteklenen (daha fazla eklenir):
    
| Yayımcı               | Sunduğu         |  Sku               | Sürüm  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2012-R2-Datacenter | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter    | en son   |
| MicrosoftWindowsServer  | WindowsServer | 2016-Datacenter-Smalldisk | en son   |



## <a name="application-health-without-service-fabric"></a>Service Fabric olmadan uygulama durumu
> [!NOTE]
> Bu bölüm, yalnızca Service Fabric olmadan ölçek kümeleri için geçerlidir. Service Fabric uygulama sağlığını kendi kavramı vardır. Otomatik işletim sistemi yükseltme Service Fabric ile kullanırken, yeni işletim sistemi görüntüsü güncelleştirme etki alanı güncelleştirme Service Fabric çalışan hizmetler, yüksek kullanılabilirliği sürdürmek için etki alanı tarafından kullanıma alındı. Service Fabric kümeleri dayanıklılık özellikleri hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Bir işletim sistemi yükseltme sırasında bir ölçek kümesindeki VM örnekleri yükseltilir aynı anda tek bir toplu. Yükseltme işlemi, yalnızca müşteri uygulaması yükseltilmiş VM örneklerinde sağlıklı olup olmadığını devam etmelidir. Bu nedenle, uygulama durumu sinyalleri ölçek kümesi işletim sistemi yükseltme altyapısı sağlar gerektirir. İşletim sistemi yükseltmeleri sırasında platform VM güç durumunu ve bir yükseltmeden sonra bir VM örneği sağlıklı olup olmadığını belirlemek için durum sağlama uzantısı göz önünde bulundurur. Bir VM örneğinin işletim sistemi yükseltme sırasında VM örneği üzerinde işletim sistemi diski son görüntü sürümlerine göre yeni bir disk ile değiştirilir. İşletim sistemi yükseltme tamamlandıktan sonra yapılandırılmış uzantılarını bu Vm'lere çalıştırılır. Yalnızca bir VM üzerindeki tüm uzantılar başarıyla sağlandığında, uygulama sağlıklı olarak kabul edilir. 

Ayrıca, Ölçek kümesini *gerekir* uygulama platformu uygulama devam eden durumunu doğru bilgi sağlamak için sistem durumu Araştırmalarının ile yapılandırılabilir. Uygulama sistem durumu Araştırmalarının yük özel dengeleyici sistem durumu sinyal kullanılan yoklamaları var. Bir ölçek kümesi VM örneği üzerinde çalışan uygulama sağlıklı olup olmadığını belirten dış HTTP veya TCP isteklerine yanıt verebilir. Özel yük dengeleyici araştırmaları nasıl çalıştığı hakkında daha fazla bilgi için bkz: [anlayın yük dengeleyici araştırmalar](../load-balancer/load-balancer-custom-probe-overview.md).

Ölçek kümesini birden çok yerleştirme grupları kullanacak şekilde yapılandırılmışsa, kullanarak yoklamaları bir [standart yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanılması gerekir.

### <a name="important-keep-credentials-up-to-date"></a>Önemli: kimlik bilgileri güncel tutun
Depolama hesabı için bir SAS belirteci kullanan bir VM uzantısı yapılandırılmışsa, örneğin dış kaynaklara erişmek için herhangi bir kimlik bilgisi ölçek kümesini kullanıyorsa, kimlik bilgilerinin güncel tutulduğundan emin olmak gerekir. Sertifikalar ve belirteçleri de dahil olmak üzere tüm kimlik bilgilerinin süresi dolmuş, yükseltme başarısız olur ve VM'lerin ilk batch başarısız durumda kalır.

Sanal makineleri kurtarma ve kaynak kimlik doğrulama hatası varsa otomatik işletim sistemi yükseltme yeniden etkinleştirmek için önerilen adımları şunlardır:

* Belirteç (veya diğer kimlik bilgilerini) geçirilen, uzantılarını yeniden.
* VM içinde dış varlıklara konuşmaya kullanıldığında kimlik güncel olduğundan emin olun.
* Ölçek kümesi modelinde uzantılarını herhangi yeni belirteçleri ile güncelleştirin.
* Başarısız olanlar da dahil olmak üzere tüm VM örnekleri güncelleştirecektir güncelleştirilmiş ölçek kümesini dağıtın. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Bir özel yük dengeleyici araştırması uygulama sistem durumu araştırma bir ölçekte yapılandırma ayarlayın
*Gerekir* sağlık ölçeği ayarlamak için yük dengeleyici araştırmasını açıkça oluşturun. Aynı uç nokta var olan HTTP araştırmasını veya TCP araştırması için kullanılabilir ancak bir sistem durumu araştırması geleneksel yük dengeleyici araştırması farklı davranışından gerektirebilir. Örneğin, geleneksel yük dengeleyici araştırmasını, örnek durumu otomatik işletim sistemi yükseltme sırasında belirlemek için uygun olmayabilir ancak örneği üzerindeki yükü çok yüksekse, sağlıksız döndürebilir. İki dakikadan kısa bir yüksek yoklama hızı için araştırma yapılandırın.

Yük Dengeleyici araştırmasını olarak başvurulabilir *networkProfile* ölçek ayarlama ve şu şekilde ya da bir dahili veya genel kullanıma yönelik Yük Dengeleyici ile ilişkili olabilir:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Bir işletim sistemi görüntüsü yükseltme İlkesi aboneliğinizi zorla
Güvenli yükseltmeler için bir yükseltme politikasını kullanmamanız önerilir. Bu ilke, aboneliğinizi uygulama sistem durumu araştırmalarının gerektirebilir. Aşağıdaki Azure Resource Manager İlkesi yükseltme yapılandırılmış ayarları otomatik işletim sistemi görüntüsü olmayan dağıtımlar reddeder:

1. Yerleşik Azure Resource Manager ilke tanımıyla elde [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) gibi:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Bir abonelik ilkesi atama [yeni AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) gibi:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```


## <a name="configure-auto-updates"></a>Otomatik Güncelleştirmeler'i yapılandırma
Otomatik güncelleştirmeleri yapılandırmak için emin *automaticOSUpgrade* özelliği ayarlanmış *true* model tanım ölçek kümesindeki. Bu özellik, Azure PowerShell veya Azure CLI 2.0 ile yapılandırabilirsiniz.

Aşağıdaki örnek, Azure PowerShell kullanır (4.4.1 veya sonrası) adlandırılmış kümesi ölçek için otomatik güncelleştirmeleri yapılandırmak için *myVMSS* kaynak grubunda adlı *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```


Aşağıdaki örnek, Azure CLI kullanır (2.0.20 veya sonrası) adlandırılmış kümesi ölçek için otomatik güncelleştirmeleri yapılandırmak için *myVMSS* kaynak grubunda adlı *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Otomatik bir işletim sistemi yükseltme durumunu denetleme
Azure PowerShell, Azure CLI 2.0 veya REST API'leri ile ayarlamak, ölçek üzerinde gerçekleştirilen en son işletim sistemi yükseltme durumunu kontrol edebilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örneğe Azure PowerShell kullanır (4.4.1 veya sonrası) ölçeği adlandırılmış Ayarla durumunu denetlemek için *myVMSS* kaynak grubunda adlı *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aşağıdaki örnek, Azure CLI kullanır (2.0.20 veya sonrası) ölçeği adlandırılmış Ayarla durumunu denetlemek için *myVMSS* kaynak grubunda adlı *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST API
Aşağıdaki örnek, ölçeği adlandırılmış Ayarla durumunu denetlemek için REST API kullanır. *myVMSS* kaynak grubunda adlı *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

GET çağrı özellikleri aşağıdaki örnek çıktıya benzer döndürür:

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


## <a name="automatic-os-upgrade-execution"></a>Otomatik işletim sistemi yükseltme yürütme
Uygulama sistem durumu araştırmalarının kullanımını genişlemeye ölçek kümesi işletim sistemi yükseltmeleri aşağıdaki adımları yürütün:

1. % 20'den fazla örneklerinin sağlıksız varsa, yükseltme Durdur; Aksi takdirde devam edin.
2. Sonraki toplu işi, en fazla % 20'sini toplam örnek sayısı olan bir toplu işin yükseltmek için VM örnekleri tanımlayın.
3. VM örnekleri sonraki toplu işletim sistemini yükseltin.
4. Yükseltilen örnekleri % 20'den fazla sağlıksız varsa, yükseltme Durdur; Aksi takdirde devam edin.
5. Service Fabric kümesinin parçası olmayan ölçek kümeleri için yükseltme sağlıklı duruma araştırmalar 5 dakika kadar bekler sonra hemen sonraki toplu devam eder. Service Fabric kümesinin parçası olan ölçek kümeleri için ölçek sonraki toplu geçmeden önce 30 dakika bekler ayarlayın.
6. Yükseltme için örnekler kaldığı varsa goto adım 1) için bir sonraki toplu; Aksi takdirde yükseltme işlemi tamamlanmış olur.

Ölçek her toplu yükseltmeden önce işletim sistemi yükseltme altyapısı denetleyeceğini VM örneği durumunu ayarlayın. Bir toplu yükseltirken olabilir diğer eş zamanlı planlanmış veya planlanmamış bakım Vm'leriniz kullanılabilirliğini etkileyebilecek Azure merkezlerinde olmuyor. Bu nedenle, geçici olarak birden fazla %20 örnekleri kapalı olabilir mümkündür. Böyle durumlarda, geçerli toplu işlem sonunda ölçeği yükseltme durakları ayarlayın.


## <a name="deploy-with-a-template"></a>Şablon ile dağıtma

Aşağıdaki şablonu otomatik yükseltme kullanan bir ölçek kümesini dağıtmak için kullanabileceğiniz <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>otomatik çalışırken yükseltme - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Sonraki adımlar
Otomatik işletim sistemi yükseltme ölçek kümeleri ile kullanma hakkında daha fazla örnek için bkz: [Önizleme özellikleri için GitHub deposuna](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
