---
title: Kapsayıcı görüntülerini Azure Container Registry'ye içeri aktarma
description: Kapsayıcı görüntülerini bir Azure container registry'ye Docker komutlarını çalıştırmak gerek kalmadan Azure API'lerini kullanarak içeri aktarın.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 02/06/2019
ms.author: danlep
ms.openlocfilehash: b8a2280fe82e0f4be8e2812f5494150927642692
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827298"
---
# <a name="import-container-images-to-a-container-registry"></a>Bir kapsayıcı kayıt defterine kapsayıcı görüntüleri içeri aktarma

Docker komutlarını kullanarak olmadan bir Azure container registry'ye (kopya) kapsayıcı görüntülerini kolayca içeri aktarabilirsiniz. Örneğin, bir üretim kayıt defterine geliştirme kayıt defterinden görüntüleri içe veya temel görüntüler genel bir kayıt defterinden kopyalayın.

Azure Container Registry, mevcut bir kayıt defterinden görüntüleri kopyalamak için genel senaryolar sayısı işler:

* Genel bir kayıt defterinden içeri aktarma

* Aynı veya farklı bir Azure aboneliği başka bir Azure container registry'den içeri aktarma

* Azure dışı özel kapsayıcı kayıt defteri alma

Bir Azure kapsayıcı kayıt defterine görüntü alma Docker CLI komutlarını kullanmaya göre aşağıdaki faydaları vardır:

* İstemci ortamınızı yerel bir Docker yükleme gereksinimi olmadığından desteklenen bir işletim sistemi türünden bağımsız olarak herhangi bir kapsayıcı görüntüsünü alın.

* Tüm mimarileri ve bildirim listesinde belirtilen platformlar için görüntüleri (resmi Docker görüntüleri için gibi) çok mimari görüntüleri içeri aktardığınızda kopyalanmasını.

