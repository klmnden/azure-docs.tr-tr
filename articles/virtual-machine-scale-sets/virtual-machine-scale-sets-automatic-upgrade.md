---
title: Otomatik işletim sistemi görüntüsü yükseltmeleri ile Azure sanal makine ölçek kümeleri | Microsoft Docs
description: İşletim sistemi görüntüsü bir ölçek kümesi VM örnekleri üzerinde otomatik olarak yükseltme hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: rajraj
ms.openlocfilehash: cf25d08fc9a0e1ae458d350be93af31447928ecb
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069463"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltmeleri Azure sanal makine ölçek kümesi

Otomatik işletim sistemi görüntüsü yükseltme tüm sanal makineler en son işletim sistemi görüntüsünü otomatik olarak yükselten bir Azure sanal makine ölçek kümeleri özelliğidir.

Otomatik işletim sistemi yükseltmesi, aşağıdaki özelliklere sahiptir:

- Yapılandırıldıktan sonra görüntü yayımcılar tarafından yayımlanan en son işletim sistemi görüntüsü ölçek kullanıcı müdahalesi olmadan otomatik olarak uygulanır.
- Toplu örneklerinin sıralı bir şekilde her saat yeni bir platform görüntüsü yayımcı tarafından yayımlanan yükseltir.
- Uygulama durum araştırması ile tümleştirilir.
- Hem Windows hem de Linux platform görüntüleri yanı sıra, tüm VM boyutları için çalışır.
- Otomatik yükseltmeler dışında (işletim sistemi yükseltmelerini de el ile başlatılabilir) herhangi bir zamanda seçebilirsiniz.
- Bir VM'nin işletim sistemi diski, en son görüntü sürümü ile oluşturulan yeni işletim sistemi diski ile değiştirilir. Kalıcı veri diskleri korunur yapılandırılmış uzantıları ve özel veri betikler çalıştırılır.
- Azure disk şifrelemesi (önizlemede) şu anda desteklenmiyor.  

## <a name="how-does-automatic-os-image-upgrade-work"></a>Otomatik işletim sistemi nasıl yaptığını görüntü yükseltme iş?

Bir VM'nin işletim sistemi diski en son görüntü sürümü kullanılarak oluşturulmuş yeni bir tane ile değiştirerek bir yükseltme çalışır. Tüm uzantıları yapılandırılır ve özel veri komut dosyaları çalıştırılır, kalıcı veri diskleri korunur. Uygulama kapalı kalma süresini en aza indirmek için makineleri geçiremediğiniz en fazla %20 ölçek herhangi bir zamanda yükseltme kümesinin yerinde yükseltmeler yararlanın. Ayrıca, bir Azure Load Balancer uygulama sistem durumu araştırması tümleştirmek için seçeneğiniz de vardır. Bir uygulama sinyal birleştirmek ve yükseltme işlemi, her batch için yükseltme başarısını doğrulama önemle tavsiye edilir. Yürütme adımlar şunlardır: 

1. Orchestrator yükseltme işlemine başlamadan önce en fazla %20 örneklerinin sağlıksız sağlayacaktır. 
2. Batch VM örnekleri, en fazla %20 toplam örnek sayısı, sahip batch ile yükseltmek için belirleyin.
3. VM örnekleri bu toplu işletim sistemi görüntüsü yükseltin.
4. Müşteri uygulama sistem durumu araştırmaları yapılandırdıysa, yükseltmeden sonraki toplu yükseltme geçmeden önce iyi ve olmak araştırmaları 5 dakikaya kadar bekler. 
5. Yükseltmek için örnekleri kaldığı, goto 1. adım) sonraki toplu işlem için; Aksi takdirde yükseltme işlemi tamamlanmış olur.

Her batch yükseltmeden önce işletim sistemi yükseltme orchestrator denetimleri genel VM örneği durumu için ölçek kümesi. Bir batch yükseltirken, olabilir diğer eş zamanlı planlanmış veya planlanmamış bakım Azure sanal makinelerinizin kullanılabilirliğini etkileyebilecek veri merkezlerinde'olmuyor. Bu nedenle, 20'den fazla geçici olarak % örnekleri çalışmıyor olabilir mümkündür. Bu gibi durumlarda, geçerli toplu işlem, sonunda yükseltme durdurur ölçek kümesi.

