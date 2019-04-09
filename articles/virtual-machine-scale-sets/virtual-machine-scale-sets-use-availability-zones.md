---
title: Kullanılabilirlik alanları kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs
description: Kullanılabilirlik için artıklığı kesintilere karşı kullanan Azure sanal makine ölçek kümeleri oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: cynthn
ms.openlocfilehash: 24cff3a2ec4d0bed7a030ca430eaa698eb4a7325
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278777"
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Kullanılabilirlik alanları kullanan bir sanal makine ölçek kümesi oluşturma

Sanal makine ölçek kümeleri, veri merkezi düzeyinde arızasına karşı korumak için kullanılabilirlik alanında ölçek oluşturabilirsiniz. Kullanılabilirlik alanlarını destekleyen azure bölgeleri sahip en az üç ayrı bölgelerinin her biri kendi bağımsız güç kaynağı, ağ ve soğutma. Daha fazla bilgi için [kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md).

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Bir ölçek kümesi bir veya daha fazla API sürümünden itibaren bölgelere dağıtırken *2017-12-01*, "en yüksek yayılma" veya "statik 5 hata etki alanı yayılmak" dağıtma seçeneğine sahipsiniz. Pek çok hata etki alanı her bölgedeki mümkün olduğunca gibi en yüksek yayılma ile ölçek kümesi sanal makineleriniz arasında yayar. Arasında bu yayılmasını olabilir büyük veya Beşten az hata etki alanı bölgesi başına. "Statik 5 hata etki alanı yayılma ile", Ölçek kümesi sanal makinelerinizin tam olarak beş hata etki alanına bölge başına yayılır. Ölçek kümesi ayırma isteğini karşılamak için bölge başına beş ayrı hata etki alanı bulunamıyor istek başarısız olur.

**Çoğu iş yükü için en yüksek yayılma ile dağıtma öneririz**gibi bu yaklaşım, çoğu durumda en iyi yayılmasını sağlar. Farklı donanım yalıtım birimlerindeki yayılmaya çoğaltmaları gerekiyorsa, biz kullanılabilirlik alanları genelinde yaymak önerilir yazılımınız olması ve her bölgede en fazla yayılma.

En yüksek yayılma ile ölçek kümesi sanal makine örnek görünümünü ve örnek meta veri Vm'leri yayılmış kaç hata etki alanlarına bakılmaksızın bir hata etki alanı yalnızca görebilirsiniz. Her bölge içinde yayılmasını örtük olarak bulunuyor.

En yüksek yayılma kullanmak için ayarlanmış *platformFaultDomainCount* için *1*. Statik beş hata etki alanı yayılmasını kullanmak için ayarlanmış *platformFaultDomainCount* için *5*. API Sürüm *2017-12-01*, *platformFaultDomainCount* varsayılan olarak *1* tek bölgeli ve bölgeler arası ölçek için ayarlar. Şu anda yalnızca statik beş hata etki alanı yayılma bölgesel ölçek kümeleri için desteklenir.

### <a name="placement-groups"></a>Yerleştirme grupları

Bir ölçek kümesi dağıtırken, tek bir dağıtmak için seçeneğiniz de [yerleştirme grubu](./virtual-machine-scale-sets-placement-groups.md) kullanılabilirlik alanı başına veya birden çok bölge başına. Bölgesel ölçek kümeleri için tek bir yerleştirme grubu bölgesinde veya birden çok bölgede olmasını seçimdir. Çoğu iş yükü için birden fazla yerleştirme grubundan için daha büyük bir ölçek sağlayan öneririz. API Sürüm *2017-12-01*, ölçeği birden fazla yerleştirme grubundan tek bölgeli ve bölgeler arası ölçek kümeleri için varsayılan ayarlar, ancak bunlar tek bir yerleştirme grubu bölgesel ölçek kümeleri için varsayılan.

> [!NOTE]
> En yüksek yayılma kullanırsanız, birden fazla yerleştirme grubundan kullanmanız gerekir.

### <a name="zone-balancing"></a>Bölge Dengeleme

Son olarak, birden çok alanda dağıtılan ölçek kümeleri için de "en iyi çaba alan dengesi" veya "katı alan dengesi" seçme seçeneğiniz vardır. Bir ölçek kümesi "her VM ile aynı sayıda bölge, dengeli" olarak kabul edilir veya +\\-1 sanal makine ölçek kümesi için diğer tüm bölgelerde. Örneğin:

- Bölge 1, bölge 3, 2 ve 3 Vm'leri olarak kabul edilir bölgedeki 3 VM içinde 2 VM içeren bir ölçek dengeli. Farklı bir VM sayısı ile yalnızca bir alan yoktur ve diğer bölgeler'den küçük olan yalnızca 1 olan. 
- Ölçek bölge 1 ile 1 sanal makine kümesi, bölge 3, 2 ve 3 Vm'leri olarak kabul edilir bölgedeki 3 VM dengesiz. Bölge 1, 2, bölge 2 ve 3'den daha az VM var.

