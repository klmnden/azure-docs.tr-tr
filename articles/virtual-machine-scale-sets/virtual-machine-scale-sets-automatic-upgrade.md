---
title: Otomatik işletim sistemi görüntüsü yükseltmeleri ile Azure sanal makine ölçek kümeleri | Microsoft Docs
description: İşletim sistemi görüntüsü bir ölçek kümesi VM örnekleri üzerinde otomatik olarak yükseltme hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: manayar
ms.openlocfilehash: 007f2801efed8da4964808056563418dec7f64d5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328825"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltmeleri Azure sanal makine ölçek kümesi

Otomatik işletim sistemi görüntüsü etkinleştirme, Ölçek kümesi yardımcı kolayca güncelleştirme yönetim işletim sistemi diski ölçek kümesindeki tüm örnekleri için güvenli bir şekilde ve otomatik olarak yükselterek yükseltir.

Otomatik işletim sistemi yükseltmesi, aşağıdaki özelliklere sahiptir:

- Yapılandırıldıktan sonra görüntü yayımcılar tarafından yayımlanan en son işletim sistemi görüntüsü ölçek kullanıcı müdahalesi olmadan otomatik olarak uygulanır.
- Toplu örneklerinin sıralı bir şekilde her saat yeni bir platform görüntüsü yayımcı tarafından yayımlanan yükseltir.
- Uygulama sistem durumu araştırmaları ile tümleşir ve [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md).
- Hem Windows hem de Linux platform görüntüleri yanı sıra, tüm VM boyutları için çalışır.
- Otomatik yükseltmeler dışında (işletim sistemi yükseltmelerini de el ile başlatılabilir) herhangi bir zamanda seçebilirsiniz.
- Bir VM'nin işletim sistemi diski, en son görüntü sürümü ile oluşturulan yeni işletim sistemi diski ile değiştirilir. Kalıcı veri diskleri korunur yapılandırılmış uzantıları ve özel veri betikler çalıştırılır.
- [Uzantı sıralama](virtual-machine-scale-sets-extension-sequencing.md) desteklenir.

## <a name="how-does-automatic-os-image-upgrade-work"></a>Otomatik işletim sistemi nasıl yaptığını görüntü yükseltme iş?

Yükseltme son görüntü sürümü kullanılarak oluşturulmuş yeni bir disk ile bir sanal makinenin işletim sistemi diski değiştirerek çalışır. Kalıcı veri diskleri korunur çalışırken işletim sistemi diskini üzerinde herhangi bir yapılandırılmış uzantıları ve özel veri betikleri çalıştırın. Uygulamaların kapalı kalma süresini en aza indirmek için en fazla %20, Ölçek kümesindeki herhangi bir zamanda yükseltme bir toplu iş yerinde yükseltmeler yararlanın. Bir Azure Load Balancer uygulama sistem durumu araştırması tümleştirebilirler veya [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md). Biz, önerilen bir uygulama sinyal ekleme ve yükseltme işlemi, her batch için yükseltme başarısını doğrulama.

Yükseltme işlemi aşağıdaki gibi çalışır:
1. Orchestrator yükseltme işlemine başlamadan önce en fazla %20 tüm ölçek kümesindeki örneklerin (herhangi bir nedenle) sağlıksız sağlayacaktır.
2. VM örnekleri, en fazla %20 toplam örnek sayısı, sahip herhangi bir batch ile yükseltme toplu yükseltme orchestrator tanımlar.
3. İşletim sistemi diskinin VM örneklerinin Seçili toplu işin en son görüntüden oluşturulan yeni bir işletim sistemi diski ile değiştirilir ve tüm belirtilen uzantılara ve ölçek kümesi modelinde yapılandırmaları için yükseltilmiş örneğinde uygulanır.
4. Yapılandırılmış uygulama sistem durumu araştırmaları ya da uygulama durumunu uzantısı ile ölçek kümeleri için yükseltmeden sonraki toplu yükseltme geçmeden önce iyi ve olmak örnek 5 dakika kadar bekler.
5. Yükseltme orchestrator sağlıksız sonrası yükseltme hale gelen örneklerin yüzde de izler. Yükseltme işlemi sırasında % 20'den fazla yükseltilen örneklerinin sağlıksız hale gelirse, yükseltme durdurulur.
6. Yukarıdaki işlem, Ölçek kümesindeki tüm örnekleri yükseltilene dek devam eder.

