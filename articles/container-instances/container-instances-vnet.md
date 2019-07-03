---
title: Azure sanal ağına Container Instances'ı dağıtma
description: Kapsayıcı grupları bir yeni veya var olan Azure sanal ağına dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: 25f9d4e02bcb354acf1c771157622f07c5f4bcc1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712812"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Azure sanal ağına Container Instances'ı dağıtma

[Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ve şirket içi kaynaklara Azure için güvenli, özel ağ sağlar. Kapsayıcı grupları bir Azure sanal ağa dağıtma, kapsayıcıları güvenli bir şekilde sanal ağdaki diğer kaynaklarla iletişim kurabilir.

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

* Kapsayıcı grubu bir alt ağa dağıtmak için alt ağdaki diğer kaynak türlerini içeremez. Kapsayıcı grubu için dağıtmadan önce var olan bir alt ağdan var olan tüm kaynakları kaldırın veya yeni bir alt ağ oluşturun.
* Kullanamazsınız bir [yönetilen kimliği](container-instances-managed-identity.md) bir kapsayıcı grubunda dağıtılan bir sanal ağa.
* Ek ağ kaynakları nedeniyle dahil, bir kapsayıcı grubu için bir sanal ağ dağıtma genellikle bir standart kapsayıcı örneği dağıtmaya oranla biraz daha yavaştır.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Bu özellik Önizleme aşamasında olduğu sürece, bir sanal ağa kapsayıcı grupları dağıtma yüklenirken aşağıdaki sınırlamalar uygulanır. 

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

Kapsayıcı kaynak sınırları ağa container Instances aşağıdaki bölgelerde sınırlarını farklılık gösterebilir. Şu anda yalnızca Linux kapsayıcıları için bu özelliği desteklenmektedir. Windows desteği planlanmaktadır.

### <a name="unsupported-networking-scenarios"></a>Desteklenmeyen ağ senaryoları 

* **Azure Load Balancer** -container Instances önünde Azure Load Balancer bir ağa bağlı bir kapsayıcı grubuna eklemek desteklenmiyor
* **Sanal Ağ eşlemesi** -başka bir sanal ağ için Azure Container Instances'a temsilci bir alt ağ içeren bir sanal ağı eşleyebilme olamaz
* **Rota tabloları** -kullanıcı tanımlı yollar ayarlanamaz bir alt ağda Azure Container Instances'a temsilcisi
* **Ağ güvenlik grupları** -olmayan uygulanmakta Azure Container Instances'a temsilci bir alt ağa uygulanan Nsg giden güvenlik kuralları 
* **Genel IP veya DNS etiketi** -kapsayıcı grupları dağıtılan bir sanal ağa genel bir IP adresi veya tam etki alanı adı ile doğrudan Internet'e ifşa edildi kapsayıcılar şu anda desteklemez
* **İç ad çözümlemesi** -sanal ağ üzerinden iç Azure DNS, Azure kaynakları için ad çözümlemesi desteklenmiyor

**Ağ kaynak silme** gerektirir [ek adımlar](#delete-network-resources) kapsayıcı grupları sanal ağa dağıttıktan sonra.

## <a name="required-network-resources"></a>Gerekli ağ kaynakları

Kapsayıcı grupları bir sanal ağa dağıtmak için gereken üç Azure sanal ağ kaynağı vardır: [sanal ağ](#virtual-network) kendisini bir [alt temsilci](#subnet-delegated) sanal ağı ve bir içinde[ağ profili](#network-profile). 

### <a name="virtual-network"></a>Sanal ağ

Bir sanal ağ, bir veya daha fazla alt ağ oluşturduğunuz adres alanı tanımlar. (Kapsayıcı grupları gibi) Azure kaynaklarını alt ağlara sanal ağınızdaki dağıtırsınız.

### <a name="subnet-delegated"></a>Alt ağ (yönetici temsilcisi)

Alt ağlar, sanal ağ yerleştirebilirsiniz, Azure kaynaklarını kullanılabilir ayrı adres alanları ölçütü. Bir sanal ağ içindeki bir veya birden çok alt ağlar oluşturursunuz.

Kapsayıcı grubu için kullandığınız alt ağ yalnızca kapsayıcı grubu içerebilir. Bir alt ağ için bir kapsayıcı grubu ilk kez dağıttığınızda, Azure alt ağın Azure Container ınstances'a atar. Alt ağ, temsilci sonra yalnızca kapsayıcı grupları için kullanılabilir. Kapsayıcı grupları dışındaki kaynaklara temsilci bir alt ağa dağıtma girişiminde işlemi başarısız olur.

### <a name="network-profile"></a>Ağ profili

Azure kaynakları için ağ yapılandırma şablonu ağ profilidir. Bu kaynak, örneğin, içine, dağıtılması alt ağ için bazı ağ özellikleri belirtir. İlk kez kullandığınızda [az kapsayıcı oluşturma] [ az-container-create] bir kapsayıcı grubu bir alt ağ (ve bir sanal ağ böylece) dağıtmak için komut, Azure sizin için bir ağ profili oluşturur. Ardından bu ağ profili alt ağa gelecekteki dağıtımlar için de kullanabilirsiniz. 

Bir alt ağ için bir kapsayıcı grubu dağıtmak için Resource Manager şablonu, YAML dosyası ya da programlı bir yöntem kullanmak için bir ağ profili tam Resource Manager kaynak Kimliğini sağlamanız gerekir. Kullanarak daha önce oluşturduğunuz bir profili kullanabilirsiniz [az kapsayıcı oluşturma][az-container-create], veya bir Resource Manager şablonu kullanarak profil oluşturma (bkz [şablon örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet) ve [başvuru](https://docs.microsoft.com/azure/templates/microsoft.network/networkprofiles)). Daha önce oluşturulmuş bir profilini Kimliğini almak için kullanın [az ağ profili listesi] [ az-network-profile-list] komutu. 

Aşağıdaki diyagramda, birkaç kapsayıcı grupları, Azure Container Instances'a temsilci bir alt ağa dağıtıldığı. Bir alt ağ için bir kapsayıcı grubunu dağıttıktan sonra ek kapsayıcı grubu için aynı ağ profili belirterek dağıtabilirsiniz.

![Bir sanal ağ içindeki kapsayıcı grupları][aci-vnet-01]

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Kullanabileceğiniz [az kapsayıcı oluşturma] [ az-container-create] yeni bir sanal ağa kapsayıcılı grupları dağıtma ve sizin için gerekli ağ kaynakları oluşturma veya mevcut bir sanal ağa dağıtmak Azure izin vermek için. 

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
   * Sanal ağ adını ve alt ağ adı
   * Sanal ağ kaynağı kimliği ve sanal ağdan farklı bir kaynak grubu kullanarak alt ağ kaynak kimliği
   * Ağ profili adını veya Kimliğini kullanarak elde edebilirsiniz [az ağ profili listesi][az-network-profile-list]

İlk kapsayıcı grubunuzu mevcut bir alt ağa dağıttığınız sonra Azure alt ağın Azure Container ınstances'a atar. Artık, kapsayıcı grupları haricinde kaynaklar bu alt ağa dağıtabilirsiniz.

## <a name="deployment-examples"></a>Dağıtım örnekleri

Aşağıdaki bölümlerde, sanal ağ Azure CLI ile kapsayıcı grupları dağıtma açıklanmaktadır. Komut örnekleri için biçimlendirilmiş **Bash** Kabuğu. Satır devamlılığı karakteri, PowerShell veya komut istemi gibi başka bir kabuk tercih ederseniz, buna göre ayarlayın.

### <a name="deploy-to-a-new-virtual-network"></a>Yeni bir sanal ağa dağıtma

İlk olarak, bir kapsayıcı grubuna dağıtın ve yeni sanal ağ ve alt ağ için parametreleri belirtin. Bu parametreleri belirttiğinizde, Azure sanal ağı ve alt ağ oluşturur, alt ağ ile Azure Container Instances temsilcilerini ve ayrıca bir ağ profili oluşturur. Bu kaynaklar oluşturulduktan sonra kapsayıcı grubunuzun alt ağa dağıtılır.

Aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma] [ az-container-create] yeni sanal ağ ve alt ağ ayarlarını belirten komutu. Bir bölgede oluşturulan bir kaynak grubu adı sağlamanız gereken, [destekler](#preview-limitations) kapsayıcı grupları bir sanal ağ içinde. Bu komutu, genel Microsoft dağıtır [aci-helloworld][aci-helloworld] statik bir web sayfasına hizmet veren küçük bir Node.js Web sunucusu çalıştıran bir kapsayıcı. Sonraki bölümde, aynı alt ağa ikinci bir kapsayıcı grubu dağıtın ve iki kapsayıcı örnekleri arasında iletişimi test etme.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --vnet aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Bu yöntemi kullanarak yeni bir sanal ağa dağıttığınızda, dağıtım ağ kaynakları oluşturulurken birkaç dakika sürebilir. İlk dağıtımdan sonra ek bir kapsayıcı grubu dağıtımları daha hızlı bir şekilde tamamlayın.

### <a name="deploy-to-existing-virtual-network"></a>Mevcut bir sanal ağa dağıtma

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
    --vnet aci-vnet \
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

### <a name="deploy-to-existing-virtual-network---yaml"></a>Mevcut bir sanal ağa - YAML dağıtma

Ayrıca, bir YAML dosyası kullanarak bir kapsayıcı grubu mevcut bir sanal ağa dağıtabilirsiniz. Bir sanal ağ içindeki alt ağ dağıtmak için çeşitli ek özellikler YAML içinde belirtin:

* `ipAddress`: Kapsayıcı grubu için IP adresi ayarları.
  * `ports`: Varsa, açmak için bağlantı noktaları.
  * `protocol`: Protokolü (TCP veya UDP) bağlantı noktasının açık.
* `networkProfile`: Bir Azure kaynağı için alt ağ ve sanal ağ gibi ağ ayarlarını belirtir.
  * `id`: Tam Resource Manager kaynak kimliği `networkProfile`.

Kapsayıcı grubu, bir YAML dosyası ile bir sanal ağa dağıtmak için önce ağ profilini Kimliğini almanız gerekir. Yürütme [az ağ profili listesi] [ az-network-profile-list] sanal ağınız ile temsil edilen alt ağ içeren kaynak grubunun adını belirterek komutu.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

Komut çıktısı, ağ profili için tam kaynak Kimliğini görüntüler:

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Profil Kimliği, adlı yeni bir dosyaya aşağıdaki YAML'ye kopyalayın ağ oluşturduktan sonra *vnet dağıtma aci.yaml*. Altında `networkProfile`, değiştirin `id` kimliği henüz değeriyle alınmış değiştirip dosyayı kaydedin. Adlı bir kapsayıcı grubu bu YAML oluşturur *appcontaineryaml* sanal ağınızda.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/container/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Kapsayıcı grubu dağıtma [az kapsayıcı oluşturma] [ az-container-create] YAML dosyası adını belirterek komutu `--file` parametresi:

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Dağıtım tamamlandıktan sonra Çalıştır [az container show] [ az-container-show] komut durumunu görüntülemek için:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                                       IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   mcr.microsoft.com/azuredocs/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="delete-container-instances"></a>Azure Container Instances

İşiniz bittiğinde kapsayıcı örnekleri ile çalışma, oluşturduğunuz ve aşağıdaki komutlarla silin:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Ağ kaynakları silme

Bu özelliğin ilk önizleme daha önce oluşturduğunuz ağ kaynakları silmek için birkaç ek komutlar gerektirir. Sanal ağ ve alt ağ oluşturmak için bu makalenin önceki bölümlerinde örnek komutlarını kullandıysanız, bu ağ kaynakları silmek için aşağıdaki betiği kullanabilirsiniz.

Komut yürütülmeden önce ayarlayın `RES_GROUP` değişken silinmesi gereken alt ağ ve sanal ağın bulunduğu kaynak grubunun adı. Sanal ağ ve alt ağ adları, kullanmadıysanız güncelleştirme `aci-vnet` ve `aci-subnet` isimleridir daha önce. Komut, Bash kabuğunda biçimlendirilir. PowerShell veya komut istemi gibi başka bir kabuk tercih ederseniz, değişken ataması hem de erişimcileri uygun şekilde ayarlamanız gerekir.

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
# Replace aci-vnet and aci-subnet with your VNet and subnet names in the following commands

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

Yeni sanal ağ, alt ağ, ağ profili ve bir Resource Manager şablonu kullanarak kapsayıcı grubu dağıtmak için bkz. [sanal ağ ile bir Azure kapsayıcı grubu oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
).

Birden çok sanal ağ kaynakları ve özellikler bu makalede, ancak kısaca ele alınan. Azure sanal ağ belgeleri, kapsamlı bir şekilde bu konuları kapsamaktadır:

* [Sanal ağ](../virtual-network/manage-virtual-network.md)
* [Alt ağ](../virtual-network/virtual-network-manage-subnet.md)
* [Hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list