Ölçek kümesindeki sanal makineler başarıyla oluşturuldu, ancak bu vm'lerdeki uzantılarını dağıtma başarısız olabilir. Bu VM uzantısı hatalarıyla yine de bir ölçek kümesi dengelenir belirlerken dikkate alınır. Örneği için 3 VM içeren bölge 1, bölge 2, 3 VM ve bölge 3, 3 VM içinde bir ölçek kümesi 1 bölgesinde tüm uzantıları başarısız olsa bile dengeli olarak kabul edilir ve tüm uzantıları 2 ve 3 bölgelerde başarılı oldu.

En yüksek çaba alan dengesi ile Bakiye korurken ölçeğini daraltma ve genişletme girişimleri ölçek kümesi. Ancak, bazı nedenlerden dolayı bu mümkün değilse (bir bölge kalırsa, örneğin, Ölçek kümesinin yeni bir sanal makine bu bölgede oluşturulamıyor), Ölçek kümesi başarıyla ölçeğini daraltma veya genişletme geçici dengesizliği sağlar. Sonraki genişleme girişimleri, Ölçek kümesinin Vm'leri daha fazla sanal makine ölçek dengelenmeye devam kümesi için gereken alanları ekler. Benzer şekilde, girişim sonraki ölçek üzerinde daha az dengelenmeye ölçek için gereken bölgelerdeki Vm'leri ölçek kümesi kaldırır. "Katı alan dengesi" ile ölçek kümesi bunu yaparsanız bu nedenle unbalance neden olacaksa ölçeğini daraltma veya genişletme girişimleri başarısız olur.

En yüksek çaba alan dengesi kullanmak için ayarlanmış *zoneBalance* için *false*. Bu API sürümünde varsayılan ayardır *2017-12-01*. Katı alan dengesi kullanmak için ayarlanmış *zoneBalance* için *true*.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Tek bölgeli ve bölgesel olarak yedekli ölçek kümeleri

Bir sanal makine ölçek kümesi dağıtırken, tek bir kullanılabilirlik alanı bir bölgedeki veya birden çok bölgeyi kullanmayı da tercih edebilirsiniz.

Bir ölçek kümesi tek bir bölge, hangi bölgede tüm VM örnekleri çalıştırmak ve ölçek kümesi yönetilen, denetimini ve yalnızca ilgili bölge içinde daralttığında oluşturduğunuzda. Bölgesel olarak yedekli ölçek kümesi, birden çok bölgeyi kapsayan tek bir ölçek kümesi oluşturmanızı sağlar. Sanal makine örnekleri oluşturulduğunda varsayılan olarak, eşit olarak bölgeler arasında dengelenir. Bir kesintinin bölgelerinden biri oluşursa, bir ölçek kümesini otomatik olarak kapasitesini artırmak için ölçeği değil. CPU veya bellek kullanımına göre otomatik ölçeklendirme kurallarını yapılandırmak için en iyi yöntem olacaktır. Otomatik ölçeklendirme kurallarını ölçek kümesindeki VM örnekleri, bir bölgede kaybı kalan çalışma alanlarında yeni örnekleri genişletme yanıt çalıştırmasına olanak tanır.

