---
title: Kullanılabilirlik bölgeleri kullanan bir Azure ölçek kümesi oluşturma | Microsoft Docs
description: Kesintilere karşı artıklığı için kullanılabilirlik bölgeleri kullanmak Azure sanal makine ölçek kümeleri oluşturma hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: iainfou
ms.openlocfilehash: 70dff848ce0674461749075e6498741c3eb9a381
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Kullanılabilirlik bölgeleri kullanan bir sanal makine ölçek kümesi oluşturma

Sanal makine ölçek kümeleri datacenter düzeyi arızasına karşı korumak için kullanılabilirlik dilimlerinde ayarlamak ölçek oluşturabilirsiniz. Kullanılabilirlik bölgeleri destekleyen azure bölgeleri sahip en az üç ayrı bölgeler, her biri kendi bağımsız güç kaynağı, ağ ve soğutma. Daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](../availability-zones/az-overview.md).

## <a name="availability-considerations"></a>Kullanılabilirlik konusunda dikkat edilmesi gerekenler

Ölçeği API sürümü'dan sonra bir veya daha fazla bölgelere Ayarla dağıttığınızda *2017-12-01*, "max yayılmak" veya "statik 5 hata etki alanı yayılmak" ile dağıtmak için seçeneğiniz vardır. Etki alanları sayıda her bölge içinde mümkün olduğunca arıza max yayılmak ile ölçek kümesini Vm'leriniz arasında yayar. Üzerinden bu yayılmak olabilir büyük veya daha az beş bölge başına etki alanı arıza. Ölçek kümesi "statik 5 hata etki alanı yayılmak ile", bölge başına tam olarak beş hata etki alanları arasında Vm'leriniz yayar. Ölçek kümesini ayırma isteği karşılamak için bölge başına beş farklı hata etki alanlarını bulamazsanız, istek başarısız olur.

**Çoğu iş yükleri için en fazla yayılmasını ile dağıtma öneririz**gibi en iyi yayılmak çoğu durumda bu yaklaşım sağlar. Farklı donanım yalıtım birimleri yayılmaya çoğaltmaları gerekiyorsa, biz kullanılabilirlik dilimlerinde yayılmak önerilir ve her bölgede en fazla yayılmasını kullanma.

Max yayılmak ile bir hata etki alanı ölçek kümesi VM örnek görünümünü ve VM'ler arasında yayılır kaç hata etki alanlarını bağımsız olarak örneği meta verileri yalnızca bakın. Her bölge içinde yayılmak örtük.

Max yayılmak kullanmak üzere ayarlanmış *platformFaultDomainCount* için *1*. Statik beş hata etki alanı yayılmak kullanmak üzere ayarlanmış *platformFaultDomainCount* için *5*. API sürümünde *2017-12-01*, *platformFaultDomainCount* varsayılan olarak *1* tek bölge ve çapraz bölge ölçek için ayarlar. Şu anda yalnızca statik beş hata etki alanı yayılmak bölgesel ölçek kümeleri için desteklenir.

### <a name="placement-groups"></a>Yerleştirme grupları

Ölçek kümesini dağıttığınızda, tek bir tıklatmayla dağıtmak için seçeneğiniz de [yerleştirme grup](./virtual-machine-scale-sets-placement-groups.md) kullanılabilirlik bölge başına veya bölge başına birden çok ile. Bölgesel ölçek kümeleri için bölgede bir tek yerleştirme grubunuz için veya birden çok bölgede seçimdir. Çoğu iş yükleri için birden çok yerleştirme grupları için büyük ölçek sağlayan öneririz. API sürümünde *2017-12-01*ölçek tek bölge ve çapraz bölge ölçek kümeleri için birden çok yerleştirme grupları için varsayılan ayarlar, ancak bunlar bölgesel ölçek kümeleri için tek yerleştirme grubuna varsayılan.

> [!NOTE]
> Max yayılmak kullanırsanız, birden çok yerleştirme gruplarını kullanmanız gerekir.

### <a name="zone-balancing"></a>Dengeleme bölge

