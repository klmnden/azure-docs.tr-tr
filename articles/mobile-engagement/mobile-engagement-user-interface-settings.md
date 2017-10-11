---
title: "Azure Mobile Engagement kullanıcı arabirimi - ayarlar"
description: "Azure Mobile Engagement kullanarak uygulamanızı genel ayarlarını yönetmeyi öğrenin"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 858f4cb4-14de-4bb5-826f-28cadbfc928b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af5c81df2b9f288161b38625d3ac2adde8fb195d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-the-global-settings-of-your-application"></a>Uygulamanızın genel ayarlarını yönetme
**Ayarları** menü seçeneklerini uygulama ve size verildi uygulama için izinler platforma bağlı olarak bir uygulama ayırmayı için kullanılabilir. Ayarları aşağıdakileri içerir: ayrıntıları, projeler, yerel gönderim, anında iletme hız, etiket (uygulama bilgisi) ve Ticari baskının. Etiket (uygulama bilgisi) menü seçeneği ayarları bölümünün uygulamanız (SDK'yı kullanarak) veya (cihaz API'sini kullanarak), arka uç tarafından yönetilebilir. 

> [!NOTE]
> Birçok bölümlerini **Mobile Engagement** portal UI içeren **YARDIMINI Göster** düğmesi. Bir bölümü hakkında daha fazla kavramsal bilgi almak için bu düğmesine basın.
> 
> 

## <a name="details"></a>Ayrıntılar
Ad ve açıklama, uygulamanızın değiştirmenize izin verir, uygulamanızı ve rol izinlerinizi sahibi görüntüleyin. 

Analizi Yapılandırması hafta başlatmak gün ve gün bekletme zamanında görüntülemek veya değiştirmek etkinleştirir.

  ![Ayarları1][46]

## <a name="projects"></a>Projeleri
Uygulamanızın görünmesini istediğiniz tüm projeleri seçmenize olanak sağlar. 

Ayrıca, proje için arama ve adı, açıklama, sahibi ve rol izinleri, uygulamanın parçası olduğu projesinin görüntüleyebilirsiniz.

Daha fazla bilgi için bkz: [UI belgelerine – giriş][Link 13]

  ![settings3][48]

## <a name="native-push"></a>Yerel gönderim
Yeni bir sertifika veya Sil ve kullanmak için var olan sertifika ile yerel gönderim kaydetmenizi sağlar. Yerel gönderim sağlayan herhangi bir zamanda uygulamanıza göndermek Azure Mobile Engagement bile zaman çalışır durumda. 

Kimlik bilgileri veya sertifikalar için en az bir yerel gönderim hizmet sağladıktan sonra "Herhangi bir zaman" seçebilirsiniz Reach kampanyaları ve ayrıca kullan "bildirim" parametresi anında API oluştururken.

### <a name="apple-push-notification-service-apns"></a>Apple anında iletilen bildirim servisi (APNS)
Apple anında iletilen bildirim Servisi'ni kullanarak yerel gönderim özelliğini etkinleştirmek için sertifikanızı kaydetmeniz gerekir. Sertifika türü geliştirme (Geliştirme) veya üretim (üretim) olarak belirtmeniz gerekir. Ardından, sertifikanızı ve parola karşıya.

Daha fazla bilgi için bkz: [SDK Belgeleri - iOS - uygulamanız Apple anında iletme bildirimleri için hazırlama][Link 5]

![settings4][49]

### <a name="windows-push-notification-service-wpns"></a>Windows anında bildirim hizmeti (WPNS)
Windows Bildirim Hizmeti'ni kullanarak Yerel Gönderim özelliğini etkinleştirmek için uygulamanızın kimlik bilgilerini sağlamanız gerekir. Paket güvenlik tanımlayıcınızı (SID) ve gizli anahtarınızı gerekir.

![settings5][50]

### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud Messaging (GCM) Android için
GCM kullanarak yerel gönderim özelliğini etkinleştirmek için Google yönergeleri izleyerek gerekir. Bir sunucu basit API anahtarı yapıştırmanız gerekir sonra IP kısıtlamaları yapılandırılmış. Android v1.12.0 + için SDK'sı ile tümleştirme gerektirir.

Daha fazla bilgi için bkz. 

* [SDK Belgeleri Android GCM tümleştirme][Link 5]
* [Google Developer GCM Kılavuzu](http://developer.android.com/guide/google/gcm/gs.html)

### <a name="amazon-device-messaging-for-android-adm"></a>Amazon cihaz Mesajlaşma (ADM) Android için
ADM kullanarak yerel gönderim özelliğini etkinleştirmek için Amazon sağlayın <OAuth credentials> oluşan bir istemci kimliği ve istemci parolası (SDK'sı ile tümleştirme gerektiren Android v2.1.0 + için).

Daha fazla bilgi için bkz. 

* [SDK Belgeleri Android ADM tümleştirme][Link 5]
* [Amazon Geliştirici ADM belgeleri](https://developer.amazon.com/sdk/adm/credentials.html#Getting)

![settings6][51]

## <a name="push-speed"></a>Gönderim hızı
Uygulamanızın gönderim hızını tanımlamanıza olanak sağlar ve geçerli, uygulamanızın gönderim hızını gösterir.

  ![settings7][52]

## <a name="tag-app-info"></a>Etiket (uygulama bilgisi)
![settings11][56]

## <a name="commercial-pressure"></a>Ticari baskının
![settings12][57]

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

