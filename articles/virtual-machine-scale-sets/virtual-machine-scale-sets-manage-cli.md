---
title: "Sanal makine ölçek kümeleri Azure CLI 2.0 ile yönetme | Microsoft Docs"
description: "Sanal makine ölçek kümeleri örneğini durdurmak ve başlatmak nasıl gibi yönetmek veya ölçeği değiştirmek için ortak Azure CLI 2.0 komutları kapasite ayarlayın."
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
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 5686d8bd3f9817be2308583afe778e0615154580
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Azure CLI 2.0 ile ayarlanmış bir sanal makine ölçek yönetme
Bir sanal makine ölçek kümesi yaşam döngüsü boyunca, bir veya daha fazla yönetim görevleri çalıştırmanız gerekebilir. Ayrıca, çeşitli yaşam döngüsü görevleri otomatikleştiren komut dosyaları oluşturmak isteyebilirsiniz. Bu makalede bu görevleri gerçekleştirmenize olanak sağlayan ortak Azure CLI 2.0 komutları bazıları ayrıntılarını verir.

Bu yönetim görevleri tamamlamak için en son Azure CLI 2.0 yapı gerekir. Yükleme ve en son sürümünü kullanma hakkında daha fazla bilgi için bkz: [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Bir sanal makine ölçek kümesi oluşturmanız gerekiyorsa, yapabilecekleriniz [ölçeği Azure portalında Ayarla oluşturma](virtual-machine-scale-sets-portal-create.md).


## <a name="view-information-about-a-scale-set"></a>Ölçek kümesi hakkında bilgi görüntüleyin
Ölçek kümesi hakkındaki genel bilgileri görüntülemek için kullanın [az vmss Göster](/cli/azure/vmss#show). Aşağıdaki örnek ölçeği adlandırılmış Ayarla hakkındaki bilgileri alır *myScaleSet* içinde *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Görünüm VM ölçek kümesindeki
Ölçek kümesindeki VM örneği listesini görüntülemek için kullanın [az vmss listesi-örneklerini](/cli/azure/vmss#list-instances). Aşağıdaki örnek ölçeği adlandırılmış Ayarla tüm VM örnekleri listesi *myScaleSet* içinde *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi girin:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

Belirli bir VM örneği hakkında ek bilgi görüntülemek için add `--instance-id` parametresi [az vmss get-örnek-görünümü](/cli/azure/vmss#get-instance-view) ve görüntülemek için bir örnek seçin. Aşağıdaki örnek görünümleri VM örneği hakkında bilgi *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Aşağıdaki gibi kendi adlarınızı girin:

```azurecli
az vmss get-instance-view \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>VM'ler için liste bağlantı bilgileri
VM ölçek kümesindeki, SSH ya da bir atanan ortak IP adresi ve bağlantı noktası numarası için RDP bağlanmak için. Varsayılan olarak, her bir VM uzak bağlantı trafiğini Azure yük dengeleyici ağ adresi çevirisi (NAT) kuralları eklenir. Ölçek kümesi VM örnekleri bağlanmak için bağlantı noktaları ve adres listelemek için kullanın [az vmss listesi-örnek-bağlantı-bilgisi](/cli/azure/vmss#list-instance-connection-info). Aşağıdaki örnek listesi bağlantı bilgilerini VM örnekleri adlandırılmış kümesi ölçek *myScaleSet* ve *myResourceGroup* kaynak grubu. Bu adları için kendi değerlerinizi girin:

```azurecli
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesi kapasitesi değiştirme
Yukarıdaki komutlar, Ölçek kümesi ve VM örnekleri hakkında bilgi gösterdi. Artırmak veya ölçek kümesindeki örneklerinin sayısını azaltmak için kapasite değiştirebilirsiniz. Ölçek kümesi oluşturur veya VM'ler gereken sayıda kaldırır ve sonra uygulama trafiği almaya VM'ler yapılandırır.

Ölçek kümesindeki şu anda sahip örneklerinin sayısını görmek için [az vmss Göster](/cli/azure/vmss#show) ve sorgulayın *sku.capacity*:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Daha sonra el ile artırabilir veya sanal makine ölçek kümesi sayısını azaltmak [az vmss ölçek](/cli/azure/vmss#scale). Aşağıdaki örnek VM'lerin sayısını ayarlamak, Ölçek ayarlar *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Kapasite, Ölçek güncelleştirilmesi birkaç dakika bir ayarlarsanız. Bir ölçek kapasitesini azaltırsanız kimlikleri ilk kaldırılır en yüksek örnek VM'lerin ayarlayın.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Durdurun ve ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya daha fazla sanal makineleri durdurmak için kullanma [az vmss Dur](/cli/azure/vmss/stop). `--instance-ids` Parametresi durdurmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler durdurulur. Birden çok VM durdurmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek örneği durdurur *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Durdurulmuş VM'ler ayrılmış kalır ve bilgi işlem ücretleri uygulanmaya devam eder. Bunun yerine bırakılmasına VM'ler istiyor ve yalnızca depolama ücretleri kullanın [az vmss ayırması](/cli/azure/vmss#deallocate). Birden çok VM serbest bırakma için her örnek kimliği boşlukla ayırın. Aşağıdaki örnek durdurur ve örnek kaldırır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Başlat
Ölçek kümesindeki bir veya daha fazla VM başlatmak için kullanmak [az vmss Başlat](/cli/azure/vmss#start). `--instance-ids` Parametresi başlatmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek kümesindeki tüm VM'ler başlatılır. Birden çok VM başlatmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnekte bir örneğini başlatır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Ölçek kümesindeki sanal makineleri yeniden başlatın
Bir veya daha fazla VM ölçek kümesindeki yeniden başlatmak için kullanın [az vmss yeniden](/cli/azure/vmss#restart). `--instance-ids` Parametresi yeniden başlatmak için bir veya daha fazla VM belirtmenize olanak verir. Bir örnek kimliği belirtmezseniz, Ölçek grubundaki tüm sanal makineleri yeniden başlatılır. Birden çok VM yeniden başlatmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek örneği yeniden *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>Ölçek kümesindeki sanal makineleri Kaldır
Bir veya daha fazla VM ölçek kümesindeki kaldırmak için kullanın [az vmss silme-örnekleri](/cli/azure/vmss#delete-instances). '--Örneği kimlikleri '' parametresi kaldırmak için bir veya daha fazla VM belirtmenize olanak verir. Belirtirseniz * için örnek kimliği, Ölçek kümesindeki tüm VM'ler kaldırılır. Birden çok VM kaldırmak için her örnek kimliği boşlukla ayırın.

Aşağıdaki örnek, örnek kaldırır *0* adlandırılmış kümesi ölçeğinde *myScaleSet* ve *myResourceGroup* kaynak grubu. Değerlerinizi aşağıdaki gibi belirtin:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Sonraki adımlar
Diğer ortak görevler ölçek kümeleri için nasıl [bir uygulamayı dağıtmak](virtual-machine-scale-sets-deploy-app.md), ve [yükseltme VM örnekleri](virtual-machine-scale-sets-upgrade-scale-set.md). Azure CLI için de kullanabilirsiniz [otomatik ölçeklendirme kurallarını yapılandırma](virtual-machine-scale-sets-autoscale-overview.md).
