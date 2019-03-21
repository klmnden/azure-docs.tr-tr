---
ms.openlocfilehash: 8e02067b32d9404a5bbdf7362b2cc32712b96250
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57907776"
---
## <a name="pause-reset-or-stop-the-session"></a>Duraklatmak, sıfırlama veya oturumu durdurun

Geçici olarak oturumunu durdurmak için Stop() çağırabilirsiniz. ProcessFrame() çağırma bile bunu tüm İzleyicileri ve ortam işlemeyi durdurun. Ardından, işleme devam etmek için Start() çağırabilirsiniz. Edilirken, zaten oturum sırasında yakalanan ortam veriler korunur.
