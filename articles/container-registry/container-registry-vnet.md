---
title: Bir sanal ağa bir Azure container Registry'yi dağıtma
description: Erişim yalnızca bir Azure sanal ağ içindeki kaynaklarla veya ortak IP adresi aralıkları bir Azure container registry'ye izin verin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/14/2019
ms.author: danlep
ms.openlocfilehash: 0a4d9f355a5cdc92bab4491c08677042c42986cb
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517938"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Bir Azure sanal ağ veya güvenlik duvarı kurallarını kullanarak bir Azure container registry'ye erişimi kısıtlama

[Azure sanal ağı](../virtual-network/virtual-networks-overview.md) ve şirket içi kaynaklara Azure için güvenli, özel ağ sağlar. Azure sanal ağına özel Azure kapsayıcısı kayıt defteriniz dağıtarak, yalnızca sanal ağ içindeki kaynaklarla kayıt defterine erişim sağlayabilirsiniz. Şirket içi senaryolar için yalnızca belirli IP adreslerinden gelen kayıt defteri erişim izni vermek için güvenlik duvarı kuralları yapılandırabilirsiniz.

Bu makalede, Azure container registry erişimi sınırlamak için ağ erişim kuralları oluşturmak için iki senaryo gösterilmektedir: aynı ağda dağıtılmış bir sanal makine veya sanal makinenin genel IP adresi.

