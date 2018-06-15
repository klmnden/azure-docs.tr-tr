---
title: Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma
description: Azure kapsayıcı kayıt defterinde kapsayıcı görüntüleri kullanarak Azure kapsayıcı örnekleri kapsayıcılarında dağıtmayı öğrenin.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 7a7d2aa61f25bc4782c6a1a6744e329935477f8c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32168124"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma

Azure kapsayıcı kayıt defteri Azure tabanlı, özel bir kayıt defteri, Docker kapsayıcısı görüntüleri ' dir. Bu makalede, Azure kapsayıcı örnekleri için bir Azure kapsayıcı kayıt defterinde depolanan kapsayıcı görüntülerini dağıtmayı açıklar.

## <a name="prerequisites"></a>Önkoşullar

**Azure kapsayıcı kayıt defteri**: bir Azure kapsayıcı kayıt defteri--ve bu makaledeki adımları tamamlamak için kayıt defteri--en az bir kapsayıcı görüntüde gerekir. Bir kayıt defteri gerekirse bkz [Azure CLI kullanarak bir kapsayıcı kayıt](../container-registry/container-registry-get-started-azure-cli.md).

**Azure CLI**: Bu komut satırı örnekleri makalesi kullanım [Azure CLI](/cli/azure/) ve Bash kabuğunda biçimlendirilir. Yapabilecekleriniz [Azure CLI yükleme](/cli/azure/install-azure-cli) yerel olarak veya [Azure bulut Kabuk][cloud-shell-bash].

## <a name="configure-registry-authentication"></a>Kayıt defteri kimlik doğrulamasını yapılandırma

Hiçbir üretim senaryosu kullanarak Azure kapsayıcı kayıt defterine erişim verilmelidir [hizmet sorumluları](../container-registry/container-registry-auth-service-principal.md). Hizmet sorumluları, kapsayıcı görüntüleriniz için rol tabanlı erişim denetimi sağlamak izin verir. Örneğin, bir kayıt defteri çoğaltılmadığı erişimi olan bir hizmet sorumlusu yapılandırabilirsiniz.

Bu bölümde, Azure anahtar kasası ve hizmet sorumlusu oluşturun ve hizmet sorumlusuna ilişkin kimlik bilgilerini kasaya depolamak.

### <a name="create-key-vault"></a>Anahtar kasası oluştur

Zaten bir kasa yoksa, [Azure anahtar kasası](/azure/key-vault/), aşağıdaki komutları kullanarak Azure CLI ile oluşturun.

Güncelleştirme `RES_GROUP` anahtar kasası oluşturulacağı kaynak grubunun adı ile değişken ve `ACR_NAME` kapsayıcı kayıt adı. Yeni anahtar kasasına için bir ad belirtin `AKV_NAME`. Kasa adı Azure içinde benzersiz olmalıdır ve 3-24 alfasayısal karakter olmalıdır uzunluğu, son harf veya rakam, bir harf ile başlamalı ve art arda kısa çizgi içeremez.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Hizmet sorumlusu oluşturma ve kimlik bilgilerini depolamak

Şimdi bir hizmet sorumlusu oluşturmak ve anahtar kasanızı kimlik bilgilerini depolamak gerekir.

Aşağıdaki komut kullanır [az ad sp oluşturma-için-rbac] [ az-ad-sp-create-for-rbac] hizmet sorumlusu oluşturmak için ve [az keyvault gizli kümesi] [ az-keyvault-secret-set] depolamak için Hizmet sorumlusuna ilişkin **parola** kasadaki.

