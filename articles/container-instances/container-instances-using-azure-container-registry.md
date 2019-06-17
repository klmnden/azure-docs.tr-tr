---
title: Azure Container Registry'den Azure Container Instances'a dağıtma
description: Kapsayıcı görüntüleri kullanarak bir Azure container Registry'de Azure Container Instances'a kapsayıcıları dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 01/04/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 515dc8ed4a2fc9b3d2973d393c6894d8c7cef8f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66729392"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Azure Container Registry'den Azure Container Instances'a dağıtma

[Azure Container Registry](../container-registry/container-registry-intro.md) özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir Azure tabanlı, yönetilen bir kapsayıcı kayıt defteri hizmeti. Bu makalede, kapsayıcı görüntülerini Azure Container Instances için bir Azure container Registry'de depolanan dağıtmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar

**Azure kapsayıcı kayıt defteri**: Bir Azure kapsayıcı kayıt defteri--ve en az bir kapsayıcı görüntüsü bu makaledeki adımları tamamlayabilmeniz için kayıt defterinde--gerekir. Bir kayıt defteri gerekirse bkz [Azure CLI kullanarak bir kapsayıcı kayıt defteri oluşturma](../container-registry/container-registry-get-started-azure-cli.md).

**Azure CLI**: Bu komut satırı örnekleri makalesi kullanım [Azure CLI](/cli/azure/) ve Bash kabuğunda biçimlendirilir. Yapabilecekleriniz [Azure CLI'yı yükleme](/cli/azure/install-azure-cli) yerel olarak veya [Azure Cloud Shell][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Kayıt defteri kimlik doğrulamasını yapılandırma

Herhangi bir üretim senaryosunda kullanarak bir Azure kapsayıcı kayıt defterine erişim sağlanmalıdır [hizmet sorumluları](../container-registry/container-registry-auth-service-principal.md). Hizmet sorumluları izin vermenizi [rol tabanlı erişim denetimi](../container-registry/container-registry-roles.md) kapsayıcı görüntülerinizi için. Örneğin, bir hizmet sorumlusunu bir kayıt defterine yalnızca çekme erişimiyle yapılandırabilirsiniz.

Aşağıdaki bölümde, bir Azure anahtar kasası ve hizmet sorumlusu oluşturun ve hizmet sorumlusunun kimlik bilgileri Kasası'nda depolayın. 

### <a name="create-key-vault"></a>Anahtar kasası oluşturma

[Azure Key Vault](../key-vault/key-vault-overview.md) içinde henüz bir kasanız yoksa, aşağıdaki komutları kullanarak Azure CLI ile bir kasa oluşturun.

Güncelleştirme `RES_GROUP` değişkenini, anahtar kasası oluşturmak bir kaynak grubunun adıyla ve `ACR_NAME` kapsayıcı kayıt defterinizin adıyla. Yeni anahtar kasanıza için bir ad belirtin `AKV_NAME`. Kasa adı Azure'da benzersiz olmalı ve 3 ila 24 alfasayısal karakterden uzunluğu, bir başlamalı, harf veya rakam ile başlamalı ve art arda kısa çizgi içeremez.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Hizmet sorumlusu oluşturma ve kimlik bilgilerini depolama

Şimdi bir hizmet sorumlusu oluşturup kimlik bilgilerini anahtar kasanızda depolamanız gerekiyor.

Aşağıdaki komutu kullanır [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] hizmet sorumlusu oluşturma ve [az keyvault secret set] [ az-keyvault-secret-set] depolamak için Hizmet sorumlusunun **parola** kasadaki.

```azurecli
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name http://$ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)
```

`--role` Önceki komutta bağımsız değişkeni ile hizmet sorumlusu yapılandırır *acrpull* rolü çoğaltılmadığı kayıt defterine erişim verir. Hem İtme hem de çekme erişim vermek için değiştirme `--role` bağımsız değişkeni *acrpush*.

Ardından, hizmet sorumlusunun depolamak *AppID* Kasası'nda olduğu **kullanıcıadı** kimlik doğrulaması için Azure Container Registry'ye geçirin.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Bir Azure Anahtar Kasası oluşturdunuz ve içinde iki gizli dizi depoladınız:

* `$ACR_NAME-pull-usr`: Kapsayıcı kayıt defteri olarak kullanılmak üzere hizmet sorumlusu kimliği **username**.
* `$ACR_NAME-pull-pwd`: Kapsayıcı kayıt defteri olarak kullanılmak üzere hizmet sorumlusu parolası **parola**.

Artık siz veya uygulamalarınız ve hizmetleriniz kayıt defterinden görüntüleri çektiğinde bu gizli dizilere ada göre başvurabilirsiniz.

## <a name="deploy-container-with-azure-cli"></a>Azure CLI ile kapsayıcı dağıtma

Hizmet sorumlusu kimlik bilgileri Azure Key Vault gizli dizileri depolanır, uygulamalarınızı ve hizmetlerinizi bunları özel kayıt erişmek için kullanabilirsiniz.

İlk kullanarak kayıt defterinin oturum açma sunucusu adını alın [az acr show] [ az-acr-show] komutu. Tümü küçük harf ve benzer şekilde, oturum açma sunucusu adını `myregistry.azurecr.io`.

```azurecli
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --resource-group $RES_GROUP --query "loginServer" --output tsv)
```

Bir kapsayıcı örneği dağıtmak için aşağıdaki [az container create][az-container-create] komutunu yürütün. Komut, kapsayıcı kayıt defterinizde kimlik doğrulaması için Azure Key Vault'ta depolanan bir hizmet sorumlusunun kimlik bilgilerini kullanır ve daha önce gönderdiğiniz varsayılır [aci-helloworld](container-instances-quickstart.md) kayıt defterinize görüntü. Güncelleştirme `--image` kayıt defterinizden farklı bir görüntü kullanmak istiyorsanız, değer.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_LOGIN_SERVER/aci-helloworld:v1 \
    --registry-login-server $ACR_LOGIN_SERVER \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

`--dns-name-label` Önceki komutta kapsayıcının DNS ad etiketi için rastgele bir sayı ekler. Bu nedenle değer Azure içinde benzersiz olmalıdır. Komutun çıktısı, kapsayıcının tam etki alanı adını (FQDN) gösterir, örneğin:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_LOGIN_SERVER/aci-helloworld:v1 --registry-login-server $ACR_LOGIN_SERVER --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Kapsayıcı başarıyla başlatıldıktan sonra uygulamanın başarıyla çalıştığını doğrulamak için tarayıcınızda FQDN'sine gidebilirsiniz.

## <a name="deploy-with-azure-resource-manager-template"></a>Azure Resource Manager şablonu ile dağıtma

Azure Container Registry'nize özelliklerini dahil ederek bir Azure Resource Manager şablonunda belirtebilirsiniz `imageRegistryCredentials` kapsayıcı grubu tanımı özelliği:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Resource Manager şablonu Azure Key Vault gizli dizileri başvurma hakkında daha fazla bilgi için bkz [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Azure portal ile dağıtma

Bir Azure container registry'den kapsayıcı görüntüleri sahipseniz, Azure portalını kullanarak Azure Container Instances bir kapsayıcı kolayca oluşturabilirsiniz. Bir kapsayıcı kayıt defterinden bir kapsayıcı örneği dağıtmak için portal'ı kullanırken, kayıt defterindeki etkinleştirmelisiniz [yönetici hesabı](../container-registry/container-registry-authentication.md#admin-account). Yönetici hesabı, test etmek için çoğunlukla kayıt defterine erişmek tek bir kullanıcı için tasarlanmıştır. 

1. Azure portalında kapsayıcı kayıt defterinize gidin.

1. Yönetici hesabı etkin olduğunu doğrulamak için şunu seçin **erişim anahtarları**, altında **yönetici kullanıcı** seçin **etkinleştirme**.

1. Seçin **depoları**, seçin, dağıtmak istediğiniz depoya sağ kapsayıcı görüntüsünü dağıtmak ve istediğiniz etiketi **örneği Çalıştır**.

    !["Azure portalında Azure Container Registry'de örneği Çalıştır"][acr-runinstance-contextmenu]

1. Kapsayıcı için bir ad ve kaynak grubu için bir ad girin. İsterseniz, varsayılan değerleri değiştirebilirsiniz.

    ![Azure Container Instances için menü oluşturma][acr-create-deeplink]

1. Dağıtım tamamlandığında kapsayıcı grubuna IP adresini ve diğer özellikleri bulmak için bildirimler bölmesinden gidebilirsiniz.

    ![Azure Container Instances'a kapsayıcı grubu için ayrıntıları görüntüle][aci-detailsview]

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure container registry ile kimlik doğrulama](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-try-it]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
