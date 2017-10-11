---
title: "Sorun giderme kılavuzu - API'leri azure Mobile Engagement"
description: "Azure Mobile Engagement - API'leri için kılavuzları sorunlarını giderme"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: a7ae0a83046f2d67b790f672dcd3ae261987357a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a>API sorunları için sorun giderme kılavuzu
Azure Mobile engagement API'leri aracılığıyla nasıl Yöneticiler etkileşimde karşılaşabileceğiniz olası sorunlar şunlardır:

## <a name="syntax-issues"></a>Sözdizimi sorunları
### <a name="issue"></a>Sorun
* API (ya da beklenmeyen davranışlarla) kullanarak söz dizimi hataları.

### <a name="causes"></a>Neden olur.
* Sözdizimi sorunlar:
  * Seçenek kullanılabilir olduğunu doğrulamak için kullandığınız belirli API sözdizimi olduğundan emin olun.
  * Bir ortak API kullanımı Reach API'sini ve anında iletme (görevlerin çoğunu itme API yerine Reach API'sini ile yapılmalıdır) API karmaşık hale getirmeye sorunudur. 
  * Başka bir ortak SDK tümleştirmesi ve API kullanımı ile SDK anahtarı ve API anahtarını karmaşık hale getirmeye sorunudur.
  * API'lerine bağlanmak komut dosyalarını en az 10 dakikada veri göndermek için gereken veya bağlantı zaman aşımı (verilerini dinleme İzleyici API komut ortak özellikle) olur. Zaman aşımları önlemek için kodunuzu XMPP ping her 10 dakikada oturum sunucuyla Canlı göndermek sahip.

### <a name="see-also"></a>Ayrıca bkz.
* [API belgeleri][Link 4]
* [XMPP protokol bilgileri](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Azure Mobile Engagement Arabiriminde kullanılabilir aynı eylemi gerçekleştirmek için API kullanılamıyor
### <a name="issue"></a>Sorun
* Azure Mobile Engagement Arabiriminden çalışır bir eylem ilgili Azure Mobile Engagement API'SİNDEN işe yaramaz.

### <a name="causes"></a>Neden olur.
* Azure Mobile Engagement Arabiriminden aynı eylemi gerçekleştirebilirsiniz onaylayan SDK ile Azure Mobile Engagement'ın bu özelliği doğru Tümleştirildi gösterir.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri][Link 1]

## <a name="error-messages"></a>Hata iletileri
### <a name="issue"></a>Sorun
* Hata kodları çalışma zamanında ya da günlükleri görüntülenen API'yi kullanma.

### <a name="causes"></a>Neden olur.
* Başvuru ve ön sorun giderme için ortak API durum kodları numaraları bileşik listesi aşağıdadır:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Ayrıca bkz.
* [-Her belirli API ayrıntılı hataları için API belgeleri][Link 4]

## <a name="silent-failures"></a>Sessiz hataları
### <a name="issue"></a>Sorun
* API Eylem günlükleri ya da çalışma zamanında görüntülenen hiçbir hata iletisiyle başarısız olur.

### <a name="causes"></a>Neden olur.
* Çok sayıda öğe doğru bütünleştirdiyseniz değil Azure Mobile Engagement Arabiriminde devre dışı bırakılacak ancak sessizce API'SİNDEN başarısız, de şekilde çalışıp çalışmadığını görmek için kullanıcı Arabirimi aynı işlevlerini test unutmayın.
* Azure Mobile Engagement ve Azure Mobile Engagement kullanmak için çalıştığınız birçok gelişmiş özelliklerini kullanabilmek için önce ayrı ayrı uygulamanıza SDK'sı ayrı adımlar gerekiyor.

### <a name="see-also"></a>Ayrıca bkz.
* [Sorun giderme kılavuzu - SDK][Link 25]

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

