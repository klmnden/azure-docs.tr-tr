---
title: Akıllı algılama - Azure Application Insights tarafından algılanan bulut kaynakların az kullanılması | Microsoft Docs
description: Azure Application Insights ile uygulamaları için bulut kaynaklarını düşük kullanımını izleyin.
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
ms.openlocfilehash: 7cf72068b9cabceb0c5b535986ac4dfb62151b94
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61297524"
---
# <a name="low-cpu-utilization-in-cloud-resources-preview"></a>Düşük CPU kullanımı bulut kaynaklarında (Önizleme)

Application Insight otomatik olarak uygulamanızdaki her rol örneğinin CPU tüketimini analiz eder ve düşük CPU kullanımı örnekleriyle algılar. Bu algılama, Azure kaynaklarınızı azaltmak ve her rol yararlanan rol örneklerinin sayısını azaltmayı ya da rolleri sayısını azaltarak maliyetleri azaltmak sağlar.

Bu özellik dışında hiçbir özel kurulum gerektiren [performans sayaçları yapılandırılıyor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) uygulamanız için. Uygulamanızı yeterli CPU performans sayacı telemetri (% işlemci süresi) oluşturduğunda etkin değil.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirim ne zaman elde edersiniz?
Tipik bir bildirim, birçok Web/çalışan rolü örneklerinizi düşük CPU kullanımından ek oluşur.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Uygulamamı kesinlikle çok fazla kaynak tüketmesine mu?
Hayır, bir bildirim uygulamanızı kesinlikle çok fazla kaynak tükettiğini anlamına gelmez. Kaynak tüketimi düşürülmesini desenler düşük CPU kullanımı genellikle gösterir ancak bu davranışı belirli rolünüz için tipik olabilir veya doğal İş Gerekçesi olabilir ve göz ardı edilebilir. Örneğin, birden fazla bellek/ağ ve CPU değil gibi diğer kaynaklar için gerekli olup olmadığını olabilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimler tanılama işleminde desteklemek için tanılama bilgileri içerir:
1. **Değerlendirme.** Bildirim uygulamanızda düşük CPU kullanımından ek rolleri gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Düşük CPU kullanımı ve düşük CPU her rolden kaç tane yazılımınız kaç rol sergilenen? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama zaman içinde her örneğinin CPU kullanımını gösteren kullanılan, CPU yüzdesi içerir. Sorunu daha ayrıntılı olarak tanılamaya yardımcı olmak için ilgili öğeleri ve CPU kullanımı. yüzdebirlik değerleri gibi bilgileri destek bağlama raporları kullanabilirsiniz.