Son olarak, birden çok dilimlerinde dağıtılan ölçek kümeleri için aynı zamanda "en iyi çaba bölge balance" veya "katı bölge balance" seçme seçeneğiniz vardır. Her bölge içindeki VM'ler VM ölçek kümesi için tüm diğer bölgelerdeki sayısını biri içinde ise bir ölçek kümesini "dengeli" olarak kabul edilir. Örneğin:

- Bölge 1, 2 ve 3 VM'ler bölgesinde 3 olarak kabul bölgedeki 3 VM ile 2 VM kümesindeki bir ölçek dengeli.
- Bölge 1 ile 1 VM kümesi ölçek, 2 ve 3 VM'ler bölgesinde 3 olarak kabul bölgedeki 3 VM dengesini.

Ölçek kümesindeki sanal makineleri başarıyla oluşturuldu, ancak dağıtmak bu vm'lerde uzantıları başarısız mümkündür. Uzantı hataları olan bu Vm'lere hala ölçek kümesini dengelenir belirlerken dikkate alınır. Örneği için bölge 1, 3 VM'ler bölgesinde 2 ve 3 bölgedeki 3 VM ile 3 VM kümesindeki bir ölçek 1 bölgesinde tüm uzantıları başarısız olsa bile dengeli kabul edilir ve tüm uzantıları 2 ve 3 bölgelerinde başarılı oldu.

En yüksek çaba bölge Bakiye ile ölçek Bakiye korurken ölçeğini girişimleri ayarlayın. Ancak, herhangi bir nedenden dolayı bu mümkün değilse (bir bölge devre dışı kalırsa, örneğin, Ölçek kümesi yeni bir VM bu bölgede oluşturulamıyor.), başarılı bir şekilde içeri veya dışarı ölçeklendirmek geçici dengesizliği ölçek kümesi sağlar. Sonraki genişleme denemeler hakkında daha fazla sanal makineleri dengelenmeye ayarlamak için ölçek ihtiyacınız bölgeler VM'ler ölçek kümesi ekler. Benzer şekilde, deneme, sonraki ölçekte ölçek kümesini VM'ler daha az VM'ler dengelenmeye ayarlamak için ölçek ihtiyacınız bölgeler kaldırır. "Katı bölge balance" ile ölçek kümesini bunun nedenle unbalance neden olacaksa içeri veya dışarı ölçeklendirme girişimleri başarısız olur.

En yüksek çaba bölge Bakiye kullanmak üzere ayarlanmış *zoneBalance* için *false*. Bu API sürümü, varsayılan ayardır *2017-12-01*. Katı bölge Bakiye kullanmak üzere ayarlanmış *zoneBalance* için *doğru*.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Tek bölge ve bölge olarak yedekli ölçek kümeleri

Bir sanal makine ölçek kümesi dağıttığınızda, tek bir kullanılabilirlik bölge bir bölgede ya da birden fazla bölge kullanmayı seçebilirsiniz.

Tek bir bölge, bölge bu tüm VM örnekleri çalıştırmak ve ölçek kümesini yönetilen kontrol ve yalnızca o bölge içinde autoscales kümesindeki bir ölçek oluşturduğunuzda. Bölge olarak yedekli ölçek birden çok bölgelere yayılan bir tek ölçek kümesi oluşturmanıza olanak tanır. VM örnekleri oluşturuldukça, varsayılan olarak bunlar eşit dilimlerinde dengeli. Bir kesinti bölgelerinden biri gerçekleşeceğini, Ölçek kümesini otomatik olarak kullanıma kapasitesini artırmak için ölçeklenmez. CPU veya bellek kullanımına dayalı otomatik ölçeklendirme kurallarını yapılandırmak için en iyi uygulama olacaktır. Otomatik ölçeklendirme kurallarını ölçeği bir bölge VM örnekleri kaybı kalan işletimsel bölgelerde yeni örnekleri ölçeğini yanıt verecek şekilde ayarla olanak tanır.

