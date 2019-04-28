---
title: Akıllı algılama - olağan dışı özel durum hacmindeki artışı, Azure Application ınsights | Microsoft Docs
description: Azure Application Insights ile uygulama özel durumları için özel durum hacmindeki olağan dışı modelleri izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: a6e7e8e01ccb623a3ff340c318c9c238c919cb38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61298593"
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Olağan dışı özel durum hacmindeki artışı (Önizleme)

Application ınsights'ı otomatik olarak uygulamanızda oluşturulan özel durumları inceler ve olağan dışı, özel durum telemetrisi yaklaşımlar hakkında uyarır.

Bu özellik dışında hiçbir özel kurulum gerektiren [özel durum bildirimini yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) uygulamanız için. Uygulamanızın yeterli özel durum telemetrisi oluşturduğunda etkin değil.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirim ne zaman elde edersiniz?
Uygulamanızın olağandışı artışı belirli bir türdeki özel durumların sayısı için bir gün içinde son yedi gün hesaplanan temel karşılaştırıldığında varsa bu tür bir bildirim alabilirsiniz.
Makine öğrenimi algoritmaları artış özel durum sayısı, uygulama kullanımınızı doğal bir büyüme göz önünde bulundurularak algılamak için kullanılıyor.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamı kesinlikle bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. Aşırı sayıda özel genellikle uygulama sorununu gösterir ancak zararsız ve doğru şekilde uygulamanız tarafından işlenen bu özel durumlar olabilir.

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimler tanılama işleminde desteklemek için tanılama bilgileri içerir:
1. **Değerlendirme.** Bildirim kaç kullanıcının veya kaç istek etkilendiğini gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun, tüm trafiği veya yalnızca bazı işlem etkileniyor? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama içinden, özel durum türü yanı sıra özel durum oluştu yöntemi hakkında bilgi içerir. İlgili öğeler de kullanabilirsiniz ve daha fazla yardımcı olacak bilgiler için destek bağlama raporları sorunu tanılayın.