---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2019
ms.openlocfilehash: b20ee2df1f468323c3298854371c97950c017e49
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57788944"
---
Kapsayıcıya sorguları için kullanılan bir Azure kaynak fiyatlandırma katmanını faturalandırılır `<ApiKey>`.

Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin. Kapsayıcı yaklaşık her 10 ila 15 dakika kullanım raporları.

`docker run` Faturalama amacıyla aşağıdaki değişkenleri kullanır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgileri izlemek için kullanılan Bilişsel Hizmet kaynağı API anahtarı.<br/>Bu seçeneğin değeri, belirtilen sağlanan kaynak için bir API anahtarı ayarlanmalıdır `Billing`. |
| `Billing` | Fatura bilgileri izlemek için kullanılan Bilişsel hizmet kaynak uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir LUIS Azure kaynağın URI'sini uç noktasına ayarlamanız gerekir.|
| `Eula` | Kapsayıcı lisansını kabul ettiğinizi gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.
