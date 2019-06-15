---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110502"
---
## <a name="pause-reset-or-stop-the-session"></a>Duraklatmak, sıfırlama veya oturumu durdurun

Oturumunu durdurmak için geçici olarak çağırabilirsiniz `Stop()`. ProcessFrame() çağırma bile bunu tüm İzleyicileri ve ortam işlemeyi durdurun. Ardından çağırabilirsiniz `Start()` işleme devam etmek için. Edilirken, zaten oturum sırasında yakalanan ortam veriler korunur.
