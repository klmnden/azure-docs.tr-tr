---
title: Azure Container Registry'de kayıt durumunu denetleme
description: Yerel Docker yapılandırmasını ve kayıt defteri bağlantısı içeren bir Azure kapsayıcı kayıt defteri kullanırken sık karşılaşılan sorunları belirlemek için hızlı bir tanılama komutu çalıştırmayı öğrenin
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 3e5b5467f9fa25e23f6661c6630d346aa85e2205
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555104"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Azure container registry durumunu denetleyin

Azure container registry kullanırken bazen sorunlarla karşılaşabilirsiniz. Örneğin, yerel ortamınızda Docker ile ilgili bir sorun nedeniyle bir kapsayıcı görüntüsü çekin mümkün olmayabilir. Veya bir ağ sorunu kayıt defterine bağlanmanızı engelleyebilir. 

İlk tanılama adımı olarak çalıştırabilir [az acr onay durumunu][az-acr-check-health] command to get information about the health of the environment and optionally access to a target registry. This command is available in Azure CLI version 2.0.67 or later. If you need to install or upgrade, see [Install Azure CLI][azure-cli].

## <a name="run-az-acr-check-health"></a>Az acr onay durumunu çalıştırın

Aşağıdaki örnekler çalıştırmak için farklı yollar gösterilmiştir `az acr check-health` komutu.

> [!NOTE]
> Azure Cloud Shell'de komutu çalıştırırsanız, yerel ortam denetlenmez. Ancak, bir hedef kayıt defteri erişimi denetleyebilirsiniz.

### <a name="check-the-environment-only"></a>Yalnızca ortamını denetle

Yerel Docker denetlemek için arka plan programı, CLI sürümü ve Helm istemci yapılandırması komutu ek parametre olmadan çalıştırın:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Ortamı ve bir hedef kayıt defteri denetleme

Yerel ortam denetimleri gerçekleştirmek yanı sıra bir kayıt defterine erişim denetimi için bir hedef kayıt defteri adını geçirin. Örneğin:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Hata Raporlama

Komutu, standart çıktıya bilgileri günlüğe kaydeder. Bir sorun algılanırsa, bir hata kodu ve bir açıklama sağlar. Kodları ve olası çözümleri hakkında daha fazla bilgi için bkz: [hata başvurusu](container-registry-health-error-reference.md).

Bir hata bulduğunda, varsayılan olarak, komut durdurur. Tüm sistem durumu denetimleri için çıkış sağlar, böylece hata bulunması halinde komutu çalıştırabilirsiniz. Ekleme `--ignore-errors` parametresi, aşağıdaki örneklerde gösterildiği gibi:

```azurecli
# Check environment only
az acr check-health --ignore-errors

# Check environment and target registry
az acr check-health --name myregistry --ignore-errors
```      

Örnek çıktı:

```console
$ az acr check-health --name myregistry --ignore-errors --yes

Docker daemon status: available
Docker version: Docker version 18.09.2, build 6247962
Docker pull of 'mcr.microsoft.com/mcr/hello-world:latest' : OK
ACR CLI version: 2.2.9
Helm version:
Client: &version.Version{SemVer:"v2.14.1", GitCommit:"5270352a09c7e8b6e8c9593002a73535276507c0", GitTreeState:"clean"}
DNS lookup to myregistry.azurecr.io at IP 40.xxx.xxx.162 : OK
Challenge endpoint https://myregistry.azurecr.io/v2/ : OK
Fetch refresh token for registry 'myregistry.azurecr.io' : OK
Fetch access token for registry 'myregistry.azurecr.io' : OK
```  



## <a name="next-steps"></a>Sonraki adımlar

Tarafından döndürülen hata kodları hakkında daha fazla ayrıntı için [az acr onay durumunu][az-acr-check-health] komutu, bkz: [sistem durumu denetimi hata başvurusu](container-registry-health-error-reference.md).

Bkz: [SSS](container-registry-faq.md) sık sorulan sorular ve Azure Container Registry hakkında bilinen diğer sorunlar için.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
