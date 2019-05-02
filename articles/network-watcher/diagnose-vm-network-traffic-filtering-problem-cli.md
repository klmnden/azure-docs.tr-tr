---
title: Sanal makine ağ trafiği filtreleme sorununu tanılama - hızlı başlangıç - Azure CLI | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Ağ İzleyicisi’nin IP akış doğrulama özelliği kullanılarak sanal makine ağ trafiği filtreleme sorununun nasıl tanılanacağını öğrenirsiniz.
services: network-watcher
documentationcenter: network-watcher
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose a virtual machine (VM) network traffic filter problem that prevents communication to and from a VM.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 460513e4818cbef8fca0cd1b84d69b3021afaab7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64690438"
---
# <a name="quickstart-diagnose-a-virtual-machine-network-traffic-filter-problem---azure-cli"></a>Hızlı Başlangıç: Bir sanal makine ağ trafik filtresi sorununu - Azure CLI tanılama

Bu hızlı başlangıçta, bir sanal makine (VM) dağıtır ve sonra bir IP adresi ve URL ile iletişimleri ve bir IP adresinden gelen iletişimleri denetlersiniz. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). CLI sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `az login` komutunu çalıştırın. Bu hızlı başlangıçtaki CLI komutları, bir Bash kabuğunda çalıştırılacak şekilde biçimlendirilir.

## <a name="create-a-vm"></a>VM oluşturma

Sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm) ile bir VM oluşturun. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Aşağıdaki örnek, *myVm* adlı bir sanal makine oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulup CLI çıktı döndürünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimini test etmek için önce test etmek istediğiniz sanal makinenin bulunduğu bölgede bir ağ izleyicisini etkinleştirmeniz ve sonra iletişimi test etmek için Ağ İzleyicisinin IP akışı doğrulama özelliğini kullanmanız gerekir.

### <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde etkinleştirilmiş bir ağ izleyicisi zaten varsa [IP akış doğrulamayı kullanma](#use-ip-flow-verify) bölümüne atlayın. Doğu ABD bölgesinde bir ağ izleyicisi oluşturmak için [az network watcher configure](/cli/azure/network/watcher#az-network-watcher-configure) komutunu kullanın:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-ip-flow-verify"></a>IP akışı doğrulamayı kullanma

