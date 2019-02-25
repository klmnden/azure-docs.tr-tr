---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 0e4c0b6d622fe0d8d20692ec6c0c889779e4601a
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753233"
---
### <a name="access-tokens"></a>Erişim Belirteçleri

Erişim belirteçleri, Azure uzamsal Çıpasıyla kimliğini doğrulamak için daha güçlü bir yöntemdir. Özellikle,, uygulamanızı bir üretim dağıtımı için hazırlayın. Bu yaklaşım özetini istemci uygulamanız ile güvenli bir şekilde doğrulanabilir bir arka uç hizmeti ayarlamaktır. Arka uç Hizmeti Arabirimleri olduğu bir erişim belirteci istemek için Azure uzamsal bağlayıcılarını STS hizmetiyle çalışma zamanı sırasında AAD ile. Bu belirteci sonra istemci uygulamaya teslim ve SDK'yı Azure uzamsal Çıpasıyla kimliğini doğrulamak için kullanılır.
