---
title: "Akıllı algılama - özel birim, Azure Application Insights anormal artışa | Microsoft Docs"
description: "Özel durum birimindeki olağan dışı desenleri Azure Application Insights ile uygulama özel durumlarını izleyin."
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 8030f3331a03170bb265c417a57725544bdc7d3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Olağan dışı artışa özel birim (Önizleme)

Application Insights otomatik olarak uygulamanızda oluşturulan özel durumları analiz eder ve özel durum telemetrisi olağan dışı düzenleri hakkında uyarır.

Bu özellik, başka hiçbir özel kurulum gerektirir [özel durum bildirimini yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) uygulamanız için. Uygulamanızı yeterli özel durum telemetrisi oluşturduğunda etkin olur.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirim ulaştıklarında?
Uygulamanızı belirli bir türün özel durumların sayısı olağan dışı bir artışa bir günde önceki yedi gün içinde hesaplanan bir taban çizgisi ile karşılaştırıldığında varsa bu tür bir uyarı alabilirsiniz.
Machine learning algoritmaları, uygulama kullanımınızı doğal bir artışa göz önünde bulundurularak özel durum sayısı artışa algılamak için kullanılıyor.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamam kesinlikle bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Özel durumlar aşırı sayıda genellikle uygulama sorununu gösterse de, bu özel durumlar zararsız ve doğru şekilde uygulamanız tarafından işlenmiş olabilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimleri tanılama işlemi desteklemek için tanılama bilgileri içerir:
1. **Önceliklendirme.** Bildirim kaç kullanıcı veya kaç istek etkilenen gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Sorun, tüm trafik ya da yalnızca başka bir işlem söz konusu? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama içinden, özel durum türü yanı sıra özel durum oluştu yöntemi hakkında bilgi içerir. Daha fazla yardımcı olacak bilgiler, destek bağlama raporları sorunu tanılamak ve ilgili öğeler de kullanabilirsiniz.