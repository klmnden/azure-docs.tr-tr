---
title: Erişim ve Azure Time Series Insights'ı yönetmek için güvenlik yapılandırma | Microsoft Docs
description: Bu makalede güvenlik ve izinler yönetim erişimi yapılandırma ilkeleri ve veri erişim ilkeleri Azure Time Series Insights güvenliğini sağlamak için.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7cb5dc5b170103f98d56abc920f36dd85f855961
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364831"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamına veri erişimi verme

Zaman Serisi Görüşleri ortamlarının birbirinden bağımsız türde iki erişim ilkesi vardır:

* Yönetim erişimi ilkeleri
* Veri erişimi ilkeleri

Her iki ilke de Azure Active Directory asıl adlarına (kullanıcılar ve uygulamalar) belirli bir ortam üzerinde çeşitli izinler verir. Ortamı içeren abonelikle ilişkili (Azure kiracısı bilinir) active Directory asıl adlarına (kullanıcılar ve uygulamalar) ait olmalıdır.

Yönetim erişimi ilkeleri, ortamı, olay kaynaklarını, başvuru veri kümelerini oluşturma ve
*   Silme, veri erişimi ilkelerini yönetme gibi ortamın yapılandırmasıyla ilgili izinler
*   verir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

İki ilke türü de ortamın yönetimine erişim ile ortam içindeki verilere erişim arasında net bir ayrım yapmaya olanak tanır. Örneğin, ortamın sahibinin/oluşturucusunun veri erişiminden kaldırılır, bir ortamı ayarlamak mümkündür. Ayrıca, kullanıcılar ve ortamdan veri okumak için izin verilen hizmetler ortamının yapılandırması erişim verilebilir.

[!INCLUDE [iot-tsi-data-access](../../includes/iot-tsi-data-access.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Azure zaman serisi görüşleri ortamınıza bir Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
* Ortamınızı görüntüleyebilirsiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
