---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 09eaf9465ec3912dea6e1f3ee1693f6bfed50abc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188927"
---
## <a name="push-image-to-registry"></a>Kayıt defterine görüntü gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Henüz yerel kapsayıcı görüntünüz yoksa, aşağıdaki komutu çalıştırın. [docker isteği] [ docker-pull] Docker hub'dan mevcut bir görüntüyü çekmek için komutu. Bu örnekte, çekme `hello-world` görüntü.

```
docker pull hello-world
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir. Oturum açma sunucusunun adı şu biçimdedir  *\<kayıt defteri adı\>. azurecr.io* (tamamı küçük harflerle), örneğin, *mycontainerregistry007.azurecr.io*.

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüyü ACR örneğine gönderin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin. Bu örnekte **Merhaba-Dünya** deposu içeren `hello-world:v1` görüntü.

```
docker push <acrLoginServer>/hello-world:v1
```

Kapsayıcı kayıt defterinizde görüntünün gönderildikten sonra Kaldır `hello-world:v1` yerel Docker ortamınızdan görüntü. (Not Bu [docker RMI] [ docker-rmi] komut görüntüyü kaldırmaz **Merhaba-Dünya** Azure kapsayıcı kayıt defterinizde depoda.)

```
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