Kullanılabilirlik bölgeleri kullanmak için ölçek kümesini oluşturulmalıdır bir [Azure bölgesi desteklenen](../availability-zones/az-overview.md#regions-that-support-availability-zones). Aşağıdaki yöntemlerden biriyle kullanılabilirlik bölgeleri kullanan bir ölçek kümesi oluşturabilirsiniz:

- [Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Azure Resource Manager şablonları](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](quick-create-portal.md). Bilgisayarınızda yüklü olduğundan emin olun [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll). Desteklenen bir Azure bölgesini seçtiğinizde, aşağıdaki örnekte gösterildiği gibi bir ölçeği kullanılabilen bölgeler, birini Ayarla oluşturabilirsiniz:

![Ölçeği tek bir kullanılabilirlik bölgesinde Ayarla oluşturma](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Ölçek kümesi ve Azure yük dengeleyici ve genel IP adresi gibi kaynakları destekleyen belirttiğiniz tek bir bölge içinde oluşturulur.

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanın

Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturmak için işlem içinde ayrıntılı olarak aynıdır [makale Başlarken](quick-create-cli.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ölçek kümesini oluşturmak ve gerekir sahip [kullanılabilirlik bölgeleri Önizleme için kayıtlı](http://aka.ms/azenroll).

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

Bölge olarak yedekli ölçek oluşturmak için kullandığınız bir *standart* SKU ortak IP adresi ve yük dengeleyici. Gelişmiş artıklığı *standart* SKU bölge olarak yedekli ağ kaynaklarına oluşturur. Daha fazla bilgi için bkz: [Azure yük dengeleyici standart genel bakış](../load-balancer/load-balancer-standard-overview.md).

Bölge olarak yedekli ölçek kümesi oluşturmak için birden çok bölgede belirtin `--zones` parametresi. Aşağıdaki örnek, bir bölge olarak yedekli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* bölgeler arasında *1,2,3*:

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

Oluşturun ve tüm belirttiğiniz bölgelerini kaynaklar ve VM ölçek kümesi yapılandırmak için birkaç dakika sürer. Bölge olarak yedekli ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek CLI betik](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ayarlayın, Ölçek oluşturmanız gerekir. Ekleme `-Zone` parametresi [yeni AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) komut ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*).

Aşağıdaki örnek, bir tek bölge ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *Doğu ABD 2* bölge *1*. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzureRmVmss `
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

### <a name="zone-redundant-scale-set"></a>Bölge olarak yedekli ölçek kümesi

Bölge olarak yedekli ölçek kümesi oluşturmak için birden çok bölgede belirtin `-Zone` parametresi. Aşağıdaki örnek, bir bölge olarak yedekli ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* arasında *Doğu ABD 2* bölgeleri *1, 2, 3*. Bölge olarak yedekli Azure ağ kaynakları sanal ağ, genel IP adresi ve yük dengeleyici için otomatik olarak oluşturulur. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```powershell
New-AzureRmVmss `
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

Bir kullanılabilirlik bölge kullanan bir ölçek kümesi oluşturma işlemi için alma başlatılan makalesinde ayrıntılı olarak aynıdır [Linux](quick-create-template-linux.md) veya [Windows](quick-create-template-windows.md). Kullanılabilirlik bölgeleri kullanmak için desteklenen bir Azure bölgesinde ayarlayın, Ölçek oluşturmanız gerekir. Ekleme `zones` özelliğine *Microsoft.Compute/virtualMachineScaleSets* kaynak türü şablonunuzda ve kullanmak için hangi bölgede belirtin (bölge gibi *1*, *2*, veya *3*).

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

Tek bölge ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/zones/singlezone.json)

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

Bölge olarak yedekli ölçek tam bir örnek için ayarlamak ve ağ kaynakları, bkz: [Bu örnek Resource Manager şablonu](https://github.com/Azure/vm-scale-sets/blob/master/zones/multizone.json)

## <a name="next-steps"></a>Sonraki adımlar

Ölçeği bir kullanılabilirlik bölgesinde Ayarla oluşturduğunuza göre öğrenebilirsiniz nasıl [dağıtma uygulamaları sanal makineye ölçek kümeleri](tutorial-install-apps-cli.md) veya [ile sanal makine ölçek kümeleri otomatik ölçeklendirme kullanmak](tutorial-autoscale-cli.md).
