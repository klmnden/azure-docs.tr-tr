---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: 1d8340054ace25a0cdc36eef9c3d5b4238a6f99b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188655"
---
Saniye başına yapabileceğiniz sorgu sayısı (QPS), hizmete ve aboneliğinizin türüne göre belirlenir. Uygulamanızın, kotanızın sınırları içinde kalmak için gereken mantığı içerdiğinden emin olun. QPS sınırına ulaşılırsa veya bu sınır aşılırsa istek başarısız olur ve HTTP 429 durum kodu döndürülür. Yanıt, başka bir istek göndermeden önce beklemeniz gereken süreyi gösteren `Retry-After` üst bilgisini içerir.

## <a name="denial-of-service-versus-throttling"></a>Hizmet reddi ile azaltma karşılaştırması

Hizmette, hizmet reddi (DoS) saldırısıyla QPS ihlali arasında bir ayrım yapılır. Hizmet bir DoS saldırısı olduğundan şüphelenirse istek başarılıyla sonuçlanır (HTTP durum kodu 200 TAMAM). Ancak, yanıt gövdesi boş olur.