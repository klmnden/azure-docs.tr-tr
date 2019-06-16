---
title: Azure portalında Azure Container Registry depoları
description: Azure portalında Azure Container Registry depoları görüntülemek nasıl.
services: container-registry
author: cristy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/05/2018
ms.author: cristyg
ms.openlocfilehash: 685c978ff206e75d770918f2528a826ad522b706
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64710195"
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Azure portalındaki kapsayıcı kayıt defteri depoları görüntüleme

Azure Container Registry, Docker depolamak için kapsayıcı görüntülerini depolardaki sağlar. Görüntüleri depolarında depolayarak, yalıtılmış ortamlarında grupları resimlerinin (ya da görüntülerini sürümleri) depolayabilirsiniz. Görüntüleri kayıt defterinize gönderin ve Azure portalında içeriklerini görüntülemek, bu depoları belirtebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* **Kapsayıcı kayıt defteri**: Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalında](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Yükleme [Docker] [ docker-install] yerel makinenizde sağlayan, Docker komut satırı arabirimi.
* **Kapsayıcı görüntüsü**: Görüntüyü kapsayıcı kayıt defterinize gönderin. Gönderme ve çekme görüntülerine konusunda yönergeler için bkz [görüntü çekme ve itme](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Azure portalında depoları görüntüleme

Azure portalında görüntüsü etiketlerini yanı sıra, görüntülerinizi barındırma depoları listesini görebilirsiniz.

Adımları izlediyseniz [görüntü çekme ve itme](container-registry-get-started-docker-cli.md) (ve daha sonra görüntüyü silmek olmadı), kapsayıcı kayıt defterinizde bir Nginx görüntüsü olması gerekir. Bu makaledeki yönergeleri belirtilen içinde bir ad alanı, "örnekler" görüntüyü etiketlemek `/samples/nginx`. Bilgilerinizi tazelemeniz olarak [docker itme] [ docker-push] makalesinde belirtilen komutu:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Azure Container Registry gibi çok düzeyli depo ad alanlarından desteklediğinden, belirli bir uygulama ya da farklı geliştirme veya işlem ekipleri için bir uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını kapsamını belirleyebilirsiniz. Daha fazla bilgi için kapsayıcı kayıt defterleri depolarda hakkında bkz: [azure'da özel Docker kapsayıcısı kayıt defterleri](container-registry-intro.md).

Bir depo görüntülemek için:

1. Oturum [Azure portalı][portal]
1. Seçin **Azure Container Registry** olduğu Nginx görüntüsü gönderdiğiniz
1. Seçin **depoları** , kayıt defterindeki görüntüleri içeren depoların listesini görmek için
1. Bu depo içinde görüntüsü etiketlerini görmek için bir depo seçin

Nginx görüntüsü olarak gönderildi, örneğin, belirtildiği [görüntü çekme ve itme](container-registry-get-started-docker-cli.md), benzer bir şey görmeniz gerekir:

![Portalda depoları](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry ile kullanarak, görüntüleme ve Portalı'nda depoları ile çalışma hakkındaki temel bilgileri öğrendiğinize göre deneyin bir [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md) kümesi.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