## <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
Yalnızca belirli işletim sistemi platform görüntüleri şu anda desteklenmiyor. Kendi oluşturduğunuz sahip özel görüntüler şu anda kullanamazsınız. 

Aşağıdaki SKU'ları şu anda desteklenen (daha fazla gelecekte eklenecektir):
    
| Yayımcı               | İşletim sistemi teklifi      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04 LTS *        | 
| Rogue Wave (OpenLogic)  | CentOS        | 7.5 *              | 
| CoreOS                  | CoreOS        | Dengeli             | 
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter | 
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    | 
| Microsoft Corporation   | WindowsServer | 2016 Datacenter Smalldisk |
| Microsoft Corporation   | WindowsServer | Kapsayıcılar ile 2016 Datacenter |

* Bu görüntüleri için destek, şu anda kullanıma sunuluyor ve kısa süre içinde tüm Azure bölgelerinde kullanılabilir. 

## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntüsü yükseltme yapılandırma gereksinimleri

- *Sürüm* platform görüntüsü özelliği ayarlanmalıdır *son*.
- Uygulama sistem durumu araştırmaları olmayan Service Fabric ölçek kümeleri için kullanın.
- Kaynakları emin kullanılabilir ve güncel kalmasını ölçek kümesi modeline başvurma. 
  Modelinde gizli dizileri için VM uzantısı özellikleri, depolama hesabı, yükteki önyükleme yükteki Exa.SAS URI başvurusu. 

## <a name="configure-automatic-os-image-upgrade"></a>Otomatik işletim sistemi görüntüsü yükseltmeyi yapılandırma
Otomatik işletim sistemi görüntüsü yükseltme yapılandırmak için emin olun *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* özelliği *true* model tanımı ölçek kümesi. 

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

Aşağıdaki örnek, Azure CLI'yı kullanır (2.0.47 veya üzeri) adlı ölçek kümesi için otomatik güncelleştirmeleri yapılandırmak için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az vmss update --name myVMSS --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```
Bu özellik Azure PowerShell aracılığıyla yapılandırma desteği yakında sunulacaktır.

## <a name="using-application-health-probes"></a>Kullanarak uygulama sistem durumu araştırmaları 

İşletim sistemi yükseltme sırasında bir ölçek kümesindeki sanal makine örnekleri yükseltilir aynı anda tek bir toplu. Yükseltme işlemi, yalnızca müşteri Uygulamayı yükseltilmiş kullanarak VM örneklerine sağlıklı olup olmadığını devam etmelidir. Uygulama durum sinyallerini ölçek kümesinin işletim sistemi yükseltme altyapısı sağlar öneririz. Varsayılan olarak, işletim sistemi yükseltmeleri sırasında VM güç durumunu ve uzantı sağlama durumu, bir VM örneği yükseltme sonrasında iyi durumda olup olmadığını belirlemek için platform olarak değerlendirir. Bir sanal makine örneği işletim sistemi yükseltme sırasında işletim sistemi diski bir sanal makine örneğindeki son görüntü sürümüne dayanan yeni bir disk ile değiştirilir. İşletim sistemi yükseltme tamamlandıktan sonra yapılandırılmış uzantıları bu Vm'lerde çalıştırılır. Uygulama, yalnızca bir VM üzerindeki tüm uzantıları başarıyla sağlandığında, sağlıklı olarak değerlendirilir. 

Bir ölçek kümesi, isteğe bağlı olarak uygulama platformu üzerinde devam eden durumunu doğru bilgileri sağlamak için sistem durumu Araştırmalarının ile yapılandırılabilir. Uygulama sistem durumu Araştırmalarının yük özel dengeleyici sistem durumu sinyal kullanılan araştırmalar olan. Bir ölçek kümesi VM örneğine üzerinde çalışan uygulama durumunun iyi olup olmadığını belirten dış HTTP veya TCP isteklerine yanıt verebilirsiniz. Özel yük dengeleyici araştırmalarını nasıl çalıştığı hakkında daha fazla bilgi için bkz [yük dengeleyici araştırmalarını anlama](../load-balancer/load-balancer-custom-probe-overview.md). Bir uygulama sistem durumu araştırma otomatik işletim sistemi yükseltmeleri için gerekli değildir ancak önemle tavsiye edilir.