Kapsayıcı görüntüleri içeri aktarmak için bu makalede Azure Cloud Shell'i Azure CLI'da çalıştırın veya yerel olarak gerektirir (2.0.55 sürümü veya üzeri önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

> [!NOTE]
> Aynı kapsayıcı görüntülerini Azure bölgelerinde dağıtmanız gerekiyorsa, Azure Container Registry de destekler [coğrafi çoğaltma](container-registry-geo-replication.md). Coğrafi olarak çoğaltarak kayıt defteri (Premium SKU) gerekli bir, tek bir kayıt defterinden görüntü ve etiket aynı ada sahip birden çok bölgede görebilir.
>

## <a name="prerequisites"></a>Önkoşullar

Azure container registry zaten yoksa, bir kayıt defteri oluşturun. Adımlar için bkz: [hızlı başlangıç: Azure CLI kullanarak bir özel kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md).

Bir Azure container registry'ye görüntü almak için kimliğinizi hedef kayıt defterine yazma izinlerine sahip olmalıdır (en az katkıda bulunan rolü). Bkz: [Azure Container Registry rolleri ve izinleri](container-registry-roles.md). 

## <a name="import-from-a-public-registry"></a>Genel bir kayıt defterinden içeri aktarma

### <a name="import-from-docker-hub"></a>Docker hub'dan içeri aktarma

Örneğin, [az acr alma] [ az-acr-import] çok mimari içeri aktarmak için komutu `hello-world:latest` adlı bir kayıt defterine Docker Hub görüntüsünden *myregistry*. Çünkü `hello-world` resmi bir görüntü Docker Hub'ından bu varsayılan görüntüdür `library` depo. Depo adı ve isteğe bağlı olarak bir etiket değerine dahil `--source` görüntü parametresi. (, İsteğe bağlı olarak bir görüntü, bildirim Özet yerine belirli bir görüntü sürümü garanti etikete göre tanımlayabilirsiniz.)
 
```azurecli
az acr import --name myregistry --source docker.io/library/hello-world:latest --image hello-world:latest
```

Çalıştırarak birden çok bildirimleri bu resimle ilişkili olduğunu doğrulayabilirsiniz `az acr repository show-manifests` komutu:

```azurecli
az acr repository show-manifests --name myregistry --repository hello-world
```

Aşağıdaki örnek bir genel görüntüyü alır `tensorflow` Docker hub'ında depo:

```azurecli
az acr import --name myregistry --source docker.io/tensorflow/tensorflow:latest-gpu --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Microsoft kapsayıcı kayıt defteri alma

Örneğin, en son Windows Server Core görüntüyü almak `windows` Microsoft kapsayıcı kayıt defteri deposu.

```azurecli
az acr import --name myregistry --source mcr.microsoft.com/windows/servercore:latest --image servercore:latest
```

## <a name="import-from-another-azure-container-registry"></a>Başka bir Azure container registry'den içeri aktarma

Tümleşik Azure Active Directory izinlerini kullanarak başka bir Azure container registry'den görüntü içeri aktarabilirsiniz.

* Kimliğinizi Azure Active Directory (Okuyucu rolü) kaynak kayıt defterinden okuma ve kayıt defteri (Katkıda bulunan rolü) hedef yazmak için izinleriniz olmalıdır.

* Kayıt defteri, aynı veya farklı bir Azure aboneliğinde aynı Active Directory kiracısındaki olabilir.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Aynı Abonelikteki bir kayıt defterinden içeri aktarma

Örneğin, içeri aktarma `aci-helloworld:latest` kaynak kayıt defterinden görüntü *mysourceregistry* için *myregistry* aynı Azure aboneliğinde.

```azurecli
az acr import --name myregistry --source mysourceregistry.azurecr.io/aci-helloworld:latest --image hello-world:latest
```

Aşağıdaki örnek bir görüntü tarafından bildirim Özet alır (SHA-256 karma olarak temsil edilir, `sha256:...`) yerine göre etiketleyin:

```azurecli
az acr import --name myregistry --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Farklı Abonelikteki bir kayıt defterinden içeri aktarma

Aşağıdaki örnekte, *mysourceregistry* içinde farklı bir abonelikten *myregistry* aynı Active Directory kiracısındaki. Kaynak kayıt defteri kaynak Kimliğini sağlamanız `--registry` parametresi. Dikkat `--source` parametresi, yalnızca kaynağı depo ve görüntü adını değil kayıt defteri oturum açma sunucusu adını belirtir.
 
```azurecli
az acr import --name myregistry --source sourcerepo/aci-helloworld:latest --image aci-hello-world:latest --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Hizmet sorumlusu kimlik bilgilerini kullanarak bir kayıt defterinden içeri aktarma

Active Directory izinlerini kullanarak erişilemiyor. bir kayıt defterinden içeri aktarmak için hizmet sorumlusu kimlik bilgileri (varsa) kullanabilirsiniz. Uygulama kimliği ve parolası bir Active Directory [hizmet sorumlusu](container-registry-auth-service-principal.md) ACRPull erişim kaynak kayıt defterine sahip. Bir hizmet sorumlusunu kullanarak, yapı sistemi ve kayıt defterinize görüntü almak için gereken diğer katılımsız sistemleri için kullanışlıdır.

```azurecli
az acr import --name myregistry --source sourceregistry.azurecr.io/sourcerepo/sourceimage:tag --image targetimage:tag --username <SP_App_ID> –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Azure dışı özel kapsayıcı kayıt defteri alma

Görüntüyü kayıt defterine çekme erişim sağlayan kimlik bilgileri belirterek özel bir kayıt defterinden içeri aktarın. Örneğin, özel bir Docker kayıt defterinden bir görüntüyü çekin: 

```azurecli
az acr import --name myregistry --source docker.io/sourcerepo/sourceimage:tag --image sourceimage:tag --username <username> --password <password>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kapsayıcı görüntülerini Azure container registry için genel bir kayıt defteri veya başka bir özel kayıt defteri alma hakkında bilgi edindiniz. İçeri aktarma seçenekleri ek görüntü için bkz: [az acr alma] [ az-acr-import] komut başvurusu. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
