---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 2f5b03dd0170da9a9869183d7a412688509525ef
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56740778"
---
`ApplicationInsights` Ayarı eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği. Application Insights, kapsayıcının kapsamlı izleme sağlar. Kapsayıcınızı kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca bir kolayca belirleyin ve kapsayıcınızda hatalarını tanılayın.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` bölümü.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Örnek:<br>`InstrumentationKey=123456789`|

