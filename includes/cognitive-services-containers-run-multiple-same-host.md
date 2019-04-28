---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/21/2019
ms.openlocfilehash: e80feac7dbf16652cc2e2a6176ed8b2c8c48e35b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598787"
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
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktası olmalıdır. 