Her batch yükseltmeden önce işletim sistemi yükseltme orchestrator denetimleri genel ölçek kümesi durumu için ölçek kümesi. Bir batch yükseltirken, olabilir, Ölçek kümesi örneklerine durumunu etkileyebilecek diğer eş zamanlı planlanmış veya planlanmamış bakım etkinlikleri. Bu gibi durumlarda, daha fazla sistem durumu, Ölçek kümesinin örnekleri % 20'den sonra ölçeği yükseltme durakları geçerli toplu işlem sonunda ayarlayın.

## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Yalnızca belirli işletim sistemi platform görüntüleri şu anda desteklenmiyor. Özel görüntüler şu anda desteklenmemektedir.

Aşağıdaki SKU'ları şu anda desteklenen (ve daha fazla düzenli olarak eklenir):

| Yayımcı               | İşletim sistemi teklifi      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| Rogue Wave (OpenLogic)  | CentOS        | 7.5                |
| CoreOS                  | CoreOS        | Dengeli             |
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    |
| Microsoft Corporation   | WindowsServer | 2016 Datacenter Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-with-Containers |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-with-Containers |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntüsü yükseltme yapılandırma gereksinimleri

- *Sürüm* platform görüntüsü özelliği ayarlanmalıdır *son*.
- Uygulama sistem durumu araştırmaları kullanın veya [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md) için Service Fabric ölçek kümeleri.
- Ölçek kümesi modelinde belirtilen dış kaynaklara kullanılabilir ve güncel olduğundan emin olun. Örnekler, VM uzantısı özellikleri, depolama hesabındaki yükü yükteki önyükleme için SAS URI'sini içerir, model ve diğer gizli dizileri başvurusu.

## <a name="configure-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntüsü yükseltmeyi yapılandırma
Otomatik işletim sistemi görüntüsü yükseltme yapılandırmak için emin olun *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* özelliği *true* model tanımı ölçek kümesi.

