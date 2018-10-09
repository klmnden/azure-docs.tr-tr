---
title: Azure sanal ağına Container Instances'ı dağıtma
description: Kapsayıcı grupları bir yeni veya var olan Azure sanal ağına dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: 6d319c09b8a935b5ca81a6d5815daa5d2f706f45
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854632"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Azure sanal ağına Container Instances'ı dağıtma

[Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ve şirket içi kaynaklara güvenli, özel ağ filtreleme, Yönlendirme ve eşleme için Azure dahil olmak üzere sağlar. Kapsayıcı grupları bir Azure sanal ağa dağıtma, kapsayıcıları güvenli bir şekilde sanal ağdaki diğer kaynaklarla iletişim kurabilir.

Bir Azure sanal ağa dağıtılan kapsayıcı grupları gibi senaryolara olanak tanır:

* Kapsayıcı grupları aynı alt ağda arasında doğrudan iletişim
* Gönderme [görev-tabanlı](container-instances-restart-policy.md) container Instances sanal ağdaki bir veritabanına iş yükü çıktısı
* Container Instances ' içeriği almak bir [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) sanal ağ
* Sanal ağdaki sanal makineler ile kapsayıcı iletişimi
* Kapsayıcıya iletişim şirket içi kaynaklarla bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Bu özellik şu anda önizlemededir ve bazı [sınırlamalar uygulanır](#preview-limitations). Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="virtual-network-deployment-limitations"></a>Sanal ağ dağıtım sınırlamaları

Kapsayıcı grupları bir sanal ağa dağıttığınız zaman, bazı sınırlamalar uygulanır.

* Windows kapsayıcıları desteklenmez
* Kapsayıcı grubu bir alt ağa dağıtmak için alt ağdaki diğer kaynak türlerini içeremez. Kapsayıcı grubu için dağıtmadan önce var olan bir alt ağdan var olan tüm kaynakları kaldırın veya yeni bir alt ağ oluşturun.
* Kapsayıcı grupları dağıtılan bir sanal ağa, genel IP adresleri veya DNS adı etiketleri şu anda desteklemez.
* Ek ağ kaynakları nedeniyle dahil, bir kapsayıcı grubu için bir sanal ağ dağıtma genellikle bir standart kapsayıcı örneği dağıtmaya oranla biraz daha yavaştır.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Bu özellik Önizleme aşamasında olduğu sürece, bir sanal ağa container Instances dağıtımı yüklenirken aşağıdaki sınırlamalar uygulanır.

**Desteklenen** bölgeler:

* Batı Avrupa (westeurope)
* Batı ABD (westus)

**Desteklenmeyen** ağ kaynakları:

* Ağ Güvenliği Grubu
* Azure Load Balancer

**Ağ kaynak silme** gerektirir [ek adımlar](#delete-network-resources) kapsayıcı grupları sanal ağa dağıttıktan sonra.

## <a name="required-network-resources"></a>Gerekli ağ kaynakları

Kapsayıcı grupları bir sanal ağa dağıtmak için gereken üç Azure sanal ağ kaynağı vardır: [sanal ağ](#virtual-network) kendisini bir [alt temsilci](#subnet-delegated) sanal ağı ve bir içinde[ağ profili](#network-profile).

### <a name="virtual-network"></a>Sanal ağ

Bir sanal ağ, bir veya daha fazla alt ağ oluşturduğunuz adres alanı tanımlar. (Kapsayıcı grupları gibi) Azure kaynaklarını alt ağlara sanal ağınızdaki dağıtırsınız.

### <a name="subnet-delegated"></a>Alt ağ (yönetici temsilcisi)

Alt ağlar, sanal ağ yerleştirebilirsiniz, Azure kaynaklarını kullanılabilir ayrı adres alanları ölçütü. Bir sanal ağ içindeki bir veya birden çok alt ağlar oluşturursunuz.

Kapsayıcı grubu için kullandığınız alt ağ yalnızca kapsayıcı grubu içerebilir. Bir alt ağ için bir kapsayıcı grubu ilk kez dağıttığınızda, Azure alt ağın Azure Container ınstances'a atar. Alt ağ, temsilci sonra yalnızca kapsayıcı grupları için kullanılabilir. Kapsayıcı grupları dışındaki kaynaklara temsilci bir alt ağa dağıtma girişiminde işlemi başarısız olur.

### <a name="network-profile"></a>Ağ profili

Azure kaynakları için ağ yapılandırma şablonu ağ profilidir. Bu kaynak, örneğin, içine, dağıtılması alt ağ için bazı ağ özellikleri belirtir. İlk kez bir alt ağ (ve bir sanal ağ böylece) bir kapsayıcı grubu dağıtma, Azure sizin için bir ağ profili oluşturur. Ardından bu ağ profili alt ağa gelecekteki dağıtımlar için de kullanabilirsiniz.

Aşağıdaki diyagramda, birkaç kapsayıcı grupları, Azure Container Instances'a temsilci bir alt ağa dağıtıldığı. Bir alt ağ için bir kapsayıcı grubunu dağıttıktan sonra ek kapsayıcı grubu için aynı ağ profili belirterek dağıtabilirsiniz.

![Bir sanal ağ içindeki kapsayıcı grupları][aci-vnet-01]

## <a name="deploy-to-virtual-network"></a>Sanal ağa dağıtma

Yeni bir sanal ağa kapsayıcılı grupları dağıtma ve sizin için gerekli ağ kaynakları oluşturma veya mevcut bir sanal ağa dağıtmak Azure izin verebilirsiniz.

### <a name="new-virtual-network"></a>Yeni sanal ağ

Yeni bir sanal ağa dağıtma ve ağ kaynakları için otomatik olarak oluşturduğunuz Azure yürüttüğünüzde aşağıdaki belirtin [az kapsayıcı oluşturma][az-container-create]:

* Sanal ağ adı
* Sanal ağ adres ön eki CIDR biçiminde
* Alt ağ adı
* Alt ağ CIDR biçiminde adres öneki

Sanal ağ ve alt ağ adres alanları, sırasıyla sanal ağ ve alt ağ adres ön ekleri belirtin. Bu değerler örneğin sınıfsız etki alanları arası yönlendirme (CIDR) gösterimde temsil `10.0.0.0/16`. Alt ağlar ile çalışma hakkında daha fazla bilgi için bkz. [ekleme, değiştirme veya silme bir sanal ağ alt](../virtual-network/virtual-network-manage-subnet.md).

Bu yöntemle ilk kapsayıcı grubunuzun dağıttıktan sonra sanal ağ ve alt ağ adları ya da Azure sizin için otomatik olarak oluşturduğu ağ profili belirterek aynı alt ağa dağıtabilirsiniz. Azure, alt ağ ile Azure Container Instances temsilcilerini çünkü dağıtabileceğiniz *yalnızca* kapsayıcı grupları için alt ağ.

### <a name="existing-virtual-network"></a>Var olan sanal ağı

Bir sanal ağınız için bir kapsayıcı grubu dağıtmak için:

1. Mevcut sanal ağınızdaki bir alt ağ oluşturun veya var olan bir alt ağdan boş *tüm* diğer kaynaklar
1. Bir kapsayıcı grubu dağıtma [az kapsayıcı oluşturma] [ az-container-create] ve aşağıdakilerden birini belirtin:
   * Sanal ağ adını ve alt ağ adı</br>
    or
   * Ağ profili adı veya kimliği

İlk kapsayıcı grubunuzu mevcut bir alt ağa dağıttığınız sonra Azure alt ağın Azure Container ınstances'a atar. Artık, kapsayıcı grupları haricinde kaynaklar bu alt ağa dağıtabilirsiniz.

Aşağıdaki bölümlerde, sanal ağ Azure CLI ile kapsayıcı grupları dağıtma açıklanmaktadır. Komut örnekleri için biçimlendirilmiş **Bash** Kabuğu. Satır devamlılığı karakteri, PowerShell veya komut istemi gibi başka bir kabuk tercih ederseniz, buna göre ayarlayın.

## <a name="deploy-to-new-virtual-network"></a>Yeni bir sanal ağa dağıtma

İlk olarak, bir kapsayıcı grubuna dağıtın ve yeni sanal ağ ve alt ağ için parametreleri belirtin. Bu parametreleri belirttiğinizde, Azure sanal ağı ve alt ağ oluşturur, alt ağ ile Azure Container Instances temsilcilerini ve ayrıca bir ağ profili oluşturur. Bu kaynaklar oluşturulduktan sonra kapsayıcı grubunuzun alt ağa dağıtılır.

Aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma] [ az-container-create] yeni sanal ağ ve alt ağ ayarlarını belirten komutu. Bu komut dağıtır [microsoft/aci-helloworld] [ aci-helloworld] statik bir web sayfasına hizmet veren küçük bir Node.js Web sunucusu çalıştıran bir kapsayıcı. Sonraki bölümde, aynı alt ağa ikinci bir kapsayıcı grubu dağıtın ve iki kapsayıcı örnekleri arasında iletişimi test etme.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image microsoft/aci-helloworld \
    --vnet-name aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Bu yöntemi kullanarak yeni bir sanal ağa dağıttığınızda, dağıtım ağ kaynakları oluşturulurken birkaç dakika sürebilir. İlk dağıtımdan sonra ek bir kapsayıcı grubu dağıtımları daha hızlı bir şekilde tamamlayın.

## <a name="deploy-to-existing-virtual-network"></a>Mevcut bir sanal ağa dağıtma

Yeni bir sanal ağ için bir kapsayıcı grubu dağıttığınıza göre aynı alt ağa ikinci bir kapsayıcı grubu dağıtın ve iki kapsayıcı örnekleri arasındaki iletişimi doğrulayın.

İlk olarak, ilk kapsayıcı grubu dağıttıysanız, IP adresini alın *appcontainer*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

Çıkış özel alt kapsayıcı grubu IP adresini görüntülemelidir.

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Şimdi, `CONTAINER_GROUP_IP` ile aldığınız IP `az container show` komutunu ve aşağıdakileri yürütün `az container create` komutu. Bu ikinci kapsayıcı *commchecker*, Alpine Linux tabanlı bir görüntü çalıştırır ve yürüten `wget` karşı ilk kapsayıcı grubun özel alt ağ IP adresi.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet-name aci-vnet \
    --subnet aci-subnet
```

Bu ikinci kapsayıcı dağıtımı tamamlandıktan sonra günlükleri çıktısını görebilmeniz için çekme `wget` yürütülen komut:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

İkinci kapsayıcıyı ilk başarılı bir şekilde iletilen, çıkış benzer olmalıdır:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

Günlük çıktısını gösteren `wget` bağlanmak ve özel IP adresini kullanarak yerel alt ağdaki ilk kapsayıcısından dizin dosyası indirmek mümkün oldu. İki kapsayıcı grupları arasındaki ağ trafiğini sanal ağ içinde kaldı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="delete-container-instances"></a>Azure Container Instances

İşiniz bittiğinde kapsayıcı örneklerle çalışma oluşturduğunuz, aşağıdaki komutlarla hem de Sil:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
```

### <a name="delete-network-resources"></a>Ağ kaynakları silme

Bu özelliğin ilk önizleme daha önce oluşturduğunuz ağ kaynakları silmek için birkaç ek komutlar gerektirir. Sanal ağ ve alt ağ oluşturmak için bu makalenin önceki bölümlerinde örnek komutlarını kullandıysanız, bu ağ kaynakları silmek için aşağıdaki betiği kullanabilirsiniz.

Komut yürütülmeden önce ayarlayın `RES_GROUP` değişken silinmesi gereken alt ağ ve sanal ağın bulunduğu kaynak grubunun adı. Komut, Bash kabuğunda biçimlendirilir. PowerShell veya komut istemi gibi başka bir kabuk tercih ederseniz, değişken ataması hem de erişimcileri uygun şekilde ayarlamanız gerekir.

> [!WARNING]
> Bu betik, kaynakları siler! Sanal ağ ve içerdiği tüm alt ağlar siler. Artık ihtiyacınız mutlaka *herhangi* içeriyor, bu betik çalıştırılmadan önce hiçbir alt ağ sanal ağ kaynaklarından dahil. Bir kez silinmiş **bu kaynakları kurtarılamaz**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --remove delegations 0

# Delete the subnet
az network vnet subnet delete --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Sonraki adımlar

Birden çok sanal ağ kaynakları ve özellikler bu makalede, ancak kısaca ele alınan. Azure sanal ağ belgeleri, kapsamlı bir şekilde bu konuları kapsamaktadır:

* [Sanal ağ](../virtual-network/manage-virtual-network.md)
* [Alt ağ](../virtual-network/virtual-network-manage-subnet.md)
* [Hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
