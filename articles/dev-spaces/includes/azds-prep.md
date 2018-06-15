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
ms.openlocfilehash: e9f97f804985f948e5442c64a31d95e7931b03cd
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198835"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Kod Docker ve Kubernetes geliştirme için hazırlama
Şu ana kadar yerel olarak çalıştırabileceğiniz bir temel web uygulamasına sahip. Artık bu uygulamanın kapsayıcı ve Kubernetes için nasıl dağıtacağınız tanımlayın varlıklar oluşturarak containerize. Bu Azure Dev alanlarıyla yapmak kolaydır: 

1. VS Code'u başlatın ve açın `webfrontend` klasör. (Hata ayıklama varlıklar eklemek veya projeyi geri yükleme için varsayılan sizden yoksayabilirsiniz.)
1. VS Code'da tümleşik Terminali açın (kullanarak **Görünüm > tümleşik Terminal** menüsü).
1. Bu komutu çalıştırın (olduğundan emin olun **webfrontend** geçerli klasörüdür):

    ```cmd
    azds prep --public
    ```

Azure CLI `azds prep` komut, Docker ve Kubernetes varlıklar varsayılan ayarlarla oluşturur:
* `./Dockerfile` uygulamanın kapsayıcı görüntü ve kaynak kodu yerleşik olarak bulunur ve kapsayıcı içinde çalışan açıklar.
* A [Helm grafik](https://docs.helm.sh) altında `./charts/webfrontend` Kubernetes için kapsayıcı dağıtmayı açıklar.

Şu an için bu dosyalar tam içeriğini anlamak gerekli değildir. Ancak olduğu çıkışı, işaret eden değer, **aynı Kubernetes ve Docker yapılandırma olarak kod varlıklarına kullanılabilecek geliştirmeden üretime böylece sağlayarak daha iyi tutarlılık farklı ortamlar genelinde.**
 
Adlı bir dosya `./azds.yaml` de tarafından oluşturulan `prep` komutu, bu ise Azure Dev alanları için yapılandırma dosyası. Azure'da bir yinelemeli bir geliştirme deneyimine olanak ek yapılandırma Docker ve Kubernetes yapıtlarıyla tamamlar. Örneğin, varsayılan Helm grafik ortak uç nokta kullanıma sunmuyor. Bazı durumlarda, ancak genel bir uç nokta geçici olarak açmak dolayısıyla söyleyin, kodunuzu test edebilirsiniz geliştirme sırasında bir mobil cihaz veya bir Web kancası URL'si yararlı olacaktır. Kullanılarak oluşturulan bir azds.yaml dosyası `azds prep --public` genel bir uç nokta yalnızca geliştirme zamanı kullanıma sunmak için Helm varsayılan parametreleri geçersiz kılar.
