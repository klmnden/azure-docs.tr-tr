---
title: Bir sanal makine ağ yönlendirme sorunu - Azure CLI tanılama | Microsoft Docs
description: Bu makalede, Azure Ağ İzleyicisi sonraki atlama özelliğini kullanarak bir sanal makine ağ yönlendirme bir problemi tanılamaya öğrenin.
services: network-watcher
documentationcenter: network-watcher
author: KumudD
manager: twooley
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
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 968b7dd703ba40f46a068deb1d8b7d2b32e0de2b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688209"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Bir sanal makine ağ yönlendirme sorunu - Azure CLI tanılama

Bu makalede, bir sanal makine (VM) dağıtın ve ardından bir IP adresi ve URL iletişimler olup olmadığını denetleyin. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI 2.0.28 çalıştırdığınız gerekir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). CLI sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `az login` komutunu çalıştırın. Bu makalede CLI komutları bir Bash kabuğunda çalıştırmak için biçimlendirilir.

## <a name="create-a-vm"></a>VM oluşturma

Sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Aşağıdaki örnek, *myVm* adlı bir sanal makine oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulup CLI çıktı döndürünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimi test etmek için önce test etmek istediğiniz VM'nin bulunduğu bölgede bir Ağ İzleyicisi'ni etkinleştirin ve ardından iletişimi test etme için Ağ İzleyicisi'nin sonraki atlama özelliği kullanın.

### <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde etkin bir Ağ İzleyicisi zaten varsa, atlamak [kullanım sonraki atlama](#use-next-hop). Kullanım [az Ağ İzleyicisi'ni yapılandırma](/cli/azure/network/watcher#az-network-watcher-configure) komutunu bir Ağ İzleyicisi Doğu ABD bölgesinde oluşturun:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Sonraki atlamayı kullanma

Azure, varsayılan hedeflerin yollarını otomatik olarak oluşturur. Varsayılan yolları geçersiz kılmak için özel yollar oluşturabilirsiniz. Bazı durumlarda, özel yollar iletişimin başarısız olmasına neden olabilir. Bir sanal makineden yönlendirmeyi test etmek için [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) yönlendirme sonraki atlama belirlerken trafiği için belirli bir adresi yönlendirilir.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Birkaç saniye sonra çıktı size bildirir, **nextHopType** olduğu **Internet**ve **routeTableId** olduğu **sistem yolu**. Bu sonucu, geçerli bir hedef yolu olduğunu bilmenizi sağlar.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Döndürülen çıktının size bildirir, **hiçbiri** olduğu **nextHopType**ve **routeTableId** de **sistem yolu**. Bu sonuç, hedefin geçerli bir sistem yolu olmasına rağmen trafiği hedefe yönlendiren bir sonraki atlama olmadığını size bildirir.

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

Daha fazla yönlendirme analiz etmek için ağ arabirimi için geçerli rotalar gözden [az network nic show-etkin-yönlendirme-tablosunu](/cli/azure/network/nic#az-network-nic-show-effective-route-table) komutu:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

Aşağıdaki metni döndürülen çıkışında yer almaktadır:

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

Kullanıldığında, `az network watcher show-next-hop` giden iletişimi 13.107.21.200 içinde test etmek için komut [kullanım sonraki atlama](#use-next-hop), rota ile **addressPrefix** 0.0.0.0/0** adresine beri trafiği yönlendirmek için kullanıldı çıktıda başka bir yolun adres içerir. Varsayılan olarak, başka bir yolun adres ön ekinde belirtilmeyen tüm adresler İnternet'e yönlendirilir.

Kullanıldığında, `az network watcher show-next-hop` 172.31.0.100 giden iletişimi ancak test etmek için komut, sonraki atlama türü yok edildi sonucu haberdar. Döndürülen çıktısında, ayrıca aşağıdaki metni görürsünüz:

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

Çıkışta gördüğünüz gibi `az network watcher nic show-effective-route-table` komutu 172.31.0.100 içerir 172.16.0.0/12 önekini varsayılan bir yolu olsa adresi **nextHopType** olduğu **hiçbiri**. Azure, 172.16.0.0/12 için varsayılan bir yol oluşturur ancak bir neden olmadıkça sonraki atlama türünü belirtmez. Örneğin, 172.16.0.0/12 adres aralığını sanal ağın adres alanına eklediyseniz, Azure değişiklikleri **nextHopType** için **sanal ağ** rota. Bir onay ardından gösterebilir **sanal ağ** olarak **nextHopType**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturulur ve sanal makineden ağ yönlendirme tanı koydu. Azure’un birkaç varsayılan yol oluşturduğunu öğrendiniz ve iki farklı hedefin yolunu test ettiniz. [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [özel yollar oluşturma](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route) hakkında daha fazla bilgi edinin.

Giden sanal makine bağlantılarında, gecikme süresini de belirleyebilirsiniz ve izin verilen ve VM Ağ İzleyicisi'nin kullanarak bir uç nokta arasındaki ağ trafiğini reddedildi [bağlantı sorunlarını giderme](network-watcher-connectivity-cli.md) yeteneği. Ağ İzleyicisi Bağlantı İzleyicisi özelliğini kullanarak zaman içinde bir VM ile URL veya bir IP adresi gibi bir uç noktası arasındaki iletişimi izleyebilirsiniz. Bilgi edinmek için bkz [Ağ Bağlantı İzleyicisi](connection-monitor.md).