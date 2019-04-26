---
title: Akıllı algılama - Azure Application Insights ile güvenlik algılama paketi | Microsoft Docs
description: Azure Application Insights ile bir uygulama için olası güvenlik sorunlarını izler.
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
ms.openlocfilehash: 16dd381301bdc650022ba5580f96a1733aeb32b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60199961"
---
# <a name="application-security-detection-pack-preview"></a>Uygulama güvenlik algılama paketi (Önizleme)

Application Insights, otomatik olarak uygulamanız tarafından üretilen telemetri analiz eder ve olası güvenlik sorunlarını algılar. Bu özelliği, olası güvenlik sorunlarını tanımlamak ve düzeltme uygulama tarafından veya gerekli güvenlik önlemleri alarak işlemek olanak tanır.

Bu özellik dışında hiçbir özel kurulum gerektiren [telemetri göndermek için uygulamanızı yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirim ne zaman elde edersiniz?
Üç tür algılanan güvenlik sorunları vardır:
1. Güvenli olmayan bir URL erişimi: uygulamanın bir URL HTTP ve HTTPS erişiliyor. Genellikle, HTTPS isteklerini kabul eden bir URL HTTP isteklerini kabul. Bu, uygulamanızda bir hata veya güvenlik sorununu gösterebilir.
2. Güvenli olmayan formu: uygulamada bir form (veya başka bir "POST" istek) HTTP yerine HTTPS kullanır. HTTP kullanarak form tarafından gönderilen kullanıcı verilerini tehlikeye atabilir.
3. Şüpheli kullanıcı etkinliğinden: yaklaşık aynı zamanda uygulama birden çok ülkelerden aynı kullanıcı tarafından erişiliyor. Örneğin, aynı kullanıcı uygulamayı İspanya ve Amerika Birleşik Devletleri aynı saat içinde erişilebilir. Bu algılama yöntemi, uygulamanızın bir kötü amaçlı olabilecek erişim girişimi gösterir.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Uygulamamı kesinlikle bir güvenlik sorunu var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir güvenlik sorunu olduğunu anlamına gelmez. Yukarıdaki senaryoların herhangi bir algılama, çoğu durumda, bir güvenlik sorunu olduğunu gösterebilir. Ancak, algılama doğal İş Gerekçesi olabilir ve yok sayılabilir.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>"Güvenli olmayan bir URL erişimi" algılama nasıl düzeltebilirim?
1. **Değerlendirme.** Bildirim güvenli olmayan URL'ler ve çoğu URL erişen kullanıcıların sayısını sağlar güvenli erişim tarafından etkilenen. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Hangi kullanıcıların yüzdesi, güvenli olmayan URL'ler erişilebilir? Kaç tane URL'leri etkilendi? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama, güvenli olmayan isteklerin listesini ve URL'leri ve sorunu daha ayrıntılı olarak tanılamaya yardımcı olmak için etkilenen kullanıcıların listesini sağlar.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>"Güvenli form" algılama nasıl düzeltebilirim?
1. **Değerlendirme.** Bildirim, güvenli olmayan formlar sayısını ve verisini potansiyel olarak aşılmış bir kullanıcı sayısını sağlar. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Hangi formun en büyük sayısını güvenli olmayan iletimleri söz konusuydu ve güvensiz iletimleri zaman içindeki dağılımını nedir? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama, güvenli olmayan formlar listesi ve sorunu daha ayrıntılı olarak tanılamaya yardımcı olmak için her form için güvenli aktarımlara sayısı bir dökümünü sağlar.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>"Şüpheli kullanıcı etkinliğinden" algılama nasıl düzeltebilirim?
1. **Değerlendirme.** Bildirim şüpheli davranış gösterdi farklı kullanıcı sayısını sağlar. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Hangi ülkelerde şüpheli istekleri fırsatlara? Hangi kullanıcı en şüpheli neydi? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** Algılama, Şüpheli kullanıcıların listesini ve sorunu daha ayrıntılı olarak tanılamaya yardımcı olmak için her kullanıcı için ülkelerin listesi sağlar.
