---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 5ff41442d32739763332fa9bab59543ea3c7804a
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129360"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Docker ve Kubernetes geliştirme için kod hazırlama
Şimdiye kadar, yerel olarak çalıştırılabilen temel bir web uygulamanız vardı. Şimdi uygulamanın kapsayıcısını tanımlayan varlıklar oluşturup Kubernetes’de nasıl dağıtılacağını belirleyerek uygulamayı kapsayıcılı hale getireceksiniz. Bu görev Azure Dev Spaces ile kolayca gerçekleştirilebilir: 

1. VS Code’u başlatın ve `webfrontend` klasörünü açın. (Hata ayıklama varlıkları eklemek veya projeyi geri yüklemek için tüm varsayılan istemleri yoksayabilirsiniz.)
1. VS Code’da Tümleşik Terminal’i açın (**Görünüm > Tümleşik Terminal** menüsünü kullanarak).
1. Şu komutu çalıştırın (geçerli klasörünüzün **webfrontend** olduğundan emin olun):

    ```cmd
    azds prep --public
    ```

Azure CLI’nin `azds prep` komutu varsayılan ayarlarla Docker ve Kubernetes varlıklarını oluşturur:
* `./Dockerfile`, uygulamanın kapsayıcı görüntüsünü açıklar, kaynak kodunun nasıl derlendiğini ve kapsayıcının içinde çalıştırıldığını belirtir.
* `./charts/webfrontend` altındaki [Helm grafiği](https://docs.helm.sh), kapsayıcının Kubernetes'de nasıl dağıtıldığını açıklar.

Şimdilik bu dosyaların tüm içeriğini anlamanız gerekli değildir. Bununla birlikte, **geliştirme aşamasından üretim aşamasına kadar aynı Kubernetes ve Docker kod yapılandırmalı varlıklarının kullanılabildiğini, bu şekilde farklı ortamlarda daha tutarlı sonuçlar sağlanabildiğini** belirtmek gerekir.
 
`prep` komutuyla `./azds.yaml` adlı bir dosya da oluşturulur ve bu dosya Azure Dev Spaces’in yapılandırma dosyasıdır. Azure’da yinelemeli bir geliştirme deneyimi sağlamak için Docker ve Kubernetes yapıtlarını ek yapılandırmayla tamamlar.