Kullanılabilirlik alanları kullanmak için ölçek kümeniz oluşturulmalıdır bir [desteklenen bir Azure bölgesinde](../availability-zones/az-overview.md#regions-that-support-availability-zones). Kullanılabilirlik alanları aşağıdaki yöntemlerden biriyle kullanan bir ölçek kümesi oluşturabilirsiniz:

- [Azure portal](#use-the-azure-portal)
- Azure CLI
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Bir kullanılabilirlik alanı kullanan bir ölçek kümesi oluşturma işlemi ayrıntılı olarak aynıdır [makale Başlarken](quick-create-portal.md). Desteklenen bir Azure bölgesine seçtiğinizde, aşağıdaki örnekte gösterildiği gibi bir ölçek kümesi bir veya daha fazla kullanılabilir bölgelerde oluşturabilirsiniz:

![Bir ölçek kümesi tek bir kullanılabilirlik alanında oluşturun](media/virtual-machine-scale-sets-use-availability-zones/vmss-az-portal.png)

Ölçek kümesi ve Azure yük dengeleyici ve genel IP adresi gibi kaynakları destekleyen, belirttiğiniz tek bir bölgede oluşturulur.

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Bir kullanılabilirlik alanı kullanan bir ölçek kümesi oluşturma işlemi ayrıntılı olarak aynıdır [makale Başlarken](quick-create-cli.md). Kullanılabilirlik alanları kullanmak için desteklenen bir Azure bölgesinde ölçek kümenizi oluşturmanız gerekir.

Ekleme `--zones` parametresi [az vmss oluşturma](/cli/azure/vmss) kullanmak için hangi bölgeyi belirtin ve komutu (bölge gibi *1*, *2*, veya *3*). Aşağıdaki örnekte adlı bir tek bölgeli ölçek kümesi oluşturur *myScaleSet* bölgesinde *1*:

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

Tam bir örnek bir tek bölgeli ölçek kümesi ve ağ kaynakları için bkz [Bu örnek CLI betiği](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh)

### <a name="zone-redundant-scale-set"></a>Bölgesel olarak yedekli ölçek kümesi

Bölgesel olarak yedekli ölçek oluşturmak için kullandığınız bir *standart* SKU genel IP adresi ve yük dengeleyici. Gelişmiş artıklık *standart* SKU bölgesel olarak yedekli ağ kaynakları oluşturur. Daha fazla bilgi için [Azure Load Balancer Standard genel bakış](../load-balancer/load-balancer-standard-overview.md) ve [standart Load Balancer ve kullanılabilirlik bölgeleri](../load-balancer/load-balancer-standard-availability-zones.md).

Bölgesel olarak yedekli ölçek kümesi oluşturmak için birden fazla bölge ile belirtin `--zones` parametresi. Aşağıdaki örnekte adlı bir bölgesel olarak yedekli ölçek kümesi oluşturur *myScaleSet* bölgelerindeki *1,2,3*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1 2 3
```

Bu, oluşturmak ve tüm kaynaklarının ve VM'lerin belirttiğiniz bölgeleri içinde ölçek kümesi yapılandırmak için birkaç dakika sürer. Tam bir örnek bir bölgesel olarak yedekli ölçek kümesi ve ağ kaynakları için bkz [Bu örnek CLI betiği](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Kullanılabilirlik alanları kullanmak için desteklenen bir Azure bölgesinde ölçek kümenizi oluşturmanız gerekir. Ekleme `-Zone` parametresi [yeni AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) kullanmak için hangi bölgeyi belirtin ve komutu (bölge gibi *1*, *2*, veya *3*).

Aşağıdaki örnekte adlı bir tek bölgeli ölçek kümesi oluşturur *myScaleSet* içinde *Doğu ABD 2* bölge *1*. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1"
```

### <a name="zone-redundant-scale-set"></a>Bölgesel olarak yedekli ölçek kümesi

Bölgesel olarak yedekli ölçek kümesi oluşturmak için birden fazla bölge ile belirtin `-Zone` parametresi. Aşağıdaki örnekte adlı bir bölgesel olarak yedekli ölçek kümesi oluşturur *myScaleSet* arasında *Doğu ABD 2* bölgeleri *1, 2, 3*. Sanal ağ, genel IP adresi ve yük dengeleyici için bölgesel olarak yedekli Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1", "2", "3"
```

## <a name="use-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Bir kullanılabilirlik alanı kullanan bir ölçek kümesi oluşturmak için aynı başlangıç makalesinde için ayrıntılı olarak işlemidir [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md). Kullanılabilirlik alanları kullanmak için desteklenen bir Azure bölgesinde ölçek kümenizi oluşturmanız gerekir. Ekleme `zones` özelliğini *Microsoft.Compute/virtualMachineScaleSets* kaynak türü, şablonunuzda ve kullanmak için hangi bölgeyi belirtin (bölge gibi *1*, *2*, veya *3*).

Aşağıdaki örnekte adlı bir Linux tek bölgeli ölçek kümesi oluşturur *myScaleSet* içinde *Doğu ABD 2* bölge *1*:

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

Tam bir örnek bir tek bölgeli ölçek kümesi ve ağ kaynakları için bkz [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json)

### <a name="zone-redundant-scale-set"></a>Bölgesel olarak yedekli ölçek kümesi

Bölgesel olarak yedekli ölçek kümesi oluşturmak için birden çok değeri nasıl belirtin `zones` özelliği *Microsoft.Compute/virtualMachineScaleSets* kaynak türü. Aşağıdaki örnekte adlı bir bölgesel olarak yedekli ölçek kümesi oluşturur *myScaleSet* arasında *Doğu ABD 2* bölgeleri *1,2,3*:

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

Genel bir IP adresi veya bir yük dengeleyici oluşturursanız belirtin *"sku": {"name": "Standart"} "* bölgesel olarak yedekli ağ kaynakları oluşturma özelliği. Ayrıca, bir ağ güvenlik grubu ve kuralları tüm trafiğe izin verecek şekilde oluşturmanız gerekir. Daha fazla bilgi için [Azure Load Balancer Standard genel bakış](../load-balancer/load-balancer-standard-overview.md) ve [standart Load Balancer ve kullanılabilirlik bölgeleri](../load-balancer/load-balancer-standard-availability-zones.md).

Tam bir örnek bir bölgesel olarak yedekli ölçek kümesi ve ağ kaynakları için bkz [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json)

## <a name="next-steps"></a>Sonraki adımlar

Bir ölçek kümesindeki bir kullanılabilirlik alanında oluşturdunuz, öğrenebilirsiniz nasıl [uygulamaları sanal makineye dağıtma ölçek kümeleri](tutorial-install-apps-cli.md) veya [sanal makine ölçek kümeleri ile otomatik ölçeklendirmeyi kullanma](tutorial-autoscale-cli.md).
