---
title: "Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma | Azure belgeleri"
description: "Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: aa1c4ea379c10dff246e2f924a345f9fa444aa64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri dağıtma

Azure kapsayıcı kayıt defteri Docker kapsayıcısı görüntüleri için bir Azure tabanlı, özel kayıt defteri ' dir. Bu makalede, Azure kapsayıcı örneklerine Azure kapsayıcı kayıt defterinde depolanan kapsayıcı yansımalarını dağıtmak alınmaktadır.

## <a name="using-the-azure-cli"></a>Azure CLI kullanma

Azure CLI oluşturmak ve Azure kapsayıcı örnekleri kapsayıcılarında yönetmek için komutlar içerir. Özel bir görüntüde belirtirseniz `create` komutu, kapsayıcı kayıt defteri ile kimlik doğrulaması için gerekli görüntü kayıt defteri parola da belirtebilirsiniz.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

`create` Komutu de destekler belirtme `registry-login-server` ve `registry-username`. Ancak, Azure kapsayıcı kayıt için oturum açma sunucusu her zaman olduğu *registryname*. azurecr.io ve varsayılan kullanıcı adı *registryname*, bu değerler görüntü adı yoksa algılanır açıkça sağlanır.

## <a name="using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak

Ekleyerek bir Azure Resource Manager şablonu Azure kapsayıcı kaydınız özelliklerini belirtebilirsiniz `imageRegistryCredentials` kapsayıcısı Grup tanımında özelliği:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Doğrudan şablonunda kapsayıcı kayıt defteri parolanızı depolamak önlemek için onu bir gizli olarak saklamanızı öneririz [Azure anahtar kasası](../key-vault/key-vault-manage-with-cli2.md) ve şablonu kullanarak referans [Azure arasında yerel tümleştirme Resource Manager ve anahtar kasası](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-the-azure-portal"></a>Azure portalını kullanma

Azure kapsayıcı kayıt defteri kapsayıcı görüntülerinde bulunduruyorsanız, Azure kapsayıcı örnekleri Azure portalını kullanarak bir kapsayıcı kolayca oluşturabilirsiniz.

1. Azure portalında kapsayıcı kayıt defterine gidin.

2. Depoları seçin.

    ![Azure portalında Azure kapsayıcı kayıt defteri menüsü][acr-menu]

3. Dağıtım yapmak istediğiniz depo seçin.

4. Dağıtmak istediğiniz kapsayıcı görüntü etiketi sağ tıklayın.

    ![Kapsayıcı Azure kapsayıcı örnekleri ile başlatmak için bağlam menüsü][acr-runinstance-contextmenu]

5. Kapsayıcı için bir ad ve kaynak grubu için bir ad girin. İsterseniz, varsayılan değerleri de değiştirebilirsiniz.

    ![Azure kapsayıcı örnekleri için menü oluşturma][acr-create-deeplink]

6. Dağıtım tamamlandıktan sonra kapsayıcı grubu için IP adresini ve diğer özellikleri bulmak için bildirimler bölmesinden gidebilirsiniz.

    ![Azure kapsayıcı örnekleri kapsayıcı grubu için ayrıntıları görüntüle][aci-detailsview]

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılar oluşturmak, bunları özel kapsayıcı kayıt defterine gönderme ve Azure kapsayıcı örnekleri tarafından dağıtmak öğrenin [öğreticiyi tamamlamak](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