```azurecli
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

`--role` Yukarıdaki komut bağımsız değişkeni ile hizmet sorumlusu yapılandırır *okuyucu* çoğaltılmadığı kayıt defterine erişim verir rol. Hem İtme hem de çekme erişim vermek için değiştirme `--role` bağımsız değişkeni *katkıda bulunan*.

Ardından, hizmet sorumlusuna ilişkin depolamak *AppID* kasasına olduğu **kullanıcıadı** kimlik doğrulaması için Azure kapsayıcı kayıt defterine geçirin.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Azure anahtar kasası oluşturulur ve iki gizli depolanan:

* `$ACR_NAME-pull-usr`: Kapsayıcı kayıt defteri olarak kullanmak için hizmet asıl kimliği **kullanıcıadı**.
* `$ACR_NAME-pull-pwd`: Kapsayıcı kayıt defteri olarak kullanmak için hizmet asıl parolası **parola**.

Ya da, uygulama ve hizmetlerinize görüntüleri kayıt defterinden çekme zaman ada göre şimdi bu Sırları başvuruda bulunabilir.

## <a name="deploy-container-with-azure-cli"></a>Azure CLI ile kapsayıcı dağıtma

Azure anahtar kasası parolalarında depolanan hizmet asıl kimlik bilgileri, uygulamaları ve Hizmetleri bunları özel kayıt defteri erişmek için kullanabilirsiniz.

Aşağıdaki yürütme [az kapsayıcı oluşturmak] [ az-container-create] bir kapsayıcı örnek dağıtmak için komutu. Komut, kapsayıcı kayıt defterine kimliğini doğrulamak için Azure anahtar kasasında depolanan hizmet sorumlusuna ilişkin kimlik bilgilerini kullanır ve daha önce gönderilen varsayılır [aci helloworld](container-instances-quickstart.md) kaydınız görüntüye. Güncelleştirme `--image` kayıt defterinden farklı bir resim kullanmak istediğiniz değeri.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_NAME.azurecr.io/aci-helloworld:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

`--dns-name-label` Yukarıdaki komut kapsayıcının DNS ad etiketi için rastgele bir sayı ekler için değer Azure içinde benzersiz olmalıdır. Örneğin, komut çıktısı kapsayıcının tam etki alanı adı (FQDN) görüntüler:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_NAME.azurecr.io/aci-helloworld:v1 --registry-login-server $ACR_NAME.azurecr.io --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Kapsayıcı başarıyla başladıktan sonra uygulamanın başarıyla çalıştığından doğrulamak için tarayıcınızda sunucunun FQDN'sini gidebilirsiniz.

## <a name="deploy-with-azure-resource-manager-template"></a>Azure Resource Manager şablonu ile dağıtma

Ekleyerek bir Azure Resource Manager şablonu Azure kapsayıcı kaydınız özelliklerini belirtebilirsiniz `imageRegistryCredentials` kapsayıcısı Grup tanımında özelliği:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Resource Manager şablonu gizli anahtarları Azure anahtar kasası başvuran hakkında daha fazla bilgi için bkz [dağıtım sırasında güvenli parametre değeri geçirmek için kullanım Azure anahtar kasası](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Azure portalı ile dağıtma

Azure kapsayıcı kayıt defteri kapsayıcı görüntülerinde bulunduruyorsanız, Azure kapsayıcı örnekleri Azure portalını kullanarak bir kapsayıcı kolayca oluşturabilirsiniz.

1. Azure portalında kapsayıcı kayıt defterine gidin.

1. Seçin **depoları**, seçin, dağıtmak istediğiniz depo sağ dağıtmak ve seçmek için kapsayıcı görüntü etiketi **örneği Çalıştır**.

    !["Örnek Azure portalında Azure kapsayıcı kayıt defterinde Çalıştır"][acr-runinstance-contextmenu]

1. Kapsayıcı için bir ad ve kaynak grubu için bir ad girin. İsterseniz, varsayılan değerleri de değiştirebilirsiniz.

    ![Azure kapsayıcı örnekleri için menü oluşturma][acr-create-deeplink]

1. Dağıtım tamamlandıktan sonra kapsayıcı grubu için IP adresini ve diğer özellikleri bulmak için bildirimler bölmesinden gidebilirsiniz.

    ![Azure kapsayıcı örnekleri kapsayıcı grubu için ayrıntıları görüntüle][aci-detailsview]

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı kayıt defteri kimlik doğrulaması hakkında daha fazla bilgi için bkz: [Azure kapsayıcı kayıt defteri ile kimlik doğrulama](../container-registry/container-registry-authentication.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-powershell]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az_container_create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set