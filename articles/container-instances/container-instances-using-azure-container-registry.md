---
title: "Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma"
description: "Bir Azure kapsayıcı kayıt defterinde kapsayıcı görüntüleri kullanarak Azure kapsayıcı örnekleri kapsayıcılarında dağıtmayı öğrenin."
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/24/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c69b95f66bf2eaf4975961da5b25f5ac6172798c
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma

Azure kapsayıcı kayıt defteri Docker kapsayıcısı görüntüleri için bir Azure tabanlı, özel kayıt defteri ' dir. Bu makalede, Azure kapsayıcı örneklerine Azure kapsayıcı kayıt defterinde depolanan kapsayıcı yansımalarını dağıtmak alınmaktadır.

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI oluşturmak ve Azure kapsayıcı örnekleri kapsayıcılarında yönetmek için komutlar içerir. Özel bir görüntüde belirtirseniz [az kapsayıcı oluşturmak] [ az-container-create] komutu, kapsayıcı kayıt defteri ile kimlik doğrulaması için gerekli görüntü kayıt defteri parola da belirtebilirsiniz.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

[Az kapsayıcı oluşturmak] [ az-container-create] komutu de destekler belirtme `--registry-login-server` ve `--registry-username`. Ancak, Azure kapsayıcı kayıt için oturum açma sunucusu her zaman olduğu *registryname*. azurecr.io ve varsayılan kullanıcı adı *registryname*, bu değerler görüntü adı yoksa algılanır açıkça sağlanır.

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

Doğrudan şablonunda kapsayıcı kayıt defteri parolanızı depolamak önlemek için onu bir gizli olarak saklamanızı öneririz [Azure anahtar kasası](../key-vault/key-vault-manage-with-cli2.md) ve şablonu kullanarak referans [Azure arasında yerel tümleştirme Resource Manager ve anahtar kasası](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Azure portalı ile dağıtma

Azure kapsayıcı kayıt defteri kapsayıcı görüntülerinde bulunduruyorsanız, Azure kapsayıcı örnekleri Azure portalını kullanarak bir kapsayıcı kolayca oluşturabilirsiniz.

1. Azure portalında kapsayıcı kayıt defterine gidin.

1. Seçin **depoları**, seçin, dağıtmak istediğiniz depo sağ dağıtmak ve seçmek için kapsayıcı görüntü etiketi **örneği Çalıştır**.

    !["Örnek Azure portalında Azure kapsayıcı kayıt defterinde Çalıştır"][acr-runinstance-contextmenu]

1. Kapsayıcı için bir ad ve kaynak grubu için bir ad girin. İsterseniz, varsayılan değerleri de değiştirebilirsiniz.

    ![Azure kapsayıcı örnekleri için menü oluşturma][acr-create-deeplink]

1. Dağıtım tamamlandıktan sonra kapsayıcı grubu için IP adresini ve diğer özellikleri bulmak için bildirimler bölmesinden gidebilirsiniz.

    ![Azure kapsayıcı örnekleri kapsayıcı grubu için ayrıntıları görüntüle][aci-detailsview]

## <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Azure kapsayıcı kayıt defteri için yönetici kullanıcı devre dışı bırakılırsa, Azure Active Directory kullanabilirsiniz [hizmet sorumlusu](../container-registry/container-registry-auth-service-principal.md) kayıt defterine bir kapsayıcı örneği oluştururken, kimlik doğrulaması yapmak için. Kimlik doğrulaması için bir hizmet sorumlusunu kullanarak bir komut dosyası veya katılımsız bir şekilde kapsayıcı örnekleri oluşturan uygulama gibi gözetimsiz senaryolarda da önerilir.

Daha fazla bilgi için bkz: [Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri ile kimlik doğrulama](../container-registry/container-registry-auth-aci.md).

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar oluşturmak, bunları özel kapsayıcı kayıt defterine gönderme ve Azure kapsayıcı örnekleri tarafından dağıtmak öğrenin [öğreticiyi tamamlamak](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create