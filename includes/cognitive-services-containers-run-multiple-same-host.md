---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/21/2019
ms.openlocfilehash: 820ea4c401d560d4cf1c937d2efd7d7bde579a91
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56675747"
---
### <a name="running-multiple-containers-on-the-same-host"></a>Aynı ana bilgisayarda çalışan birden fazla kapsayıcılar

Kullanıma sunulan bağlantı noktası olan birden çok kapsayıcı çalıştırmak istiyorsanız, her kapsayıcı farklı bir bağlantı noktası ile çalıştırmak emin olun. Örneğin, ilk kapsayıcı bağlantı noktası 5000 ve ikinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.

Değiştirin `<container-registry>` ve `<container-name>` kullandığınız kapsayıcıları değerleri. Bunlar aynı kapsayıcı olması gerekmez. Yüz tanıma kapsayıcısı ve LUIS kapsayıcı birlikte çalışıyor olabilir veya çalışan birden çok yüzü kapsayıcılar sahip olabilir. 

İlk kapsayıcı 5000 numaralı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

İkinci kapsayıcıyı 5001 bağlantı noktası üzerinde çalıştırın.


```bash 
docker run --rm -it -p 5001:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktası olmalıdır. 