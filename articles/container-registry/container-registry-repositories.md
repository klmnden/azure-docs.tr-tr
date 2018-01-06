---
title: "Azure portalında Azure kapsayıcı kayıt defteri depoları"
description: "Azure portalında Azure kapsayıcı kayıt defteri depoları görüntülemek nasıl."
services: container-registry
author: cristy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/05/2018
ms.author: cristyg
ms.openlocfilehash: 593972e972207a27d1232fcb0c1bf220ac3a8def
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Kapsayıcı kayıt defteri depoları Azure portalında görüntüleyin

Azure kapsayıcı kayıt defteri, Docker depolamak için depoları kapsayıcı görüntüleri sağlar. Depoları görüntüleri depolayarak Yalıtılmış ortamlarda grupları görüntüler (veya görüntülerinin sürümlerini) depolayabilirsiniz. Görüntüleri için kayıt anında ve Azure portalında içeriklerini görüntülediğiniz zaman bu depoları belirtebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* **Kapsayıcı kayıt defteri**: Azure aboneliğinizde bir kapsayıcı kayıt oluşturun. Örneğin, [Azure portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: yükleme [Docker] [ docker-install] yerel makinenizde sağlayan, Docker komut satırı arabirimi.
* **Kapsayıcı görüntü**: bir görüntü kapsayıcısı kaydınız anında. Anında iletme ve çekme görüntülerine konusunda yönergeler için bkz [itme ve görüntünün çekme](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Azure portalında görünüm depoları

Azure portalında görüntü etiketleri yanı sıra görüntülerinizi barındırma depoları listesini görebilirsiniz.

' Ndaki adımları izlediyseniz [itme ve görüntünün çekme](container-registry-get-started-docker-cli.md) (ve daha sonra görüntüyü silmek olmadı), kapsayıcı kayıt defterinizde Nginx görüntüsü olması. Bu makaledeki yönergeleri belirtilen içinde bir ad alanı, "örneklerini" görüntüsüyle etiketi `/samples/nginx`. Yenileyici, olarak [docker itme] [ docker-push] makalesinde belirtilen komut:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Azure kapsayıcı kayıt defteri böyle çok düzeyli deposu ad alanları desteklediğinden, belirli bir uygulamayı veya uygulamaları, farklı geliştirme veya işletimsel takımlar koleksiyonu ilgili görüntüleri koleksiyonları kapsamını belirleyebilirsiniz. Daha fazla bilgi için kapsayıcı kayıt defterleri depoları hakkında bkz [özel Docker kapsayıcısı kayıt defterleri azure'da](container-registry-intro.md).

Bir depo görüntülemek için:

1. Oturum [Azure portalı][portal]
1. Seçin **Azure kapsayıcı kayıt defteri** olduğu, gönderilen Nginx görüntüsü
1. Seçin **depoları** kayıt defterinde görüntüleri içeren depoları listesini görmek için
1. Bu havuz içindeki görüntü etiketleri görmek için bir depo seçin

Nginx resim olarak gönderilir, örneğin, belirtildiği [itme ve görüntünün çekme](container-registry-get-started-docker-cli.md), benzer bir şey görmeniz gerekir:

![Portalda depoları](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Sonraki adımlar

Görüntüleme ve Portalı'nda depoları ile çalışma temellerini bildiğinize göre Azure kapsayıcı kayıt defteri ile kullanmayı deneyin bir [Azure kapsayıcı hizmeti (AKS)](../aks/tutorial-kubernetes-prepare-app.md) küme.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
