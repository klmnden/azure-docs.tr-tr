---
title: Azure Container Registry içerik biçimleri
description: Azure Container Registry'de desteklenen içerik biçimleri hakkında bilgi edinin.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2018
ms.author: danlep
ms.openlocfilehash: e7155604339bc634078fd022e05ede5f902bc0d8
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634730"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure Container Registry'de desteklenen içerik biçimleri

Özel depo, Azure Container Registry'de içerik aşağıdaki biçimlerden birini yönetmek için kullanın. 

## <a name="docker-compatible-container-images"></a>Docker ile uyumlu kapsayıcı görüntüleri

* [Docker görüntüsü bildirim V2, Şeması 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker görüntüsü bildirim V2, şema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) -bildirim tek "image: tag" başvuru altında birden çok platform görüntüleri depolamak kayıt defterleri sağlayan listeler içerir

* [Açık kapsayıcı girişim (OCI) görüntü biçim belirtimi](https://github.com/opencontainers/image-spec/blob/master/spec.md) 


## <a name="helm-charts"></a>Helm grafikleri

Azure Container Registry de depolar için ana bilgisayar [Helm grafikleri](https://helm.sh/), hızlı bir şekilde yönetmek ve Kubernetes için uygulamaları dağıtmak için kullanılan bir paketleme biçimi. [Helm istemci](https://docs.helm.sh/using_helm/#installing-helm) 2.11.0 sürümü veya üzeri desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* Bkz. nasıl [gönderme ve çekme](container-registry-get-started-docker-cli.md) görüntülerini Azure Container Registry ile.

* Kullanım [ACR görevleri](container-registry-tasks-overview.md) oluşturmak ve kapsayıcı görüntüleri test etmek için. 

* Kullanım [Moby BuildKit](https://github.com/moby/buildkit) derlemek ve paketlemek OCI biçimde kapsayıcılar için.

* Ayarlanmış bir [Helm deposu](container-registry-helm-repos.md) Azure Container Registry'de barındırılan. 


