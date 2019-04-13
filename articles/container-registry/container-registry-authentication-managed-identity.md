---
title: Yönetilen bir kimlikle Azure Container Registry kimlik doğrulaması
description: Bir kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen Azure kimliği kullanılarak özel kapsayıcı kayıt defterinizde görüntülerine erişim sağlar.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 01/16/2019
ms.author: danlep
ms.openlocfilehash: 728a2f8cf61bbe0691350b9de45a5fab6b90cadb
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526631"
---
# <a name="use-an-azure-managed-identity-to-authenticate-to-an-azure-container-registry"></a>Azure yönetilen bir Azure container registry'ye kimliğini doğrulamak için kimliği kullan 

Kullanım bir [yönetilen Azure kaynakları için kimliği](../active-directory/managed-identities-azure-resources/overview.md) sağlayın veya kayıt defteri kimlik bilgilerini yönetmek bir Azure container registry'ye olmadan başka bir Azure kaynak kimliğini doğrulamak için gerek. Örneğin, kullanıcı tarafından atanan veya sistem tarafından atanan bir yönetilen kimlik bir Linux sanal makinesinde erişim kapsayıcı görüntüleri için kapsayıcı kayıt defterinden genel bir kayıt defteri kullanmak kadar kolay ayarlayın.

Bu makalede, yönetilen kimlikleri hakkında daha fazla bilgi edinmek ve nasıl yapılır:

> [!div class="checklist"]
> * Azure VM'de bir kullanıcı tarafından atanan veya sistem tarafından atanan kimliği etkinleştirme
> * Azure container registry kimlik erişimi verme
> * Yönetilen kimlik kayıt defterine erişim için bir kapsayıcı görüntüsü çekin kullanın 

Azure kaynaklarını oluşturmak için bu makalede, Azure CLI Sürüm 2.0.55 çalıştırmanızı gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Bir kapsayıcı kayıt defterini ayarlamak ve kapsayıcı resmini gönderirsiniz için Docker'ın yerel olarak yüklü olmalıdır. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="why-use-a-managed-identity"></a>Yönetilen bir kimlik neden kullanmalısınız?

Azure kaynakları için yönetilen bir kimlik Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yapılandırabileceğiniz [belirli Azure kaynaklarına](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), yönetilen bir kimliğe sahip sanal makineler de dahil olmak üzere. Ardından, kod veya betiklerde kimlik bilgilerini geçmeden diğer Azure kaynaklarına erişmek için kimliğini kullanın.

Yönetilen kimlik iki türleri şunlardır:

* *Kullanıcı tarafından atanan kimlikleri*, birden çok kaynağa atayın ve istediğiniz sürece için kalıcı. Kullanıcı tarafından atanan kimlikleri, şu anda Önizleme aşamasındadır.

* A *sistem yönetilen kimliği*, tek bir sanal makine gibi belirli bir kaynağa benzersizdir ve bu kaynağın ömrü boyunca sürer.

Yönetilen bir kimlik ile bir Azure kaynağı ayarladıktan sonra kimlik herhangi bir güvenlik sorumlusu gibi başka bir kaynak için istediğiniz erişimi sağlar. Örneğin, bir yönetilen kimlik azure'da özel bir kayıt defteri çekme, gönderme ve çekme veya diğer izinler sahip bir rol atayın. (Kayıt defteri rolleri tam bir listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).) Bir veya daha fazla kaynak için bir kimlik erişimi verebilirsiniz.

Ardından, herhangi bir kimlik doğrulaması için kimlik kullanmak [Azure AD kimlik doğrulamasını destekleyen hizmet](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication), kodunuzdaki herhangi bir kimlik bilgisi olmadan. Bir sanal makineden bir Azure container registry erişmek için kimlik kullanmak için Azure Resource Manager ile kimlik doğrulaması. Senaryonuza bağlı olarak yönetilen kimlik kullanarak kimlik doğrulaması yapmayı seçin:

* [Azure AD erişim belirteci alma](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) programlı olarak HTTP ya da REST çağrılarını kullanma

* Kullanım [Azure SDK'ları](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md)

* [Azure CLI veya PowerShell oturum](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md) kimliğe sahip. 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Azure container registry zaten yoksa, bir kayıt defteri oluşturmak ve bir örnek kapsayıcı resmini gönderirsiniz. Adımlar için bkz: [hızlı başlangıç: Azure CLI kullanarak bir özel kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md).

Bu makalede, sahip olduğunuz varsayılır `aci-helloworld:v1` , kayıt defterindeki kapsayıcı görüntüsü. Bir kayıt defteri adını örneklerde *myContainerRegistry*. Kendi kayıt defteri ve sonraki adımlarda görüntü adlarını değiştirin.

## <a name="create-a-docker-enabled-vm"></a>Docker özellikli VM oluşturma

Bir Docker özellikli bir Ubuntu sanal makinesi oluşturun. Ayrıca yüklemeniz gerekir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) sanal makinede. Bir Azure sanal makinesi zaten varsa, sanal makine oluşturmak için bu adımı atlayın.

