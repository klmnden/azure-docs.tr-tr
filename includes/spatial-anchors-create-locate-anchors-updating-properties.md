---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66110763"
---
## <a name="update-properties"></a>Güncelleştirme Özellikleri

Bir bağlantı özelliklerini güncelleştirmek için kullandığınız `UpdateAnchorProperties()` yöntemi. Aynı anda aynı bağlantı özelliklerini güncelleştirmek iki veya daha fazla cihaz denerseniz bir iyimser eşzamanlılık modeli kullanırız. İlk Yazımdan kazanacak anlamına gelir.  Diğer tüm yazma işlemlerini bir "Eşzamanlılık" hata iletisiyle karşılaşırsınız: tekrar denemeden önce özelliklerini yenilenmesini gerekli olur.
