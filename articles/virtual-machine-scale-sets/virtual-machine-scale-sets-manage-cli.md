---
title: Sanal makine ölçek kümeleri Azure CLI ile yönetme | Microsoft Docs
description: Yaygın Azure CLI komutları örneğini durdurmak ve başlatmak gibi nasıl sanal makine ölçek kümeleri, yönetme veya azaltarak değiştirme kapasitesini ayarlayın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: cynthn
ms.openlocfilehash: b49182ebdcc93c4a51a55f27c3e0bf7a45307b7f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60618090"
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli"></a>Sanal makine ölçek kümesi Azure CLI ile yönetme
Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevlerini otomatikleştiren betikler oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak tanıyan ortak Azure CLI komutlarının bazıları ayrıntılı olarak açıklanmaktadır.

Bu yönetim görevleri tamamlamak için en son Azure CLI'yı gerekir. Bilgi için [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Bir sanal makine ölçek kümesi oluşturmak için ihtiyacınız varsa, [ölçek kümesi Azure CLI ile oluşturma](quick-create-cli.md).


## <a name="view-information-about-a-scale-set"></a>Bir ölçek kümesi hakkındaki bilgileri görüntüleme
Bir ölçek kümesi hakkında genel bilgileri görüntülemek için kullanın [az vmss show](/cli/azure/vmss). Aşağıdaki örnekte adlı ölçek kümesi hakkında bilgi alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Ölçek kümesindeki VM’leri görüntüleme
Bir ölçek kümesindeki sanal makine örneği listesini görüntülemek için kullanın [az vmss seznamu](/cli/azure/vmss). Aşağıdaki örnekte adlı ölçek kümesi içinde tüm VM örnekleri listelenmiştir *myScaleSet* içinde *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi sağlayın:

```azurecli
az vmss list-instances \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --output table
```

Belirli bir sanal makine örneği hakkında ek bilgi görüntülemek için Ekle `--instance-id` parametresi [az vmss get-instance-view](/cli/azure/vmss) ve görüntülemek için bir örnek belirtin. Aşağıdaki örnek, sanal makine örneği hakkında bilgi görüntüler *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Şu şekilde kendi adlarınızı girin:

```azurecli
az vmss get-instance-view \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>VM'ler için bağlantı bilgilerini listeleme
Bir ölçek kümesindeki, SSH veya RDP için bir atanan genel IP adresi ve bağlantı noktası numarası Vm'lerine bağlanmak için. Varsayılan olarak, her VM için uzaktan bağlantı trafiğini ileten Azure load balancer için ağ adresi çevirisi (NAT) kuralları eklenir. Bir ölçek kümesindeki sanal makine örneklerine bağlanacak bağlantı noktalarını ve adresi listelemek için kullanın [az vmss list-instance-bağlantı-info](/cli/azure/vmss). Aşağıdaki örnekte adlı ölçek kümesi VM örnekleri için bağlantı bilgilerini listeler *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi sağlayın:

```azurecli
az vmss list-instance-connection-info \
    --resource-group myResourceGroup \
    --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Yukarıdaki komutların ölçek kümenizi ve sanal makine örnekleri hakkında bilgi gösterdi. Artırabilir veya ölçek kümesindeki örneklerin sayısını azaltmak için kapasiteyi değiştirebilirsiniz. Ölçek kümesi oluşturur veya gerekli VM sayısını kaldırır ve ardından uygulama trafiği almak için sanal makineleri yapılandırır.

Ölçek kümesinde şu anda yer alan örneklerin sayısını görmek için [az vmss show](/cli/azure/vmss) komutunu kullanarak *sku.capacity* üzerinde bir sorgu çalıştırın:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Ardından [az vmss scale](/cli/azure/vmss) ile ölçek kümesindeki sanal makinelerin sayısını elle artırabilir veya azaltabilirsiniz. Aşağıdaki örnek ölçek kümenizdeki VM'lerin sayısını ayarlar *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk önce kaldırılır en yüksek örnek ile Vm'leri ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>VM ölçek kümesindeki durdurup
Bir ölçek kümesindeki bir veya daha fazla sanal makineleri durdurmak için kullanın [az vmss stop](/cli/azure/vmss#az-vmss-stop). `--instance-ids` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Birden çok VM durdurmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek örneği durdurur *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Durdurulmuş sanal makineler, ayrılmış şekilde kalır ve işlem ücretleri uygulanmaya devam eder. Bunun yerine serbest bırakılması Vm'leri istediğiniz ve yalnızca depolama ücreti, kullanmanız [az vmss deallocate](/cli/azure/vmss). Birden çok VM ayırması için her örnek kimliği boşlukla ayırın. Aşağıdaki örnek durdurur ve örnek kaldırır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Bir ölçek kümesindeki VM'lerin başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine başlatmak için kullanın [az vmss Başlat](/cli/azure/vmss). `--instance-ids` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Birden çok VM başlatmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek bir örneğini başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Bir ölçek kümesindeki Vm'leri yeniden başlatma
Bir ölçek kümesindeki bir veya daha fazla sanal makine yeniden başlatmak için kullanmak [az vmss yeniden](/cli/azure/vmss). `--instance-ids` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Birden çok VM'yi yeniden başlatmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek örneğini yeniden başlatır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>VM ölçek kümesinden Kaldır
Bir ölçek kümesindeki bir veya daha fazla sanal makine kaldırmak için [az vmss delete-instances](/cli/azure/vmss). `--instance-ids` Parametresi, kaldırmak için bir veya daha fazla sanal makine belirtmenize olanak sağlar. Belirtirseniz * kimliği, Ölçek kümesindeki tüm sanal makineler için örneği kaldırılır. Birden çok VM kaldırmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlı ölçek kümesi içinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Kendi değerlerinizi aşağıdaki gibi sağlayın:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Sonraki adımlar
Ölçek kümeleri için sık kullanılan diğer görevler nasıl [uygulama dağıtma](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme sanal makine örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure CLI'yı da kullanabilirsiniz [otomatik ölçeklendirme kuralları yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