Bir sanal makine oluşturduğunuzda Azure varsayılan olarak sanal makineye/sanal makineden ağ trafiğine izin verir veya ağ trafiğini reddeder. Daha sonra Azure’ın varsayılanlarını geçersiz kılarak ek trafik türlerine izin verebilir veya ek trafik türlerini reddedebilirsiniz. Farklı hedeflere giden trafiğe izin verilip verilmediğini veya farklı hedeflere giden trafiğin reddedilip reddedilmediğini test etmek için [az network watcher test-ip-flow](/cli/azure/network/watcher#az-network-watcher-test-ip-flow) komutunu kullanın.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurecli-interactive
az network watcher test-ip-flow \
  --direction outbound \
  --local 10.0.0.4:60000 \
  --protocol TCP \
  --remote 13.107.21.200:80 \
  --vm myVm \
  --nic myVmVMNic \
  --resource-group myResourceGroup \
  --out table
```

Birkaç saniye sonra döndürülen sonuç, **AllowInternetOutbound** adlı bir güvenlik kuralı tarafından erişime izin verildiğini size bildirir.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurecli-interactive
az network watcher test-ip-flow \
  --direction outbound \
  --local 10.0.0.4:60000 \
  --protocol TCP \
  --remote 172.31.0.100:80 \
  --vm myVm \
  --nic myVmVMNic \
  --resource-group myResourceGroup \
  --out table
```

Döndürülen sonuç, **DefaultOutboundDenyAll** adlı bir güvenlik kuralı tarafından erişimin reddedildiğini size bildirir.

172.31.0.100 adresinden sanal makineye gelen iletişimi test etme:

```azurecli-interactive
az network watcher test-ip-flow \
  --direction inbound \
  --local 10.0.0.4:80 \
  --protocol TCP \
  --remote 172.31.0.100:60000 \
  --vm myVm \
  --nic myVmVMNic \
  --resource-group myResourceGroup \
  --out table
```

Döndürülen sonuç, **DefaultInboundDenyAll** adlı bir güvenlik kuralı tarafından erişimin reddedildiğini size bildirir. Hangi güvenlik kurallarının bir sanal makineye/sanal makineden trafiğe izin verdiğini veya trafiği reddettiğini öğrendiğinize göre sorunların nasıl çözümleneceğini belirleyebilirsiniz.

## <a name="view-details-of-a-security-rule"></a>Bir güvenlik kuralının ayrıntılarını görüntüleme

[IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümündeki kuralların neden iletişime izin verdiğini veya iletişimi engellediğini belirlemek için [az network nic list-effective-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg) komutunu kullanarak ağ arabirimi için etkili güvenlik kurallarını gözden geçirin:

```azurecli-interactive
az network nic list-effective-nsg \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

Döndürülen çıktı, [IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde yer alan bir önceki adımda www.bing.com adresine giden erişime izin veren **AllowInternetOutbound** kuralı için aşağıdaki metni içerir:

```azurecli
{
 "access": "Allow",
 "additionalProperties": {},
 "destinationAddressPrefix": "Internet",
 "destinationAddressPrefixes": [
  "Internet"
 ],
 "destinationPortRange": "0-65535",
 "destinationPortRanges": [
  "0-65535"
 ],
 "direction": "Outbound",
 "expandedDestinationAddressPrefix": [
  "1.0.0.0/8",
  "2.0.0.0/7",
  "4.0.0.0/6",
  "8.0.0.0/7",
  "11.0.0.0/8",
  "12.0.0.0/6",
  ...
 ],
 "expandedSourceAddressPrefix": null,
 "name": "defaultSecurityRules/AllowInternetOutBound",
 "priority": 65001,
 "protocol": "All",
 "sourceAddressPrefix": "0.0.0.0/0",
 "sourceAddressPrefixes": [
  "0.0.0.0/0"
 ],
 "sourcePortRange": "0-65535",
 "sourcePortRanges": [
  "0-65535"
 ]
},
```

Önceki çıktıda **destinationAddressPrefix** öğesinin **İnternet** olduğunu görebilirsiniz. Ancak 13.107.21.200 adresinin **İnternet** ile arasındaki ilişkinin ne olduğu net değildir. **expandedDestinationAddressPrefix** bölümünde birçok adres önekinin listelendiğini görürsünüz. Listedeki öneklerden biri **12.0.0.0/6** olup bu, IP adreslerinin 12.0.0.1-15.255.255.254 aralığını kapsar. 13.107.21.200 bu adres aralığı içinde yer aldığından **AllowInternetOutBound** kuralı, giden trafiğe izin verir. Ayrıca, önceki çıktıda bu kuralı geçersiz kılan daha yüksek öncelikli (daha küçük numaralı) bir kural da gösterilmemektedir. Bir IP adresine giden iletişimi reddetmek için, IP adresine giden 80 numaralı bağlantı noktasını reddeden, daha yüksek önceliğe sahip bir güvenlik kuralı ekleyebilirsiniz.

[IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde 172.131.0.100 adresine giden iletişimi test etmek için `az network watcher test-ip-flow` komutunu çalıştırdığınızda elde edilen çıktı size **DefaultOutboundDenyAll** kuralının iletişimi reddettiğini bildirdi. **DefaultOutboundDenyAll** kuralı, `az network nic list-effective-nsg` komutundan elde edilen aşağıdaki çıktıda listelenen **DenyAllOutBound** kuralına karşılık gelir:

```azurecli
{
 "access": "Deny",
 "additionalProperties": {},
 "destinationAddressPrefix": "0.0.0.0/0",
 "destinationAddressPrefixes": [
  "0.0.0.0/0"
 ],
 "destinationPortRange": "0-65535",
 "destinationPortRanges": [
  "0-65535"
 ],
 "direction": "Outbound",
 "expandedDestinationAddressPrefix": null,
 "expandedSourceAddressPrefix": null,
 "name": "defaultSecurityRules/DenyAllOutBound",
 "priority": 65500,
 "protocol": "All",
 "sourceAddressPrefix": "0.0.0.0/0",
 "sourceAddressPrefixes": [
  "0.0.0.0/0"
 ],
 "sourcePortRange": "0-65535",
 "sourcePortRanges": [
  "0-65535"
 ]
}
```

Kural, **destinationAddressPrefix** olarak **0.0.0.0/0** değerini listeler. Adres, `az network nic list-effective-nsg` komutundan elde edilen çıktıdaki diğer giden kurallarının herhangi birinin **destinationAddressPrefix** değeri içinde olmadığından kural, 172.131.0.100 adresine giden iletişimi reddeder. Giden iletişime izin vermek için, 172.131.0.100 adresinde 80 numaralı bağlantı noktasına giden trafiğe izin veren daha yüksek öncelikli bir güvenlik kuralı ekleyebilirsiniz.

172.131.0.100 adresinden gelen iletişimi test etmek için [IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde `az network watcher test-ip-flow` komutunu çalıştırdığınızda elde edilen çıktı size **DefaultInboundDenyAll** kuralının iletişimi reddettiğini bildirdi. **DefaultInboundDenyAll** kuralı, `az network nic list-effective-nsg` komutundan elde edilen aşağıdaki çıktıda listelenen **DenyAllInBound** kuralına karşılık gelir:

```azurecli
{
 "access": "Deny",
 "additionalProperties": {},
 "destinationAddressPrefix": "0.0.0.0/0",
 "destinationAddressPrefixes": [
  "0.0.0.0/0"
 ],
 "destinationPortRange": "0-65535",
 "destinationPortRanges": [
  "0-65535"
 ],
 "direction": "Inbound",
 "expandedDestinationAddressPrefix": null,
 "expandedSourceAddressPrefix": null,
 "name": "defaultSecurityRules/DenyAllInBound",
 "priority": 65500,
 "protocol": "All",
 "sourceAddressPrefix": "0.0.0.0/0",
 "sourceAddressPrefixes": [
  "0.0.0.0/0"
 ],
 "sourcePortRange": "0-65535",
 "sourcePortRanges": [
  "0-65535"
 ]
},
```

Çıktıda gösterildiği gibi, `az network nic list-effective-nsg` komutundan elde edilen çıktıda, 172.131.0.100 adresinden sanal makineye gelen 80 numaralı bağlantı noktasına izin veren daha yüksek öncelikli başka bir kural olmadığından **DenyAllInBound** kuralı uygulanır. Gelen iletişime izin vermek için, 172.131.0.100 adresinden gelen 80 numaralı bağlantı noktasına izin veren daha yüksek öncelikli bir güvenlik kuralı ekleyebilirsiniz.

Bu hızlı başlangıçtaki denetimlerinde Azure yapılandırması test edilmiştir. Denetimler beklenen sonuçları döndürdüğü halde ağ sorunları yaşamaya devam ediyorsanız, sanal makineniz ve iletişim kurduğunuz uç nokta arasında bir güvenlik duvarı olmadığından ve sanal makinenizdeki işletim sisteminin, iletişime izin veren veya iletişimi reddeden bir güvenlik duvarının olmadığından emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir sanal makine oluşturdunuz ve gelen ve giden ağ trafiği filtrelerini tanıladınız. Ağ güvenlik grubu kurallarının bir sanal makineye gelen ve sanal makineden giden trafiğe izin verdiğini veya bu trafikleri reddettiğini öğrendiniz. [Güvenlik kuralları](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [güvenlik kuralları oluşturma](../virtual-network/manage-network-security-group.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-security-rule) hakkında daha fazla bilgi edinin.

Uygun ağ trafiği filtreleri mevcut olduğunda bile, yönlendirme yapılandırması nedeniyle bir sanal makineyle iletişim başarısız olabilir. Tek bir araçla sanal makine ağ yönlendirme sorunlarını tanılama hakkında bilgi edinmek için [Sanal makine yönlendirme sorunlarını tanılama](diagnose-vm-network-routing-problem-cli.md) bölümüne veya giden yönlendirme, gecikme ve trafik filtreleme sorunlarını tanılama hakkında bilgi edinmek için [Bağlantı sorunlarını giderme](network-watcher-connectivity-cli.md) bölümüne bakın.
