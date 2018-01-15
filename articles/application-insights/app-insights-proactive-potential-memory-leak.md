---
title: "Algılama - Azure Application Insights tarafından algılanan olası bellek sızıntısı akıllı | Microsoft Docs"
description: "Azure Application Insights ile olası bellek sızıntıları için uygulamalarını izleyin."
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
ms.openlocfilehash: e98caaa387418d746905990436b69925a591b260
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="memory-leak-detection-preview"></a>Bellek sızıntısı algılama (Önizleme)

Application Insights otomatik olarak uygulamanızdaki her işlem bellek kullanımını analiz eder ve olası bellek sızıntıları veya daha yüksek bellek tüketimine hakkında uyarır.

Bu özellik, başka hiçbir özel kurulum gerektirir [performans sayaçları yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) uygulamanız için. Uygulamanızı yeterli bellek performans sayaçları telemetri (örneğin, özel bayt sayısı) oluşturduğunda etkin olur.


## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirim ulaştıklarında?
Tipik bir bildirim, bir veya daha fazla işlem ve/veya, uygulamanın parçası olan bir veya daha fazla makine bellek tüketimi bir uzun süre boyunca (birkaç saat), tutarlı bir artış izler.
Machine learning algoritmaları doğal olarak uygulama kullanımı artırmayı nedeniyle daha yüksek bellek tüketimine aksine bir bellek sızıntısı olan bir desenle eşleşen daha yüksek bellek tüketimine algılama için kullanılır.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamam kesinlikle bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Bellek sızıntısı desenleri genellikle uygulama sorununu gösterir ancak bu desenleri belirli işleminizi tipik olabilir veya doğal iş gerekçesinin olabilir ve göz ardı edilebilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimleri tanılama Çözümleme işleminin desteklemek için tanılama bilgileri içerir:
1. **Önceliklendirme.** Bildirim, bellek miktarını artırın (GB) ve bellek artan zaman aralığı gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Kaç tane makineler bellek sızıntısı düzeni sergilenen? Kaç tane özel durumlar olası bellek sızıntısı sırasında tetiklenen? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama zamanla işleminin bellek tüketimini gösteren bellek sızıntısı düzeni içerir. Daha fazla yardımcı olacak bilgiler, destek bağlama raporları sorunu tanılamak ve ilgili öğeler de kullanabilirsiniz.