Varsayılan dağıtma Ubuntu Azure sanal makine ile [az vm oluşturma][az-vm-create]. Aşağıdaki örnekte adlı bir VM oluşturur *myDockerVM* var olan bir kaynak grubunda *myResourceGroup*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’nin oluşturulması birkaç dakika sürer. Komut tamamlandığında, Not `publicIpAddress` Azure CLI tarafından görüntülenen. Bu adres, VM'ye SSH bağlantısı yapmak için kullanın.

### <a name="install-docker-on-the-vm"></a>Docker'ı VM'ye yükleyin

Sanal makine çalışmaya başladıktan sonra bir SSH bağlantısı VM'ye olun. Değiştirin *Publicıpaddress* sanal makinenizin genel IP adresiyle.

```bash
ssh azureuser@publicIpAddress
```

Docker VM üzerinde yüklemek için aşağıdaki komutu çalıştırın:

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

Bağlantısındaki [apt ile Azure CLI yükleme](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) Ubuntu sanal makinenizde Azure CLI'yı yüklemek için. Bu makalede 2.0.55 sürümünü yüklediğinizden emin olun veya üzeri.

SSH oturumundan çıkın.

## <a name="example-1-access-with-a-user-assigned-identity"></a>Örnek 1: Bir kullanıcı tarafından atanan kimlikle erişimi

### <a name="create-an-identity"></a>Bir kimlik oluşturun

