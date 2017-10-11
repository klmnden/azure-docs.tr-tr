---
title: "Sorun giderme kılavuzu - SDK'sı azure Mobile Engagement"
description: "Azure Mobile Engagement SDK tümleştirmesi sorunlarını giderme"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: de265cf1-2f88-43ef-8616-156ada5be7b6
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4d9d6165deb4bd0c65f1841aa7c457363a1f2865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>SDK tümleştirme sorunları için sorun giderme kılavuzu
Azure Mobile Engagement'ın uygulamanıza nasıl tümleştirilir karşılaşabileceğiniz olası sorunlar şunlardır:

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Bir hata, uygulamanızın başka bir alan keşfedilen SDK sorunları
### <a name="issue"></a>Sorun
* UI veri toplama hatası (Analytics, izleme, Segment veya panolar).
* Hataları gönderme (uygulama ya da her ikisini de dışında uygulamasında iter çalışmıyor).
* Özellik hataları (izleme, coğrafi konuma veya belirli iter çalışmıyor platform) Gelişmiş.
* API hataları (API'leri başarısız genellikle hata iletileri olmadan sessizce).
* Hizmet hataları (hiçbiri Azure Mobile Engagement uygulamanız için çalışır).

### <a name="causes"></a>Neden olur.
* Uygulamanız (örneğin, bir kullanıcı Arabirimi veri toplama hatası, gönderme hatası, gelişmiş özellik hatası, API hatası, uygulama kilitlenmelerine veya görünen hizmet kesintisi) bir hata ile Azure Mobile Engagement SDK'sı çözülmesi gereken çoğu sorunları bulunacaktır.  
* Azure Mobile Engagement, belirli bir özellik, hiçbir zaman uygulamanızda önce çalıştıysa tümleştirme tamamlanması gerekir. 
* Azure Mobile Engagement, belirli bir özellik çalıştığı ve durdurulmuş, en son Azure Mobile Engagement SDK'sı sürüm yükseltmeniz gerekebilir. Azure Mobile Engagement (Android, iOS, Windows ve Windows Phone) tarafından desteklenen her platform için Azure Mobile Engagement SDK'sı farklı bir sürümü olduğunu unutmayın.

#### <a name="sdk-integration"></a>SDK Tümleştirmesi
* Azure Mobile Engagement SDK'yı (Analytics) doğru biçimde tümleşik.
* SDK (uygulama içinde ve dışında uygulama iter) doğru biçimde tümleşik ulaşabilirsiniz.
* Süresi dolmuş veya hatalı ürün vs sertifika. Geliştirme (yalnızca iOS).
* GCM veya ADM SDK'da tümleşik doğru biçimde (Android yalnızca - hizmet belirli iter).
* Doğru biçimde izleme SDK (yükleme deposu izleme) tümleşiktir.
* Yavaş veya GPS yerleşimine SDK (hedefleme coğrafi konuma göre) doğru biçimde tümleşik.

**Ayrıca bkz.:**

* [SDK Belgeleri - tümleştirme kılavuzları][Link 5] 
* [Sorun giderme kılavuzu - gönderme][Link 23]

#### <a name="sdk-upgrade"></a>SDK yükseltme
* SDK'sını (genellikle daha yeni sürümleri aygıt işletim sistemi için ilgili) SDK'ın eski sürümleri ile ilgili sorunları gidermek yükseltme yapmanız gerekir.
* Cihazınızı uygulamanızı önceki tüm sürümlerini kaldırın ve en yeni sürümü, uygulamanızın, yeniden kaydettirin Cihazınızı uygulamanızı en yeni sürümünü kullandığını doğrulamak için Azure Mobile Engagement UI, cihaz kimliği yükleyin.

**Ayrıca bkz.:**

* [SDK Belgeleri - sürüm notları](http://go.microsoft.com/fwlink/?LinkId= 525554) 
* [SDK Belgeleri - yükseltme kılavuzları](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK diğer
* Uygulama bildirimi "AndroidManifest.xml" hataları Azure Mobile Engagement'ı (yalnızca Android) çalışmamasına neden olabilir.
* SDK anahtarı ve API anahtarını karmaşık hale getirmeye SDK tümleştirmesi ve API kullanımı ile yaygın bir sorun var.

**Ayrıca bkz.:**

* [Kavramlar - sözlüğü][Link 6]

## <a name="advanced-coding-issues"></a>Sorunları kodlama Gelişmiş
### <a name="issue"></a>Sorun
* İOS, Android ve Windows Phone platform belirli kodu doğrudan Azure Mobile Engagement için ilgili sorunlara neden olabilir.

### <a name="causes"></a>Neden olur.
* Birçok Azure Mobile Engagement kodlama sorunları Gelişmiş nedeni yanlış yazılmış platform belirli kodu Azure Mobile Engagement için doğrudan ilgili değildir. Azure Mobile Engagement belgeleri (Android, iOS, Web, Windows ve Windows Phone) yanı sıra için geliştirdiğiniz platformu belirli belgelere gerekecektir.
* Doğru biçimde "Kategoriler" yapılandırma, başka bir konum içinde veya dışında (yalnızca Android) uygulama bildirimden bağlanma engeller. 
* "UIKit.framework" "isteğe bağlı" "simgesi bulunamadı hatası" gösterir, iOS kodunuzun ayarını değil ve/veya eski iOS cihazlarda (yalnızca iOS) çöküyor.
* Süresi dolan sertifikaları veya doğru biçimde nedenler cert geliştirme veya üretim sürümünü kullanarak anında iletme (yalnızca iOS) verir.
* Devralınan (system center için uygulamanın oturumunu işleyişi Android ve iOS iter gibi), Azure Mobile Engagement denetleyemezsiniz bir platform için bazı sınırlamalar vardır.
* Azure Mobile Engagement tam başvuru için iOS ve Android için Azure Mobile Engagement tarafından kullanılan iç paketlerin listesini yayımlar. Azure Mobile Engagement özelliklerinden bazıları (Android, iOS, Web, Windows ve Windows Phone) platforma özgü aklınızda bulundurun.

### <a name="see-also"></a>Ayrıca bkz.
* [Sorun giderme kılavuzu - gönderme][Link 23] 
* [SDK Belgeleri - sürüm notları][Link 5]
* [SDK Belgeleri - yükseltme kılavuzları][Link 5]

## <a name="application-crashes"></a>Uygulama çökme (Crash)
### <a name="issue"></a>Sorun
* Uygulamanız son kullanıcıların cihazda çöküyor.

### <a name="causes"></a>Neden olur.
* Kilitlenme bilgi görüntülenebilir *Analytics UI* veya *Analytics API*
* Test Cihazınızı cihaz Kimliğini bulun ve kilitlenme uygulamanıza, kilitlenme nedenini belirlemeye yardımcı olmak bir son kullanıcı için neden aynı adımları uygulayın.
* Azure Mobile Engagement SDK'sı uygulamaların kilitlenmesine neden bilinen sorunlar bazen SDK'ın en son sürüme yükseltme tarafından çözümlenir. Sürüm Notları platformunuz hakkında kilitlenme araştırırken olduğundan emin olun.

### <a name="see-also"></a>Ayrıca bkz.
* [SDK Belgeleri - sürüm notları][Link 5]
* [SDK Belgeleri - yükseltme kılavuzları][Link 5]

## <a name="app-store-upload-failures"></a>Uygulama mağazası karşıya yükleme hataları
### <a name="issue"></a>Sorun
* Apple, Google veya Windows uygulama mağazası uygulamanızı en son sürümünü yükleme için ilgili hatalar.

### <a name="causes"></a>Neden olur.
* Uygulama mağazaları bazen belirli özellikleri etkin blok uygulamalar (örneğin Apple Store Mağazası'ndaki uygulamaların IDFV kullanımda ve uygulamalar arasında uygulama bilgi paylaşımı GooglePlay deposu engeller). 
* Depoya bir uygulama karşıya güçlük çekiyorsanız, platform ve SDK'ın geçerli sürümü ile ilgili sürüm notları denetlediğinizden emin olun.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
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

