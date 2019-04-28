---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60232528"
---
## <a name="update-properties"></a>Güncelleştirme Özellikleri

Bir bağlantı özelliklerini güncelleştirmek için kullandığınız `UpdateAnchorProperties()` yöntemi. Aynı anda aynı bağlantı özelliklerini güncelleştirmek iki veya daha fazla cihaz denerseniz bir iyimser eşzamanlılık modeli kullanırız. İlk Yazımdan kazanacak anlamına gelir.  Diğer tüm yazma işlemlerini bir "Eşzamanlılık" hata iletisiyle karşılaşırsınız: tekrar denemeden önce özelliklerini yenilenmesini gerekli olur.
