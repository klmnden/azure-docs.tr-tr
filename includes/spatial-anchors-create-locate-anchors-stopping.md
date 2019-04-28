---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232603"
---
## <a name="pause-reset-or-stop-the-session"></a>Duraklatmak, sıfırlama veya oturumu durdurun

Oturumunu durdurmak için geçici olarak çağırabilirsiniz `Stop()`. ProcessFrame() çağırma bile bunu tüm İzleyicileri ve ortam işlemeyi durdurun. Ardından çağırabilirsiniz `Start()` işleme devam etmek için. Edilirken, zaten oturum sırasında yakalanan ortam veriler korunur.
