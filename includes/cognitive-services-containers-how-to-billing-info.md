---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 002/08/2019
ms.openlocfilehash: 12c86ca71b7421678b68684cccca86411d604d61
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741539"
---
Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

`docker run` Faturalama amacıyla aşağıdaki değişkenleri kullanır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan Bilişsel Hizmet kaynağı API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan kaynak için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan Bilişsel hizmet kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir LUIS Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğinizi gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.