### <a name="rest-api"></a>REST API
Aşağıdaki örnek, bir ölçek kümesi modeline üzerinde otomatik işletim sistemi yükseltmelerini ayarlama işlemi açıklanmaktadır:

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Kullanım [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'ini ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol edin. Aşağıdaki örnekte adlı ölçek kümesi için Otomatik yükseltme yapılandırır *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Kullanım [az vmss update](/cli/azure/vmss#az-vmss-update) ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol etmek için. Azure clı'yi 2.0.47 veya üzeri. Aşağıdaki örnekte adlı ölçek kümesi için Otomatik yükseltme yapılandırır *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli-interactive
az vmss update --name myVMSS --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

## <a name="using-application-health-probes"></a>Kullanarak uygulama sistem durumu araştırmaları

İşletim sistemi yükseltme sırasında bir ölçek kümesindeki sanal makine örnekleri yükseltilir aynı anda tek bir toplu. Yükseltme işlemi, yalnızca müşteri Uygulamayı yükseltilmiş kullanarak VM örneklerine sağlıklı olup olmadığını devam etmelidir. Ölçek kümesi işletim sistemi yükseltme altyapısı durum sinyallerini uygulamanın sağladığı öneririz. Varsayılan olarak, işletim sistemi yükseltmeleri sırasında VM güç durumunu ve uzantı sağlama durumu, bir VM örneği yükseltme sonrasında iyi durumda olup olmadığını belirlemek için platform olarak değerlendirir. Bir sanal makine örneği işletim sistemi yükseltme sırasında işletim sistemi diski bir sanal makine örneğindeki son görüntü sürümüne dayanan yeni bir disk ile değiştirilir. İşletim sistemi yükseltme tamamlandıktan sonra yapılandırılmış uzantıları bu Vm'lerde çalıştırılır. Uygulama, yalnızca örneğindeki tüm uzantıları başarıyla sağlandığında sağlıklı olarak değerlendirilir.

Bir ölçek kümesi, isteğe bağlı olarak uygulama platformu üzerinde devam eden durumunu doğru bilgileri sağlamak için sistem durumu Araştırmalarının ile yapılandırılabilir. Uygulama sistem durumu Araştırmalarının yük özel dengeleyici sistem durumu sinyal kullanılan araştırmalar olan. Bir ölçek kümesi VM örneğine üzerinde çalışan uygulama durumunun iyi olup olmadığını belirten dış HTTP veya TCP isteklerine yanıt verebilirsiniz. Özel yük dengeleyici araştırmalarını nasıl çalıştığı hakkında daha fazla bilgi için bkz [yük dengeleyici araştırmalarını anlama](../load-balancer/load-balancer-custom-probe-overview.md). Bir uygulama sistem durumu araştırması Service Fabric ölçek kümeleri için gerekli değildir, ancak önerilir. Ölçek kümeleri olmayan Service Fabric gerektirir ya da yük dengeleyici uygulama sistem durumu araştırmalarının veya [uygulama Uzantısın](virtual-machine-scale-sets-health-extension.md).

Kullanarak ölçek kümesi, birden fazla yerleştirme grubundan kullanmak için yapılandırılmışsa, araştırmaları bir [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanılması gerekir.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Bir özel yük dengeleyici yoklama uygulama sistem durumu araştırma yapılandırma üzerinde bir ölçek kümesi
Sistem durumu ölçek kümesi için en iyi uygulama, açıkça bir yük dengeleyici araştırması oluşturun. Aynı uç noktasına bir HTTP araştırması mevcut veya TCP araştırması için kullanılabilir, ancak bir durum araştırması geleneksel yük dengeleyici araştırması farklı davranış gerektirebilir. Örneğin, geleneksel bir yük dengeleyici araştırması örneği üzerindeki yükü çok yüksek olduğu halde, otomatik işletim sistemi yükseltme sırasında örnek sistem durumu belirlemek için uygun olmayacaktır sağlıksız döndürebilir. İki dakikadan az araştırma oranı yüksek olan ve araştırma yapılandırın.

Yük Dengeleyici araştırması içinde başvurulabilir *networkProfile* ölçek kümesidir ve şu şekilde ya da bir dahili veya genel kullanıma yönelik Yük Dengeleyici ile ilişkili olabilir:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> Otomatik işletim sistemi yükseltmelerini Service Fabric ile kullanırken, yeni işletim sistemi görüntüsü güncelleştirme etki alanı güncelleştirme Service Fabric'te çalışan hizmetler yüksek kullanılabilirliğini sürdürmek için etki alanı tarafından kullanıma alınır. Service fabric'te otomatik işletim sistemi yükseltmelerini kullanmasına izin kümenizi Silver dayanıklılık katmanı kullanmak için yapılandırılmış veya üzeri olması gerekir. Service Fabric kümeleri dayanıklılık özellikleri hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Kimlik bilgileri güncel tutun
Örneğin, depolama hesabı için bir SAS belirteci kullanan bir VM uzantısı yapılandırılmışsa, dış kaynaklara erişmek için herhangi bir kimlik bilgisi ölçek kümeniz kullanıyorsa, kimlik bilgilerinin güncelleştirildiğinden emin olun. Sertifikalar ve belirteçleri, dahil olmak üzere tüm kimlik bilgilerinin süresi dolduysa yükseltme başarısız olur ve sanal makinelerin ilk batch başarısız durumda kalır.

Vm'leri geri yükleme ve bir kaynak kimlik doğrulama hatası varsa, otomatik işletim sistemi yükseltmesi yeniden etkinleştirmek için önerilen adımlar şunlardır:

* Yeniden oluşturma, uzantısı geçirilen belirteci (veya diğer kimlik bilgileri).
* Gelen dış varlıklar için anlaşmak için sanal makine içinde kullanılan tüm kimlik bilgilerini güncel olduğundan emin olun.
* Ölçek kümesi modelinde uzantısı yeni tarafından istenen belirteçleri ile güncelleştirin.
* Başarısız olanlar da dahil olmak üzere tüm sanal makine örnekleri güncelleştirecek güncelleştirilmiş ölçek kümesi dağıtın.

## <a name="using-application-health-extension"></a>Uygulama durumunu uzantısını kullanma
Uygulama durumunu uzantısı, sanal makine ölçek kümesi örneği ve sistem durumu VM ölçek kümesi örneğinin içinde raporlarda içinde dağıtılır. Bir uygulama uç noktaya yoklama ve uygulamanın bu örneği üzerinde durumunu güncelleştirmek için uzantısını yapılandırabilirsiniz. Bu örneği durumu örneği yükseltme işlemleri için uygun olup olmadığını belirlemek için Azure tarafından denetlenir.

Uzantı raporları sistem bir VM içinde uzantı, dış araştırmalarının uygulama sistem durumu Araştırmalarının gibi durumlarda kullanılabilir (özel Azure Load Balancer kullanan [araştırmaları](../load-balancer/load-balancer-custom-probe-overview.md)) kullanılamaz.

Ölçek kümenizi uzantısı ayarlar örneklerde de ayrıntılı olarak uygulama durumunun dağıtma birden çok yolu vardır [bu makalede](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension).

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltme geçmişini alma
Azure PowerShell, Azure CLI 2.0 veya REST API'leri ile ölçek kümenizde gerçekleştirilen en son işletim sistemi yükseltme geçmişini kontrol edebilirsiniz. Son iki ay içinde son beş işletim sistemi yükseltme girişimi için geçmiş alabilirsiniz.

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte [REST API](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) adlı ölçek kümesi durumunu denetlemek için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```

GET çağrı özellikleri, aşağıdaki örnek çıktıya benzer döndürür:

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Kullanım [Get-AzVmss](/powershell/module/az.compute/get-azvmss) cmdlet'ini ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol edin. Aşağıdaki örnek nasıl adlı bir ölçek kümesi için işletim sistemi yükseltme durumunu gözden geçirme ayrıntıları *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Kullanım [az vmss get-os-yükseltme-geçmiş](/cli/azure/vmss#az-vmss-get-os-upgrade-history) ölçek kümeniz için işletim sistemi yükseltme geçmişini kontrol etmek için. Azure clı'yi 2.0.47 veya üzeri. Aşağıdaki örnek nasıl adlı bir ölçek kümesi için işletim sistemi yükseltme durumunu gözden geçirme ayrıntıları *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myVMSS
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Bir platform işletim sistemi görüntüsünün son sürümünü almak nasıl?

Desteklenen SKU'ları kullanarak otomatik işletim sistemi yükseltme, yansıma sürümü alabilir örnekleri aşağıda:

### <a name="rest-api"></a>REST API
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="deploy-with-a-template"></a>Bir şablon ile dağıtım

Şablon ile desteklenen görüntüler için otomatik işletim sistemi yükseltmeleri gibi bir ölçek kümesi dağıtmak için kullanabileceğiniz [Ubuntu 16.04 LTS](https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleriyle otomatik işletim sistemi yükseltmelerini kullanma hakkında daha fazla örnek için gözden [GitHub deposunu](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
