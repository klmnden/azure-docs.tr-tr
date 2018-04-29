---
title: Bir sanal makine ağ yönlendirme sorunu - Azure CLI tanılamak | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi sonraki atlama kapasitesini kullanan bir sanal makine ağ yönlendirme bir sorunu tanılamak öğrenin.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: fcb7ec2e40b5c0e8794d2f4d70395dcbecca019c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Bir sanal makine ağ yönlendirme sorunu - Azure CLI tanılama

Bu makalede, bir sanal makine (VM) dağıtma ve bir IP adresi ve URL iletişimler denetleyin. Bir iletişim hatası ve nasıl giderebileceğiniz nedenini belirleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.28 çalıştırmasını gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). CLI Sürüm doğruladıktan sonra çalıştırmak `az login` Azure ile bir bağlantı oluşturmak için. Bu makalede CLI komutları, bir Bash kabuğunda çalıştırmak için biçimlendirilir.

## <a name="create-a-vm"></a>VM oluşturma

Bir VM oluşturmadan önce VM içerecek şekilde bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Kalan adımlar ile VM oluşturulur ve çıkış CLI döndürür kadar devam yok.

## <a name="test-network-communication"></a>Test ağ iletişimi

Ağ iletişimi Ağ İzleyicisi ile test etmek için önce test etmek istediğiniz VM'nin bulunduğu bölgede bulunan bir Ağ İzleyicisi etkinleştirin ve iletişim test etmek için Ağ İzleyicisi'nin sonraki atlama yetenek kullanmanız gerekir.

### <a name="enable-network-watcher"></a>Ağ İzleyicisi'ni etkinleştir

Doğu ABD bölgesinde etkin bir Ağ İzleyicisi zaten varsa, geçin [kullanım sonraki atlama](#use-next-hop). Kullanım [az Ağ İzleyicisi'ni yapılandırma](/cli/azure/network/watcher#az-network-watcher-configure) komutu Doğu ABD bölgesinde bir Ağ İzleyicisi oluşturmak için:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Sonraki atlama kullanın

Azure varsayılan hedeflere yolları otomatik olarak oluşturur. Geçersiz kılma varsayılan yolların özel yollar oluşturabilir. Bazı durumlarda, özel yollar iletişimi başarısız olmasına neden olabilir. Bir sanal makineden yönlendirme sınamak için kullanın [az Ağ İzleyicisi Göster sonraki atlama](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) trafiği için belirli bir adresi hedefleyen zaman sonraki yönlendirme atlama belirlemek için.

Test giden iletişim VM'den www.bing.com IP adreslerinden biri:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Birkaç saniye sonra çıktısı size bildirir, **nextHopType** olan **Internet**ve **routeTableId** olan **sistem yolu**. Bu sonuç, geçerli bir hedef yolu olduğunu bilmenizi sağlar.

Sanal makineden giden iletişim 172.31.0.100 test edin:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Döndürülen çıktının size bildirir, **hiçbiri** olan **nextHopType**ve **routeTableId** de **sistem yolu**. Bu sonucu, hedef bir geçerli sistem yolu olsa da, olduğundan, hedef trafiği yönlendirmek için bir sonraki atlama bilmenizi sağlar.

## <a name="view-details-of-a-route"></a>Bir rota ayrıntılarını görüntüleme

Daha fazla yönlendirme çözümlemek için sahip ağ arabirimi için etkili rotaları gözden [az ağ NIC Göster-etkin-yol-tablosu](/cli/azure/network/nic#az-network-nic-show-effective-route-table) komutu:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

Aşağıdaki metni döndürülen çıktısında eklenmiştir:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Kullanıldığında, `az network watcher show-next-hop` 13.107.21.200 içinde giden iletişim test etmek için komut [kullanım sonraki atlama](#use-next-hop), rota ile **addressPrefix** 0.0.0.0/0** adresine beri trafiği yönlendirmek için kullanıldı çıktıda başka bir yol yok adresini içerir. Varsayılan olarak, başka bir yol adres ön ekiyle belirtilmemiş tüm adresleri internet'e yönlendirilir.

Kullanıldığında, `az network watcher show-next-hop` 172.31.0.100 giden iletişim ancak test etmek için komut, hiçbir sonraki atlama türü olduğunu haberdar sonucu. Döndürülen çıktısında, ayrıca aşağıdaki metni görürsünüz:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

Cmdlet çıktısının gördüğünüz `az network watcher nic show-effective-route-table` 172.31.0.100 içerir 172.16.0.0/12 önek varsayılan rotaya olmasına rağmen komut adresi **nextHopType** olan **hiçbiri**. Azure 172.16.0.0/12 için varsayılan bir yol oluşturur, ancak bir nedeni kadar bir sonraki atlama türü belirtmiyor. Örneğin, 172.16.0.0/12 adres aralığını sanal ağın adres alanı eklediyseniz, Azure değiştirir **nextHopType** için **sanal ağ** rota için. Bir onay sonra göstermeniz **sanal ağ** olarak **nextHopType**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturulur ve sanal makineden ağ yönlendirme tanı koydu. Azure birkaç varsayılan yol oluşturur ve iki farklı hedefe yönlendirme test öğrendiniz. Daha fazla bilgi edinmek [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve nasıl [özel yollar oluşturmayı](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Giden VM bağlantılarında, gecikme süresini de belirleyebilirsiniz ve izin verilen ve VM Ağ İzleyicisi'nin kullanarak bir uç nokta arasındaki ağ trafiğini reddedildi [bağlantı sorunlarını giderme](network-watcher-connectivity-cli.md) yeteneği. Ağ İzleyicisi Bağlantı İzleyicisi özelliği kullanarak zamanla VM URL veya bir IP adresi gibi bir uç nokta arasındaki iletişimi izleyebilirsiniz. Bilgi edinmek için bkz [bir ağ bağlantısı izlemek](connection-monitor.md).