---
title: Akıllı algılama - Azure Application Insights tarafından algılanan olası bellek sızıntısı | Microsoft Docs
description: Azure Application Insights ile bir uygulama için olası bellek sızıntılarını izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: e430b1e976ac26f7320b28d50dd39923066cfa41
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306352"
---
# <a name="memory-leak-detection-preview"></a>Bellek sızıntısı algılamayı (Önizleme)

Application ınsights'ı otomatik olarak uygulamanızdaki her bir işlem bellek tüketimini analiz eder ve olası bellek sızıntısı ya da daha yüksek bellek tüketimi hakkında sizi uyarır.

Bu özellik dışında hiçbir özel kurulum gerektiren [performans sayaçları yapılandırılıyor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) uygulamanız için. Uygulamanızı yeterli bellek performans sayaçları telemetri (örneğin, özel baytlar) oluşturduğunda etkin değil.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirim ne zaman elde edersiniz?
Tipik bir bildirim, bir uzun süre boyunca, uygulamanızın bir parçası olan bir veya daha fazla makine ve/veya bir veya daha fazla işlem bellek tüketimi tutarlı bir artış izler. Makine öğrenimi algoritmaları, bir bellek sızıntısı deseni ile eşleşen artan bellek tüketimi algılamak için kullanılır.

## <a name="does-my-app-really-have-a-problem"></a>Uygulamamı gerçekten bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Bellek sızıntısı desenler genellikle bir uygulama sorunu olduğunu olsa da, bu desenleri belirli süreciniz için tipik olabilir veya doğal İş Gerekçesi olabilir ve yok sayılabilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimler tanılama Analiz işlemini desteklemek için tanılama bilgileri içerir:
1. **Değerlendirme.** Bildirim, bellek miktarını artırın (GB) ve bellek içinde arttığını gözlemleyebiliriz zaman aralığını gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Makine sayısını, bellek sızıntısı deseni sergilenen? Kaç özel durumlar, olası bellek sızıntısı sırasında tetiklenen? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama zaman içinde bellek tüketimini işleminin gösteren bellek sızıntısı deseni içerir. İlgili öğeler de kullanabilirsiniz ve daha fazla yardımcı olacak bilgiler için destek bağlama raporları sorunu tanılayın.
