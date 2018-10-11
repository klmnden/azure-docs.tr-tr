---
title: include dosyası
description: include dosyası
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: da63a5418ab94623f6ce3c9f35a085dd8b198d1a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48858103"
---
Bu öğreticiyi tamamlamak için aşağıdaki gereksinimleri karşılamanız gerekir:

**Azure CLI**: Yerel bilgisayarınızda Azure CLI 2.0.29 veya sonraki bir sürümü yüklenmiş olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI'yı yükleme][azure-cli-install].

**Docker**: Bu öğreticide kapsayıcılar, kapsayıcı görüntüleri ve temel `docker` komutları gibi temel Docker kavramları hakkında bilgi sahibi olduğunuz varsayılmıştır. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış][docker-get-started] bölümüne bakın.

**Docker Altyapısı**: Bu öğreticiyi tamamlamak için Docker Altyapısı’nın yerel olarak yüklenmiş olması gerekir. Docker, [macOS][docker-mac], [Windows][docker-windows] ve [Linux][docker-linux] üzerinde Docker ortamını yapılandıran paketler sağlar.

> [!IMPORTANT]
> Azure Cloud shell, Docker programını içermediğinden bu öğreticiyi tamamlamak için *yerel bilgisayarınıza* hem Azure CLI’yi hem de Docker Altyapısı’nı yüklemeniz *gerekir*. Bu öğretici için Azure Cloud Shell kullanamazsınız.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
