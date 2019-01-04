---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 31a0acbfd0c9714bf2cbdf20f9f1f82edb07f05a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977403"
---
`ApplicationInsights` Ayarı eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği. Application Insights, kapsayıcının kapsamlı izleme sağlar. Kapsayıcınızı kullanılabilirliğini, performansını ve kullanımını kolayca izleyebilir. Ayrıca bir kolayca belirleyin ve kapsayıcınızda hatalarını tanılayın.

Aşağıdaki tabloda altında desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` bölümü.

|Gerekli| Ad | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | İzleme anahtarı Application Insights örneğinin hangi telemetri veri kapsayıcısı için gönderilir. Daha fazla bilgi için [ASP.NET Core için Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Örnek:<br>`InstrumentationKey=123456789`|

