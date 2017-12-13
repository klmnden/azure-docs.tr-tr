---
title: "Algılama - güvenlik algılama paketi Azure Application Insights ile akıllı | Microsoft Docs"
description: "Azure Application Insights ile bir uygulama için olası güvenlik sorunlarını izleyin."
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
ms.openlocfilehash: 3e4604a154c16b785db1ab903587ae4a35d93c05
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="application-security-detection-pack-preview"></a>Uygulama güvenlik algılama paketi (Önizleme)

Application Insights otomatik olarak, uygulamanız tarafından oluşturulan telemetri analiz eder ve olası güvenlik sorunlarını algılar. Bu özellik, olası güvenlik sorunlarını tanımlamak ve uygulama düzelttikten veya gerekli güvenlik önlemleri alarak işlemek sağlar.

Bu özellik, başka hiçbir özel kurulum gerektirir [telemetri göndermek için uygulamanızı yapılandırmaya](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirim ulaştıklarında?
Üç tür algılanan güvenlik sorunları vardır:
1. Güvenli olmayan URL erişimi: uygulamanın URL'de hem HTTP hem de HTTPS erişiliyor. Genellikle, HTTPS isteklerini kabul eden bir URL HTTP isteklerini kabul. Bu, uygulamanızdaki bir hata veya güvenlik sorunu gösteriyor olabilir.
2. Güvenli olmayan form: uygulama bir form (veya başka bir "POST" istek) HTTP yerine HTTPS kullanır. HTTP kullanarak form tarafından gönderilen kullanıcı verilerini tehlikeye atabilir.
3. Şüpheli kullanıcı etkinliği: yaklaşık aynı zamanda uygulama birden çok ülkelerden aynı kullanıcı tarafından erişiliyor. Örneğin, aynı kullanıcı uygulamayı İspanya Amerika Birleşik Devletleri aynı saat içinde erişildiğini. Bu algılama, uygulamanız için bir kötü amaçlı olabilecek erişim girişimi gösterir.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Uygulamam kesinlikle bir güvenlik sorunu var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir güvenlik sorunu olduğunu anlamına gelmez. Yukarıdaki senaryoların herhangi biri algılanması, çoğu durumda, bir güvenlik sorunu belirtebilirsiniz. Ancak, algılama doğal iş gerekçesinin olabilir ve göz ardı edilebilir.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>"Güvenli URL erişim" algılama nasıl düzeltirim?
1. **Önceliklendirme.** Güvenli olmayan URL'leri ve çoğu URL erişen kullanıcıların sayısını bildirim sağlar güvenli erişim tarafından etkilenen. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Hangi kullanıcıların yüzdesi güvenli olmayan URL'leri erişilen? Kaç tane URL'leri etkilendi? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama güvensiz isteklerin listesini ve URL'ler ve sorunu daha ayrıntılı olarak tanılamaya yardımcı olmak için etkilenen kullanıcıların listesini sağlar.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>"Güvenli olmayan form" algılama nasıl düzeltirim?
1. **Önceliklendirme.** Bildirim güvensiz formların sayısı ve veri güvenliğinin aşılmış kullanıcı sayısını sağlar. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Hangi formda güvensiz iletimlerin en büyük sayıyı dahil ve güvenli aktarımlara dağıtım zamanla nedir? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama güvensiz formlar listesini ve her formun sorunu daha ayrıntılı olarak tanılamaya yardımcı olması için güvenli aktarımlara sayısı dökümünü sağlar.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>"Şüpheli kullanıcı etkinliği" algılama nasıl düzeltirim?
1. **Önceliklendirme.** Bildirim şüpheli davranış sergilenen farklı kullanıcı sayısını sağlar. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Hangi ülkelerde şüpheli istekleri kaynaklanan? Hangi kullanıcı en şüpheli neydi? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Algılama Şüpheli kullanıcıların listesini ve her kullanıcının sorunu daha ayrıntılı olarak tanılamaya yardımcı olması için Ülke listesi sağlar.
