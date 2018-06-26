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
ms.openlocfilehash: d44f33b0e9f71c1d8d6e2c9878b08f9fa0e1f8a1
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938181"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Kod Docker ve Kubernetes geliştirme için hazırlama
Şu ana kadar yerel olarak çalıştırabileceğiniz bir temel web uygulamasına sahip. Artık bu uygulamanın kapsayıcı ve Kubernetes için nasıl dağıtacağınız tanımlayın varlıklar oluşturarak containerize. Bu görev, Azure Dev alanlarıyla yapmak kolaydır: 

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
 
Adlı bir dosya `./azds.yaml` de tarafından oluşturulan `prep` komutu, bu ise Azure Dev alanları için yapılandırma dosyası. Azure'da bir yinelemeli bir geliştirme deneyimine olanak ek yapılandırma Docker ve Kubernetes yapıtlarıyla tamamlar.
