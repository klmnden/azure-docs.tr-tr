---
title: Azure ön kapısı hizmeti - HTTP üstbilgileri protokolü desteği | Microsoft Docs
description: Bu makale ön kapısı tarafından desteklenen HTTP üst bilgisi protokoller anlamanıza yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 0dcb769627714be9da55faf2a8e82c8750789498
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038859"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Azure ön kapısı hizmeti - HTTP üstbilgileri protokol desteği
Bu belge, aşağıdaki görüntüde belirtildiği gibi arama yolu çeşitli bölümleriyle Azure ön kapısı hizmetinin desteklediği protokolü özetlenmektedir. Aşağıdaki bölümlerde, ön kapısı desteklediği HTTP üstbilgileri hakkında daha fazla öngörüye verilmektedir.

![Azure ön kapısı hizmet HTTP üstbilgileri Protokolü][1]

>[!WARNING]
>Azure ön kapısı hizmet burada belgelenmemiş bir HTTP üstbilgileri garanti sağlamaz.

## <a name="1-client-to-front-door"></a>1. İstemci ön kapısı
(Bunları değiştirmeden) ön kapısı çoğu üst bilgiye gelen istek kabul eder, ancak bunlar gönderiliyorsa, gelen istekte kaldırılacak bazı ayrılmış üstbilgiler vardır. Bu, aşağıdaki ön ekleri ile üst bilgileri içerir:
 - X-FD *
 - X-MS *

## <a name="2-front-door-to-backend"></a>2. Arka uca ön kapısı

Yukarıda bahsedilen kısıtlamaları nedeniyle kaldırıldı sürece ön kapısı gelen istek üstbilgileri dahil edilir. Ön kapısı ayrıca aşağıdaki üst bilgileri ekler:

| Üst bilgi  | Örnek ve açıklaması |
| ------------- | ------------- |
| X-MS-başvuru |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ ==* </br> Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz başvuru dize budur. Erişim günlükleri aramak için kullanılan sorun giderme için önemlidir.|
| X-MS-RequestChain |  *X-MS-RequestChain: atlama sayısı = 1* </br> Bu istek döngü algılamak için ön kapı kullanan bir üst bilgi ve kullanıcılar üzerinde bir bağımlılık almamalıdır. |
| X-MS-aracılığıyla |  *X-MS-Via: Azure* </br> Bu, Azure/ön kapısı istemci ve arka uç arasındaki istek için bir ara alıcı olduğunu belirtmek için ön kapı tarafından eklenir. |

## <a name="3-front-door-to-client"></a>3. İstemci ön kapısı

Ön kapısı istemcilere gönderilen üst bilgiler aşağıda verilmiştir. Arka ucunuzdan ön kapısı gönderilen tüm üstbilgileri istemciye üzerinden geçirilir.

| Üst bilgi  | Örnek |
| ------------- | ------------- |
| X-MS-başvuru |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ ==* </br> Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz başvuru dize budur. Erişim günlükleri aramak için kullanılan sorun giderme için önemlidir.|

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [ön kapı oluşturmak](quickstart-create-front-door.md).
- Bilgi [ön kapısı işleyişi](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png