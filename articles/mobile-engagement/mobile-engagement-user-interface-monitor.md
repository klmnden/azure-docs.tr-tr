---
title: "Azure Mobile Engagement kullanıcı arabirimi - izleme"
description: "Azure Mobile Engagement kullanarak uygulamanız hakkında gerçek zamanlı verileri nasıl izleyeceğinizi öğrenin"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: b91ad89a-b89d-4377-abb0-cc2d16a2836d
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 5f8a02e35db93585e0fe46d77b3ad18b94c99597
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-real-time-data-about-your-application"></a>Uygulamanız hakkında gerçek zamanlı verileri nasıl izleyeceğinizi
Bu makalede **İZLEYİCİ** sekmesinde **Mobile Engagement** portal. Kullandığınız **Mobile Engagement** izlemek ve mobil uygulamalarınızı yönetmek için portal. Portalı kullanmaya başlamak için önce oluşturmanız gerektiğini unutmayın bir **Azure Mobile Engagement** hesabı. 

UI izleme bölümü gerçek zamanlı analiz bilgileri sağlar ve kullanılabilir ise aynı bilgilerin çoğu için eşiklerine ulaşıldığında uyarıları ayarlamak geçmişte içinde verir [ANALYTICS](mobile-engagement-user-interface-analytics.md) UI bölümü. Bkz: **sözlüğü** bölümüne [kavramları](http://go.microsoft.com/fwlink/?LinkId=525555) konu terim ve kısaltmaları analizi ve izleme tanımları için (aşağıdaki gibi: etkin kullanıcı, yeni kullanıcı, korunur kullanıcı, oturum, kullanıcı yolu grafik, kullanıcıların eşlemesi, izleme URL'leri, eğilimleri, etkinlik, olay, iş, hata, ek bilgileri, kilitlenme ve App-info).

> [!NOTE]
> Birçok bölümlerini **Mobile Engagement** portal UI içeren **YARDIMINI Göster** düğmesi. Bir bölümü hakkında daha fazla kavramsal bilgi almak için bu düğmesine basın.
> 
> 

## <a name="monitor---sessions-jobs-events-errors-and-crashes"></a>İzleyici - oturumları, işleri, olaylar, hatalar ve kilitlenme
Şu anda oturum ve belirli ekranlarda veya belirli eylemleri yapılması kaç kullanıcı olduğunu görebilirsiniz. Oturumları, işleri, olaylar, hatalar ve kilitlenme tarafından ayrılmış bir kullanıcı etkinliği görüntüleyebilirsiniz. Geçerli bilgileri görebilir ve son bir saat, gün veya hafta bilgileri göster. Belirli bir oturum, iş, olay, hata ve kilitlenme her kategori veya sıralama bilgilerin tümünü görebilirsiniz.  Canlı İzleme olup olmadığını bir uptick eylemde doğru anında iletme bildirimi gönderdikten sonra görmek için bir itme kampanya olaylarına sırasında kullanmak yararlıdır.

![Monitor1][14]  

## <a name="troubleshooting-with-monitor---events---details"></a>İzleyici - olayları - ayrıntılarla sorunlarını giderme
Test cihazınızdan uygulamanızdaki bir olay oluşturmak ve bunu bulma İzleyicisi - olayları - Ayrıntılar test cihazınız için cihaz Kimliğinizi bulmak için en kolay yollarından biri ve Azure Mobile Engagement analiz, izleme ve kesimleri tümleştirilmesi onaylamak için uygulamanızdan çalışmaktadır. Test aygıtınızın cihaz Kimliğine sahip olduğunuzda, test cihazlarınızı "Hesabı – Cihazlarım" ekleyebilirsiniz. Bir olay oluşturamıyorsanız, Azure Mobile Engagement SDK'sı, Android/iOS/Web/Windows/Windows Phone uygulamanızda düzgün tümleşiktir emin olun.

Daha fazla bilgi için bkz: [SDK Belgeleri][Link 5]

![Monitörü2][15]  

## <a name="troubleshooting-with-monitor---crashes---details"></a>İzleyici ile sorun giderme - Ayrıntılar çöküyor-
Uygulamanızı neden kilitlenen belirlemenize yardımcı olması için İzleyici - kilitlenme - Ayrıntılar uygulamanızdan ilgili kilitlenme bilgileri gözden geçirebilirsiniz. Ayrıca SDK sürüm notlarında Android/iOS/Web/Windows/Windows Phone SDK'ın her sürüm için her bir sürümü ile ilgili bilinen sorunlar görünmelidir.

Daha fazla bilgi için bkz: [SDK Belgeleri - sürüm notları][Link 5]

![Monitor3][16]

## <a name="monitor---alerts"></a>İzleyici - uyarıları
Ayrıca, otomatik olarak size e-posta veya anlık ileti gönderilecek uyarılar için koşullar belirtebilirsiniz. (Google GTalk XMPP ile uyumlu hizmetlerin ister veya Apple iChat desteklenir.) Uyarıları önceden tanımlanmış algılama eşiğini büyüktür (>) ya da (<) belirli sayıda oturumları, işleri, olaylar, hatalar veya kilitlenme ikinci, saat veya dakika başına'den temel alır. Uyarıları, belirli bir türde tüm etkinlikleri izlemek veya yalnızca belirli bir iş, olay veya hata etkinliğini izleyin. 

Ayrıca bir en düşük algılama, Uyarınız tetiklendiğinde, belirtilen aralık başına 1'den fazla bildirim hiçbir zaman alırsınız emin olmak aynı uyarının iki bildirimini ayıracak dakika en az miktarda olan hızı, belirtebilirsiniz.

![Monitor4][17]

## <a name="see-also"></a>Ayrıca bkz.
* [Kavramları][Link 6]
* [Sorun giderme kılavuzu hizmeti][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
