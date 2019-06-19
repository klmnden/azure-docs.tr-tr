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
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188929"
---
Bu öğreticiyi tamamlamak için aşağıdaki gereksinimleri karşılamanız gerekir:

**Azure CLI**: Daha sonra yerel bilgisayarınızda yüklü veya Azure CLI 2.0.29 sürüm olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI'yı yükleme][azure-cli-install].

**Docker**: Bu öğreticide çekirdek temel bir anlayış gibi kapsayıcılar, kapsayıcı görüntüleri ve temel Docker kavramları varsayılır `docker` komutları. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış][docker-get-started] bölümüne bakın.

**Docker altyapısı**: Bu öğreticiyi tamamlamak için Docker altyapısının'ın yerel olarak yüklü olması gerekir. Docker, [macOS][docker-mac], [Windows][docker-windows] ve [Linux][docker-linux] üzerinde Docker ortamını yapılandıran paketler sağlar.

> [!IMPORTANT]
> Azure Cloud shell, Docker programını içermediğinden bu öğreticiyi tamamlamak için *yerel bilgisayarınıza* hem Azure CLI’yi hem de Docker Altyapısı’nı yüklemeniz *gerekir*. Bu öğretici için Azure Cloud Shell kullanamazsınız.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