> [!IMPORTANT]
> Bu özellik şu anda önizlemededir ve bazı [sınırlamalar uygulanır](#preview-limitations). Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Yalnızca bir **Premium** kapsayıcı kayıt defteri ağ erişim kuralları ile yapılandırılabilir. Kayıt defteri hizmet katmanları hakkında daha fazla bilgi için bkz: [Azure Container Registry SKU'ları](container-registry-skus.md). 

* Yalnızca bir [Azure Kubernetes hizmeti](../aks/intro-kubernetes.md) küme veya Azure [sanal makine](../virtual-machines/linux/overview.md) konak olarak bir sanal ağ içindeki bir kapsayıcı kayıt defterine erişim için kullanılabilir. *Azure Container Instances gibi diğer Azure Hizmetleri şu anda desteklenmemektedir.*

* Her kayıt, en fazla 100 sanal ağ kuralları destekler.

## <a name="prerequisites"></a>Önkoşullar

* Azure'ı kullanmak için CLI adımlar bu makalede, Azure CLI Sürüm 2.0.58 veya üstü gereklidir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

* Bir kapsayıcı kayıt defterinde yoksa (Premium gerekli SKU) oluşturun ve bir örnek görüntü gibi anında iletme `hello-world` Docker hub'dan. Örneğin, [Azure portalında] [ quickstart-portal] veya [Azure CLI] [ quickstart-cli] bir kayıt defteri oluşturmak için. 

## <a name="about-network-rules-for-a-container-registry"></a>Kapsayıcı kayıt defteri için ağ kuralları hakkında

Varsayılan olarak Azure container registry, herhangi bir ağa konaklarında internet üzerinden bağlantı kabul eder. Bir sanal ağ ile bir AKS kümesi veya bir ağ sınırı geçmeden ssısdb'ye kayıt defteri güvenli bir şekilde erişmek için Azure VM gibi Azure kaynakları yalnızca izin verebilirsiniz. Ağ güvenlik duvarı kuralları beyaz liste de yapılandırabilirsiniz belirli genel internet IP adresi aralıkları. 

Tüm ağ bağlantıları reddeder bir kayıt defteri erişimi sınırlamak için ilk kayıt defteri varsayılan eylemi değiştirin. Ardından, ağ erişim kuralları ekleyin. İstemcilere verilen ağ kuralları üzerinden erişim gerekir devam [kapsayıcı kayıt defterine kimlik doğrulaması](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) ve veri erişim iznine sahip olması.

### <a name="service-endpoint-for-subnets"></a>Alt ağlar için hizmet uç noktası

Sanal ağ içinde bir alt ağından erişime izin vermek için eklemek gereken bir [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) Azure Container Registry hizmeti. 

Çok kiracılı hizmetler gibi Azure Container Registry, tüm müşteriler için tek bir IP adresleri kümesini kullanın. Hizmet uç noktası, bir kayıt defterine erişim için bir uç nokta atar. Bu uç nokta trafiği Azure omurga ağı üzerinden kaynağa bir en uygun rotayı sunar. Sanal ağ ve alt ağ kimlikleri, ayrıca her bir istekle aktarılır.

### <a name="firewall-rules"></a>Güvenlik duvarı kuralları

IP ağ kuralları için izin verilen internet gibi CIDR gösterimini kullanarak adres aralıkları sağlamak *16.17.18.0/24* veya gibi bir tek tek IP adresleri *16.17.18.19*. IP ağ kuralları için yalnızca verilir *genel* internet IP adresi. IP adresi aralıkları için özel ağlar (RFC 1918 ' tanımlandığı şekilde) ayrılmış IP kurallarında izin verilmez.

## <a name="create-a-docker-enabled-virtual-machine"></a>Docker özellikli sanal makine oluşturma

Bu makalede, Azure container registry erişmek için Docker özellikli bir Ubuntu VM kullanın. Kayıt defterine Azure Active Directory kimlik doğrulaması kullanmak için ayrıca yüklemeniz [Azure CLI] [ azure-cli] VM üzerinde. Bir Azure sanal makinesi zaten varsa, bu Oluşturma adımı atlayın.

Aynı kaynak grubunu, sanal makine ve kapsayıcı kayıt defteriniz için kullanabilir. Bu kurulum temizleme sonunda kolaylaştırır, ancak gerekli değildir. Sanal ağ ve sanal makine için ayrı bir kaynak grubu oluşturmayı seçerseniz, çalıştırma [az grubu oluşturma][az-group-create]. Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *westcentralus* konumu:

```azurecli
az group create --name myResourceGroup --location westus
```

Artık varsayılan dağıtma Ubuntu Azure sanal makine ile [az vm oluşturma][az-vm-create]. Aşağıdaki örnekte adlı bir VM oluşturur *myDockerVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’nin oluşturulması birkaç dakika sürer. Komut tamamlandığında, Not `publicIpAddress` Azure CLI tarafından görüntülenen. Sanal makineye ve isteğe bağlı olarak güvenlik duvarı kuralları daha sonra kurulumu için SSH bağlantılarına bu adresi kullanın.

### <a name="install-docker-on-the-vm"></a>Docker'ı VM'ye yükleyin

Sanal makine çalışmaya başladıktan sonra bir SSH bağlantısı VM'ye olun. Değiştirin *Publicıpaddress* sanal makinenizin genel IP adresiyle.

```bash
ssh azureuser@publicIpAddress
```

Ubuntu VM'ye Docker yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo apt install docker.io -y
```

Yükleme sonrasında, Docker VM üzerinde düzgün çalıştığını doğrulamak için aşağıdaki komutu çalıştırın:

```bash
sudo docker run -it hello-world
```

Çıkış:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme

Bağlantısındaki [apt ile Azure CLI yükleme](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) Ubuntu sanal makinenizde Azure CLI'yı yüklemek için. Bu makalede 2.0.58 sürümünü yüklediğinizden emin olun veya üzeri.

SSH bağlantısını kapatın.

## <a name="allow-access-from-a-virtual-network"></a>Bir sanal ağdan erişime izin ver

Bu bölümde, bir Azure sanal ağındaki bir alt ağından erişime izin vermek için bir kapsayıcı kayıt defterinizi yapılandırın. Azure CLI ve Azure portalını kullanarak eşdeğer adımlar verilmiştir.

### <a name="allow-access-from-a-virtual-network---cli"></a>Sanal ağ - CLI üzerinden erişime izin ver

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Bir alt ağ için hizmet uç noktası ekleme

Bir VM oluşturduğunuzda, Azure varsayılan olarak, aynı kaynak grubunda bir sanal ağ oluşturur. Sanal ağ adı, sanal makine adına bağlıdır. Örneğin, sanal makinenizin adını *myDockerVM*, varsayılan sanal ağ adı *myDockerVMVNET*, adlı bir alt ağ ile *myDockerVMSubnet*. Bunu kullanarak veya Azure portalında doğrulamak [az ağ vnet listesinde] [ az-network-vnet-list] komutu:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Çıkış:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Kullanım [az ağ sanal ağ alt ağı güncelleştirme] [ az-network-vnet-subnet-update] eklemek için komutu bir **Microsoft.ContainerRegistry** alt ağınız için hizmet uç noktası. Sanal ağ ve alt ağ aşağıdaki komutta adlarını değiştirin:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Kullanım [az ağ sanal ağ alt ağı show] [ az-network-vnet-subnet-show] alt ağın kaynak Kimliğini almak için komutu. Bu ağ erişim kuralı yapılandırmak için daha sonraki bir adımda gerekir.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Çıkış:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Kayıt defterine varsayılan ağ erişimi değiştirme

Varsayılan olarak, Azure container registry, herhangi bir ağdaki ana bilgisayarlardan gelen bağlantıları sağlar. Seçili ağ erişimini sınırlamak için erişimi engellemek için varsayılan eylem değiştirin. Aşağıdaki kayıt defterinizin adıyla değiştirin [az acr update] [ az-acr-update] komutu:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleyin

Kullanım [az acr ağ kuralı ekleyin] [ az-acr-network-rule-add] VM alt ağından erişime izin veren kayıt defteriniz için ağ kuralı eklemek için komutu. Kapsayıcı kayıt defterinin adı ve kaynak kimliği şu komutla alt ağın değiştirin: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

Devam [kayıt defterine erişim doğrulayın](#verify-access-to-the-registry).

### <a name="allow-access-from-a-virtual-network---portal"></a>Bir sanal ağ - erişime izin verecek portalı

#### <a name="add-service-endpoint-to-subnet"></a>Alt ağ için hizmet uç noktası ekleme

Bir VM oluşturduğunuzda, Azure varsayılan olarak, aynı kaynak grubunda bir sanal ağ oluşturur. Sanal ağ adı, sanal makine adına bağlıdır. Örneğin, sanal makinenizin adını *myDockerVM*, varsayılan sanal ağ adı *myDockerVMVNET*, adlı bir alt ağ ile *myDockerVMSubnet*.

Hizmet uç noktası, Azure Container Registry için bir alt ağa eklemek için:

1. Üst kısmındaki arama kutusuna [Azure portalında][azure-portal], girin *sanal ağlar*. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
1. Sanal ağlar, sanal makinenizi dağıtıldığı, gibi sanal ağ seçim *myDockerVMVNET*.
1. Altında **ayarları**seçin **alt ağlar**.
1. Sanal makinenizi dağıtıldığı, gibi bir alt ağ seçin *myDockerVMSubnet*.
1. Altında **hizmet uç noktalarını**seçin **Microsoft.ContainerRegistry**.
1. **Kaydet**’i seçin.

![Alt ağ için hizmet uç noktası ekleme][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Ağ erişimi kayıt defteri için yapılandırma

Varsayılan olarak, Azure container registry, herhangi bir ağdaki ana bilgisayarlardan gelen bağlantıları sağlar. Sanal ağ erişimini sınırlamak için:

1. Portalda kapsayıcı kayıt defterinize gidin.
1. Altında **ayarları**seçin **güvenlik duvarı ve sanal ağlar**.
1. Varsayılan olarak erişimi reddetmek için erişime izin verecek şekilde seçin: **seçili ağlar**. 
1. Seçin **var olan sanal ağı Ekle**, sanal ağ ve bir hizmet uç noktası ile yapılandırılmış olan alt ağ seçin. **Add (Ekle)** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

![Kapsayıcı kayıt defteri için sanal ağ yapılandırma][acr-vnet-portal]

Devam [kayıt defterine erişim doğrulayın](#verify-access-to-the-registry).

## <a name="allow-access-from-an-ip-address"></a>Bir IP adresinden erişime izin ver

Bu bölümde, bir Azure sanal ağındaki bir alt ağından erişime izin vermek için bir kapsayıcı kayıt defterinizi yapılandırın. Azure CLI ve Azure portalını kullanarak eşdeğer adımlar verilmiştir.

### <a name="allow-access-from-an-ip-address---cli"></a>-CLI bir IP adresinden erişime izin ver

#### <a name="change-default-network-access-to-registry"></a>Kayıt defterine varsayılan ağ erişimi değiştirme

Zaten yapmadıysanız, varsayılan olarak erişimi engellemek için kayıt defteri yapılandırmasını güncelleştirin. Aşağıdaki kayıt defterinizin adıyla değiştirin [az acr update] [ az-acr-update] komutu:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Kayıt defterinden ağ kuralını Kaldır

VM alt ağından erişime izin vermek için bir ağ kuralı daha önce eklediyseniz alt ağ hizmet uç noktası ve ağ kuralını kaldırın. Kapsayıcı kayıt defterinin adı ve kaynak kimliği, daha önceki bir adımda aldığınız alt ağın yerine [az acr ağ kuralını Kaldır] [ az-acr-network-rule-remove] komutu: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleyin

Kullanım [az acr ağ kuralı ekleyin] [ az-acr-network-rule-add] VM'nin IP adresinden erişime izin veren kayıt defteriniz için ağ kuralı eklemek için komutu. Kapsayıcı kayıt defterinin adı ve aşağıdaki komutta VM'nin genel IP adresini değiştirin.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

Devam [kayıt defterine erişim doğrulayın](#verify-access-to-the-registry).

### <a name="allow-access-from-an-ip-address---portal"></a>-Bir IP adresinden erişime portalı

#### <a name="remove-existing-network-rule-from-registry"></a>Kayıt defterinden mevcut ağ kuralını Kaldır

VM alt ağından erişime izin vermek için bir ağ kuralı önceden eklenen mevcut kuralı kaldırın. Farklı bir VM üzerindeki kayıt defterine erişmek istiyorsanız, bu bölümü atlayın.

* Azure Container Registry için alt ağ hizmet uç noktasını kaldırmak için alt ağ ayarlarını güncelleştirin. 

  1. İçinde [Azure portalında][azure-portal], sanal makinenizi dağıtıldığı sanal ağa gidin.
  1. Altında **ayarları**seçin **alt ağlar**.
  1. Sanal makinenizi dağıtıldığı alt ağı seçin.
  1. Altında **hizmet uç noktalarını**, onay kutusunu kaldırın **Microsoft.ContainerRegistry**. 
  1. **Kaydet**’i seçin.

* Alt kayıt defterine erişim izni veren ağ kuralını kaldırın.

  1. Portalda kapsayıcı kayıt defterinize gidin.
  1. Altında **ayarları**seçin **güvenlik duvarı ve sanal ağlar**.
  1. Altında **sanal ağlar**sanal ağın adını seçin ve ardından **Kaldır**.
  1. **Kaydet**’i seçin.

#### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekleyin

1. Portalda kapsayıcı kayıt defterinize gidin.
1. Altında **ayarları**seçin **güvenlik duvarı ve sanal ağlar**.
1. Zaten yapmadıysanız, erişime izin verecek şekilde seçin **seçili ağlar**. 
1. Altında **sanal ağlar**, hiçbir ağ seçildiğinden emin olun.
1. Altında **Güvenlik Duvarı**, bir VM'nin genel IP adresini girin. Veya CIDR gösteriminde sanal makinenin IP adresini içeren bir adres aralığı girin.
1. **Kaydet**’i seçin.

![Kapsayıcı kayıt defteri için güvenlik duvarı kuralı yapılandırma][acr-vnet-firewall-portal]

Devam [kayıt defterine erişim doğrulayın](#verify-access-to-the-registry).

## <a name="verify-access-to-the-registry"></a>Kayıt defteri erişimi doğrulayın

Güncelleştirilecek yapılandırma için birkaç dakika bekledikten sonra VM kapsayıcı kayıt defterine erişebildiğinizden emin olun. Sanal makinenizde bir SSH bağlantısı ve çalıştırma [az acr oturum açma] [ az-acr-login] kayıt defterinizde oturum açma komutu. 

```bash
az acr login --name mycontainerregistry
```

Gibi kayıt defteri işlemlerini gerçekleştirebilirsiniz `docker pull` kayıt defterinden bir örnek görüntü çekmek için. Bir görüntü ve etiket değeri kayıt defteri oturum açma sunucusu adıyla (tamamı küçük harf) öneki, kayıt defteri için uygun değiştirin:

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker görüntüsünü sanal Makineye başarıyla çeker.

Bu örnek, özel kapsayıcı kayıt defteri ağ erişim kuralı erişebilirsiniz gösterir. Ancak, kayıt defteri, yapılandırılmış bir ağ erişim kuralına sahip olmayan farklı bir oturum açma konaktan erişilemiyor. Kullanarak başka bir konaktan oturum açmaya çalışırsanız `az acr login` komutu veya `docker login` komut çıktısı aşağıdakine benzer:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Varsayılan kayıt defteri erişimini geri yükleme

Varsayılan olarak erişime izin verecek şekilde kayıt defterini geri yüklemek için yapılandırılmış ağ kuralları kaldırın. Daha sonra erişime izin vermek için varsayılan eylem ayarlayın. Azure CLI ve Azure portalını kullanarak eşdeğer adımlar verilmiştir.

### <a name="restore-default-registry-access---cli"></a>Varsayılan kayıt defteri erişim - CLI

#### <a name="remove-network-rules"></a>Ağ kurallarının Kaldır

Kayıt için yapılandırılmış ağ kurallarının bir listesini görmek için aşağıdaki komutu çalıştırın. [az acr ağ kural listesi] [ az-acr-network-rule-list] komutu:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

Yapılandırılan her kuralı Çalıştır [az acr ağ kuralını Kaldır] [ az-acr-network-rule-remove] kaldırmak için komutu. Örneğin:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Erişime izin ver

Aşağıdaki kayıt defterinizin adıyla değiştirin [az acr update] [ az-acr-update] komutu:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Geri yükleme varsayılan kayıt defteri erişim - portal


1. Portalda, kapsayıcı kayıt defterinize gidin ve seçin **güvenlik duvarı ve sanal ağlar**.
1. Altında **sanal ağlar**, her bir sanal ağ seçin ve ardından **Kaldır**.
1. Altında **Güvenlik Duvarı**, her bir adres aralığı seçin ve ardından Sil simgesini seçin.
1. Altında **erişime izin verecek**seçin **tüm ağlar**. 
1. **Kaydet**’i seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm Azure kaynaklarını aynı kaynak grubunda ve artık ihtiyacınız oluşturduysanız, isteğe bağlı olarak kaynakları tek bir kullanarak silebilirsiniz [az grubu Sil](/cli/azure/group) komutu:

```azurecli
az group delete --name myResourceGroup
```

Portalda, kaynakları temizlemek için myResourceGroup kaynak grubuna gidin. Kaynak grubu yüklendikten sonra tıklayarak **kaynak grubunu Sil** kaynak grubunu ve burada depolanan kaynakları kaldırmak için.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok sanal ağ kaynakları ve özellikler bu makalede, ancak kısaca ele alınan. Azure sanal ağ belgeleri, kapsamlı bir şekilde bu konuları kapsamaktadır:

* [Sanal ağ](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Alt ağ](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