Kullanarak ölçek kümesi, birden fazla yerleştirme grubundan kullanmak için yapılandırılmışsa, araştırmaları bir [Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanılması gerekir.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Bir özel yük dengeleyici yoklama uygulama sistem durumu araştırma yapılandırma üzerinde bir ölçek kümesi
Sistem durumu ölçek kümesi için en iyi uygulama, açıkça bir yük dengeleyici araştırması oluşturun. Aynı uç noktasına bir HTTP araştırması mevcut veya TCP araştırması için kullanılabilir ancak bir durum araştırması geleneksel yük dengeleyici araştırması farklı davranış gerektirebilir. Örneğin, geleneksel bir yük dengeleyici araştırması, otomatik işletim sistemi yükseltme sırasında örnek sistem durumu belirlemek için uygun olmayabilir ancak örneğinde yük çok yüksekse, sağlıksız döndürebilir. İki dakikadan az araştırma oranı yüksek olan ve araştırma yapılandırın.

Yük Dengeleyici araştırması içinde başvurulabilir *networkProfile* ölçek kümesidir ve şu şekilde ya da bir dahili veya genel kullanıma yönelik Yük Dengeleyici ile ilişkili olabilir:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```
> [!NOTE]
> Otomatik işletim sistemi yükseltmelerini Service Fabric ile kullanırken, yeni işletim sistemi görüntüsü güncelleştirme etki alanı güncelleştirme Service Fabric'te çalışan hizmetler yüksek kullanılabilirliğini sürdürmek için etki alanı tarafından kullanıma alınır. Service Fabric kümeleri dayanıklılık özellikleri hakkında daha fazla bilgi için lütfen bkz [bu belgeleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Kimlik bilgileri güncel tutun
Ölçek kümeniz, depolama hesabı için bir SAS belirteci kullanan bir VM uzantısı yapılandırılmışsa, örneğin dış kaynaklara erişmek için herhangi bir kimlik bilgisi kullanıyorsa kimlik bilgileri güncel tutulduğundan emin olmanız gerekir. Sertifikalar ve simgeleri dahil olmak üzere tüm kimlik bilgilerinin süresi dolduysa yükseltme başarısız olur ve sanal makinelerin ilk batch başarısız durumda kalır.

Vm'leri geri yükleme ve bir kaynak kimlik doğrulama hatası varsa, otomatik işletim sistemi yükseltmesi yeniden etkinleştirmek için önerilen adımlar şunlardır:

* Yeniden oluşturma, uzantısı geçirilen belirteci (veya diğer kimlik bilgileri).
* Gelen dış varlıklar için anlaşmak için sanal makine içinde kullanılan tüm kimlik bilgilerini güncel olduğundan emin olun.
* Ölçek kümesi modelinde uzantısı yeni tarafından istenen belirteçleri ile güncelleştirin.
* Başarısız olanlar da dahil olmak üzere tüm sanal makine örnekleri güncelleştirecek güncelleştirilmiş ölçek kümesi dağıtın. 

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltme geçmişini alma 
Azure PowerShell, Azure CLI 2.0 veya REST API'leri ile ölçek kümenizde gerçekleştirilen en son işletim sistemi yükseltme geçmişini kontrol edebilirsiniz. Son iki ay içinde son beş işletim sistemi yükseltme girişimi için geçmiş alabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki örnekte adlı ölçek kümesi durumunu denetlemek için Azure PowerShell kullanan *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Aşağıdaki örnek, Azure CLI'yı kullanır (2.0.47 veya üzeri) adlı ölçek kümesi durumunu denetlemek için *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```azurecli
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>REST API
Aşağıdaki örnekte adlı ölçek kümesi durumunu denetlemek için REST API kullanan *myVMSS* adlı kaynak grubunda *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```
Burada bu API için belgelere bakın: https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getosupgradehistory.

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

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Bir platform işletim sistemi görüntüsünün son sürümünü almak nasıl? 

Desteklenen SKU'ları kullanarak otomatik işletim sistemi yükseltme, yansıma sürümü alabilir örnekleri aşağıda: 

```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

```powershell
Get-AzureRmVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

```azurecli
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="deploy-with-a-template"></a>Bir şablon ile dağıtım

Aşağıdaki şablonu otomatik yükseltmeler kullanan bir ölçek kümesini dağıtmak için kullanabileceğiniz <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>otomatik çalışırken yükseltme - Ubuntu 16.04 LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleriyle otomatik işletim sistemi yükseltmelerini kullanma hakkında daha fazla örnek için bkz. [Önizleme özellikleri için GitHub deposunu](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
