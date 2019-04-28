---
ms.openlocfilehash: 3fa71085d649ace95aa24ac87c8714a7268f5386
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598465"
---
Daha doğru tüketim maliyetlerinin tahmin etmek için iletileri veya belirli bir günde gelmesi yerine yalnızca yoklama aralığına Hesaplamalarınızda temel olayları olası sayısını göz önünde bulundurun. Tetikleme ölçütü karşıladığında bir olay ya da ileti ve tüm diğer bekleyen olayları ya da ölçütleri karşılayan iletileri okumak birçok tetikleyici hemen deneyin. Bekleyen olaylar veya iş akışlarını başlatmak için uygun bulunmuş iletileri sayısına dayalı tetikleyici bir uzun yoklama aralığı seçtiğinizde bu davranışı anlamına gelir. Bu davranışı uygular tetikleyicileri, Azure Service Bus ve Azure olay hub'ı içerir.

Bu nedenle, her gün bir uç nokta denetleyen tetikleyici ayarlama gibi varsayalım. Tetikleyici bitiş noktasını denetler ve ölçütlere uyan 15 olayları bulur, tetiklenir ve 15 kez karşılık gelen iş akışı çalıştırır. Logic Apps bu 15 iş akışlarının gerçekleştirmek, tetikleyici istekleri dahil olmak üzere tüm eylemleri ölçümleri. Olası maliyetlerinizi hesaplamak için deneyin [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/).