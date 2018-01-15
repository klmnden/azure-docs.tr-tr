---
title: "Akıllı algılama - Azure Application Insights tarafından algılanan bulut kaynakların az kullanılması | Microsoft Docs"
description: "Azure Application Insights ile bulut kaynakların az kullanılması için uygulamalarını izleyin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 8382f6047ae222a01cc0e8d6ca9dcf5593d0dff6
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Düşük kullanımı bulut kaynaklarının (Önizleme)

Application Insights otomatik olarak uygulamanızdaki her rol örneğinin CPU tüketimi analiz eder ve düşük CPU kullanımı örnekleriyle algılar. Bu algılama, Azure kaynaklarınızı azaltmak ve her rol yararlanan rol örnekleri sayısını azaltarak ya da rollerinin sayısını azaltarak maliyetleri azaltmak sağlar.

Bu özellik, başka hiçbir özel kurulum gerektirir [performans sayaçları yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) uygulamanız için. Uygulamanızı yeterli CPU performans sayacı telemetri (% işlemci zamanı) oluşturduğunda etkin olur.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirim ulaştıklarında?
Tipik bir bildirim, birçok Web/çalışan rolü örneklerinizi düşük CPU kullanımı sergilemesine oluşur.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Uygulamam kesinlikle çok fazla kaynak tüketmesine mu?
Hayır, bir bildirim uygulamanızı kesinlikle çok fazla kaynak tüketen anlamına gelmez. Kaynak tüketimini azaltılabilir desenler düşük CPU kullanımı genellikle işaret eder ancak bu davranış, belirli rol için tipik olabilir veya doğal iş gerekçesinin olabilir ve göz ardı edilebilir. Örneğin, birden çok örneği bellek/ağ ve CPU değil gibi başka kaynaklar için gerekli olan olabilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimleri tanılama işlemi desteklemek için tanılama bilgileri içerir:
1. **Önceliklendirme.** Bildirim uygulamanızda düşük CPU kullanımı sergilemesine rolleri gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Kaç tane roller, düşük CPU kullanımı ve her rol kaç durumlarda düşük CPU kullanma sergilenen? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama zaman içinde her örneğinin CPU kullanımını gösteren kullanılan, CPU yüzdesini içerir. Sorunu daha ayrıntılı olarak tanılamaya yardımcı olması için ilgili öğeler ve CPU kullanımı, yüzdebirlik değeri gibi bilgileri destek bağlama raporları kullanabilirsiniz.