Bir kimlik kullanarak abonelik oluşturma [az kimliği oluşturma](/cli/azure/identity?view=azure-cli-latest#az-identity-create) komutu. Daha önce kapsayıcı kayıt defteri veya sanal makine ya da farklı bir tane oluşturmak için kullanılan aynı kaynak grubunu kullanabilirsiniz.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRId
```

Aşağıdaki adımlarda kimliğini yapılandırmak için kullanın [az kimlik show] [ az-identity-show] kimliğin kaynağı kimliği ve hizmet sorumlusu kimliği değişkenlerinde depolanacak komutu.

```azurecli
# Get resource ID of the user-assigned identity
userID=$(az identity show --resource-group myResourceGroup --name myACRId --query id --output tsv)

# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACRId --query principalId --output tsv)
```

Sanal makineniz için CLI'yı oturum açtığınızda kimliğin kimliği sonraki bir adımda gerektiğinden, değer göster:

```bash
echo $userID
```

Kimliği formu şöyledir:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRId
```

### <a name="configure-the-vm-with-the-identity"></a>Kimlikle VM yapılandırma

Aşağıdaki [az vm kimliği atamak] [ az-vm-identity-assign] komut, kullanıcı tarafından atanan kimlikle Docker VM'nizi yapılandırır:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM --identities $userID
```

### <a name="grant-identity-access-to-the-container-registry"></a>Kapsayıcı kayıt defteri kimlik erişimi verme

Artık, kapsayıcı kayıt defterine erişim için kimlik yapılandırma. İlk olarak [az acr show] [ az-acr-show] komutu kayıt defteri kaynak Kimliğini alın:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Kullanım [az rol ataması oluşturma] [ az-role-assignment-create] kayıt defterine AcrPull rol atamak için komutu. Bu rol sağlar [çekme izinlerini](container-registry-roles.md) kayıt. Her iki çekme sağlamak ve izinleri anında iletme ACRPush rol atayın.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Kimlik kayıt defterine erişim için kullanın

SSH, kimlikle yapılandırılmış Docker sanal makineye uygulayın. Azure VM'de yüklü CLI'yı kullanarak aşağıdaki Azure CLI komutları çalıştırın.

İlk olarak, Azure CLI ile kimlik doğrulaması [az login][az-login], VM üzerinde yapılandırılan kimliği kullanarak. İçin `<userID>`, önceki adımda aldığınız kimlik Kimliğini değiştirin. 

```azurecli
az login --identity --username <userID>
```

Ardından, kayıt defteri ile kimlik doğrulaması [az acr oturum açma][az-acr-login]. Bu komutu kullandığınızda, CLI'yı çalıştırdığınızda oluşturulan Active Directory belirteci kullanan `az login` sorunsuz bir şekilde oturumunuz kapsayıcı kayıt defteri ile kimlik doğrulaması için. (Sanal makinenizin Kurulum bağlı olarak, bu komut ve docker komutları ile çalıştırmanız gerekebilir `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Görmelisiniz bir `Login succeeded` ileti. Daha sonra çalıştırabileceğiniz `docker` kimlik bilgileri olmadan komutları. Örneğin, [docker isteği] [ docker-pull] çekme `aci-helloworld:v1` kayıt defterinizin oturum açma sunucusu adını belirterek görüntüsü. Oturum açma sunucusu adını, ardından kapsayıcı kayıt defterinizin adı (tümü küçük harf) oluşan `.azurecr.io` - Örneğin, `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="example-2-access-with-a-system-assigned-identity"></a>Örnek 2: Sistem tarafından atanan bir kimlikle erişimi

### <a name="configure-the-vm-with-a-system-managed-identity"></a>Sistem tarafından yönetilen bir kimlikle VM yapılandırma

Aşağıdaki [az vm kimliği atamak] [ az-vm-identity-assign] komut Docker sanal makinenizin sistem tarafından atanan bir kimlikle yapılandırır:

```azurecli
az vm identity assign --resource-group myResourceGroup --name myDockerVM 
```

Kullanma [az vm show] [ az-vm-show] değeri olarak bir değişken ayarlamak için komutu `principalId` (hizmet sorumlusu kimliği) sanal makinenin kimliğini, sonraki adımlarda kullanılacak.

```azurecli-interactive
spID=$(az vm show --resource-group myResourceGroup --name myDockerVM --query identity.principalId --out tsv)
```

### <a name="grant-identity-access-to-the-container-registry"></a>Kapsayıcı kayıt defteri kimlik erişimi verme

Artık, kapsayıcı kayıt defterine erişim için kimlik yapılandırma. İlk olarak [az acr show] [ az-acr-show] komutu kayıt defteri kaynak Kimliğini alın:

```azurecli
resourceID=$(az acr show --resource-group myResourceGroup --name myContainerRegistry --query id --output tsv)
```

Kullanım [az rol ataması oluşturma] [ az-role-assignment-create] kimliğine AcrPull rol atamak için komutu. Bu rol sağlar [çekme izinlerini](container-registry-roles.md) kayıt. Her iki çekme sağlamak ve izinleri anında iletme ACRPush rol atayın.

```azurecli
az role assignment create --assignee $spID --scope $resourceID --role acrpull
```

### <a name="use-the-identity-to-access-the-registry"></a>Kimlik kayıt defterine erişim için kullanın

SSH, kimlikle yapılandırılmış Docker sanal makineye uygulayın. Azure VM'de yüklü CLI'yı kullanarak aşağıdaki Azure CLI komutları çalıştırın.

İlk olarak, Azure CLI ile kimlik doğrulaması [az login][az-login], sanal makinede sistem tarafından atanan kimlik kullanarak.

```azurecli
az login --identity
```

Ardından, kayıt defteri ile kimlik doğrulaması [az acr oturum açma][az-acr-login]. Bu komutu kullandığınızda, CLI'yı çalıştırdığınızda oluşturulan Active Directory belirteci kullanan `az login` sorunsuz bir şekilde oturumunuz kapsayıcı kayıt defteri ile kimlik doğrulaması için. (Sanal makinenizin Kurulum bağlı olarak, bu komut ve docker komutları ile çalıştırmanız gerekebilir `sudo`.)

```azurecli
az acr login --name myContainerRegistry
```

Görmelisiniz bir `Login succeeded` ileti. Daha sonra çalıştırabileceğiniz `docker` kimlik bilgileri olmadan komutları. Örneğin, [docker isteği] [ docker-pull] çekme `aci-helloworld:v1` kayıt defterinizin oturum açma sunucusu adını belirterek görüntüsü. Oturum açma sunucusu adını, ardından kapsayıcı kayıt defterinizin adı (tümü küçük harf) oluşan `.azurecr.io` - Örneğin, `mycontainerregistry.azurecr.io`.

```
docker pull mycontainerregistry.azurecr.io/aci-helloworld:v1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Container Registry ile kullanarak yönetilen kimlikleri hakkında öğrendiniz ve nasıl yapılır:

> [!div class="checklist"]
> * Azure VM'de bir kullanıcı tarafından atanan veya sistem tarafından atanan kimliği etkinleştirme
> * Azure container registry kimlik erişimi verme
> * Yönetilen kimlik kayıt defterine erişim için bir kapsayıcı görüntüsü çekin kullanın

* Daha fazla bilgi edinin [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/).


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-vm-show]: /cli/azure/vm#az-vm-show
[az-vm-identity-assign]: /cli/azure/vm/identity#az-vm-identity-assign
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
