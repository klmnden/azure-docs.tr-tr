---
title: "Azure Mobile Engagement kullanıcı arabirimi - Reach nasıl yapılır"
description: "Azure Mobile Engagement için kullanıcı arabirimi genel bakış"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 33a0a9d0c399cb7f0a791c4c16dde2e2d62364ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Son kullanıcılarınıza ulaşmak için kullanarak ve yönetmeye başlamak nasıl iter
SDK'sını uygulamanıza tam olarak tümleşiktir sonra kullanmaya başlamak, uygulamanızın kullanıcılara anında iletme bildirimleri UI ulaşma bölüm.  

## <a name="do-your-first-push-notification-campaign"></a>İlk anında iletme bildirimi kampanyanızı yapın
* SDK'sı ile uygulamanızı elinizin tümleştirilmiş onaylayın. 
* Uygulamanızı seçin

![First1][1]

* "Reach" bölümü ve tıklatın "Yeni duyuru için" gidin

![First2][2]

* Yeni bir kampanya oluşturun ve adlandırın
  
![First3][3]

* Nasıl bildirim, uygulama yalnızca gönderilmesi gereken seçin

![First4][4]

* Anında iletme istediğiniz iletiyi oluşturun

![First5][5]

* Bir başlık (isteğe bağlı) bildirim yazabilir.
* Anında iletme iletisi içeriği yazma.
* Görüntüyü karşıya yükleyebilirsiniz. Dosyanın boyutu 32.768 baytı aşamaz unutmayın.
* Ayrıca daha fazla seçenek seçmek olanağına sahip, ancak bu öğretici için odak, daha sonra göreceğiz.
* Yalnızca bildirim olarak içerik türünü seçin

![First6][6]

* Anında iletme kampanyanızı oluşturmak ve kampanya listenizde görüntülenir.

![First7][7]

## <a name="test-your-push-notification-campaign"></a>Anında iletme bildirimi kampanyanızı test
![Test1][8]

* Cihazınızı kaydedin.
* Anında iletme cihazın onay kutusuna tıklayın.
* Anında iletme cihaza göndermek için "Test" düğmesine tıklayın.

![Test2][9]

* Kampanya etkinleştir

![test3][10]

* Kampanyanızı oluşturduğunuza göre yalnızca kullanıcılarınıza edilmesini bildirim için etkinleştirmeniz gerekir.

## <a name="send-personalized-pushes"></a>Kişiselleştirilmiş bildirim gönder
* Bu örnek, bir özel indirim kod anında iletme bildirimi girildiği push oluşturur.

![Personalize1][11]

Kişiselleştirme çalışır, böylece tarafından bir işaretçi bir uygulama bilgisi etiketinde değiştirerek kullanıcının ilk olarak tanımlanan uygun app-info olduğundan emin olmak gerekir. Bu örnekte ve hedef kullanıcı tanımlı rebate_code adlı bir uygulama bilgisi etiketi sahip olur.
Yukarıda gördüğünüz gibi anında bildirim içeriği uygulama bilgisi etiketi gerçek içerik tarafından değiştirilmesini olduğunu bildirecektir işaret ${rebate_code} içerir.

> [!WARNING]
> Uygulama bilgileri etiketi için kullanıcı tanımlı değil, kullanıcı anında alır değil.

* Sonuç

![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Daha fazla metin bildiriminizin kişiselleştirebilirsiniz
![Personalize3][13]

* Bildirim başlığı dahil olmak üzere,
* Ve ileti içeriği.
* Duyuru (metin görünümü veya Web görünümü) türünü seçin

![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Duyuru gövdesi ile de kişiselleştirilmiş:
* Eylem URL'si giriş sayfası özelleştirmek istediğiniz
* Başlık
* İleti gövdesi.

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Bilgisayarınızı anında iletilen bildirim (içeri veya uygulama dışında) ayırt
* Anında iletme, uygulamanızı seçin, "Reach" bölümüne gidin, seçin veya bir anında iletme kampanya oluşturacak ve "Bildirim" bölümüne gidin bildirim türünü seçin.
* ' I tıklatın, "teslimat modunu" istediğiniz.
* Bildirim istediğinizde "Kısıtlamak etkinlikler" onay kutusunu tıklayarak belirli etkinlikleri (ekranları) oluşur.

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Yalnızca uygulama dışında" teslimat
![Differentiate2][16]

"Yalnızca uygulama dışında" teslimat uygulama kapatıldığında anında iletme bildirimi sağlar. Standart anında iletme bildirimi budur.
"Yalnızca uygulama dışı" seçtiğinizde, zaten uygulamanız (APNS veya GCM) oluşturuyor platform sertifikalardan sağlamış olmanız gerekiyor.

### <a name="see-also"></a>Ayrıca bkz.
* [Apple anında bildirim hizmeti – sertifikaları](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google bulut Mesajlaşma – sertifika](http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"uygulama yalnızca" teslimat
![Differentiate3][17]

Uygulama çalışırken "Uygulama yalnızca" teslimat anında iletme bildirimi sağlar.
Bu bildirim için APNS ve GCM sistem üzerinden gitmek gerekmez.
Uygulama teslim sistemi kullanıcılarınıza ulaşmak için kullanabilirsiniz.
Tam bildirim özelleştirmek ve etkinlik (ekran) bildirim görüntüleneceği karar verebilirsiniz.

### <a name="anytime-delivery-mode"></a>"Dilediğiniz zaman" teslimat
Bir "Her zaman" teslimat seçebilirsiniz, uygulama veya çalışıp çalışmadığını kullanıcınızın ulaşması sağlar.
"Dilediğiniz zaman" seçtiğinizde, zaten uygulamanız (APNS veya GCM) oluşturuyor platform sertifikalardan sağlamış olmanız gerekiyor. 

## <a name="schedule-a-push-campaign"></a>Anında iletme kampanya zamanlama
### <a name="plan-to-start-a-campaign"></a>Bir kampanya başlatmak planlama
![Shedule1][18]

21 Mart olduğu ve yapmak için bir bildirime sahip, 22 Mart için gece yarısı planlanmış. Push yapmak için arabirimi önünde kalmak gerekmez! Önceden bildirimleri gönderilecek tam dakika planlayabilirsiniz.

* Kaldırma onay "Hiçbiri" onay kutusunu ve bir başlangıç saati seçin 
* Tarih ve itme kampanya başlamasını istediğiniz saati seçin.

### <a name="plan-to-end-a-campaign"></a>Bir kampanya sona erdirmek planlama
![Shedule2][19]

Kampanyanızı 25 Mart üzerinde 3:00 pm durdurmak istiyor ancak orada yapmak için olmayacaktır biliyor.
İtme arabirimi önünde kalmak gerekmez! Önceden, kampanya durur tam dakika planlayabilirsiniz.

* Tıklayın "yok" onay kutusunu veya bir bitiş saati seçin
* Tarih ve saat itme kampanya tamamlamak istiyorsanız seçin.

### <a name="end-a-campaign-manually"></a>Bir kampanya el ile bitmelidir
![Shedule3][20]

Varsayılan olarak, "None" onay kutuları seçilir.
Başka bir deyişle, ulaşma bölümünde etkinleştirmek ve reach bölümüne durdurulacak zaman sona ereceği hemen sonra kampanya başlar.

> [!NOTE]
> Bitiş tarihi oluşturulan Kampanyalar itme cihazda yerel olarak depolamak ve kampanya el ile sona erdi olsa bile uygulama sonraki açılışında gösterir.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Metin görünümü ile anında iletme bildirimi geliştirin
### <a name="what-is-a-text-view"></a>Metin Görünümü nedir?
![TextView1][21]

Metni metin içeriği içeren bir açılır pencere görülmektedir. Son kullanıcı anında iletme bildirimine tıklamıştır sonra bu açılır pencere görünür.
Metin görünümü, son kullanıcı için daha fazla içerik sunmanızı sağlar. Bu da bir coğrafi olarak yerelleştirilmiş arama başlayarak bir e-posta gönderilirken bir web sayfası açılıyor bir mağazaya, yeniden yönlendirme, uygulamanızın bir sayfa atlama gibi bir işlem için bir çağrı sunmak için fırsattır vb....

### <a name="example-text-view"></a>Örnek: Metin görünümü
* "Reach" bölümünde anında iletme bildirimi kampanyanızı oluşturmak ve kampanyanızı adlandırın

![TextView2][22]

* Görüntülenecek ileti bildirimi yazma.
* "Metin" Duyuru içerik türünü seçin

![TextView3][23]

> [!NOTE]
> Metin görünümü bastığınızda, her zaman içeren bir bildirim önce gelir. 

* Metin (metin duyuru içeriği alt bölümünün görünür, görüntülenmesini istediğiniz metin tanımlamanıza olanak sağlayan seçtikten sonra.) tanımlayın

![TextView4][24]

* İleti üstünde görünür başlık yazın.
* Metin görünümü ana içeriğini yazma.
* (Bir uygulama mağazasının veya herhangi bir tür, sağlayabilirsiniz kaynakları yönlendirme uygulama sayfasını açarak gibi belirli bir eylemi yapmak uygulama eylem düğmesi sağlar) eylem düğmesine görünür içerik yazma.
* Çıkış düğmesi görünür içerik yazma (çıkış düğmeyi tıklatarak, metin görünümü kaybolur.)
* Anında iletme bildirimi kampanyanızı oluşturmak ve kampanya listede görüntülenir.

![TextView5][25]

* Metin görünümü, kullanıcılara göndermek için anında iletme bildirimi kampanyanızı etkinleştirin.

![TextView6][26]

* Sonuç

![TextView7][27]

* Kullanıcı onu tıklayın ve bildirim alır.
* Metin görünümü ile etkileşim arkasından bir açılır pencere görünür.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Bir Web görünümü ile anında iletme bildirimi geliştirin
### <a name="what-is-a-web-view"></a>Bir Web Görünümü nedir?
![WebView1][28]

Bir web görünümü web içeriği içeren bir açılır pencere ' dir. Bu açılır pencere, son kullanıcı anında iletme bildirimine tıkladığında açılır.
Web görünümü son kullanıcı ile daha fazla etkileşime girmenizi sağlar.
Bu aynı zamanda App Store için yeniden yönlendirme gibi eylem çağrısı sunmak için fırsattır bir web sayfasını açarak, bir e-posta gönderme, coğrafi olarak yerelleştirilmiş bir arama başlayarak, vb....

### <a name="example-web-view"></a>Örnek: Web görünümü
* "Reach" bölümünde anında iletme kampanyanızı oluşturmak ve kampanyanızı adlandırın.

![WebView2][29]

* Görüntülenecek ileti bildirimi yazma.
* "Web" Duyuru içerik türünü seçin

![WebView3][30]

### <a name="about-announcement-types"></a>Duyuru türleri hakkında:
* Yalnızca bildirim: Basit standart bir bildirim değil. Bu kullanıcı üzerinde tıklatır, ek görünüm yok görünür, ancak yalnızca kendisine ilişkili eylemin oluşacağını anlamına gelir.
* Metin duyuru: metin görünümü bakın kullanıcıya prosese bir bildirimidir.
* Web duyuru: kullanıcının bir web görünümü göz prosese bir bildirimidir.
  "Web duyuru" içeriğini seçin.

> [!NOTE]
> Bir web görünümü bastığınızda, her zaman içeren bir bildirim önce gelir.

* (Web duyuru içeriği alt bölümde görünür, görüntülenmesini istediğiniz web görünümü içeriğini tanımlamanıza olanak sağlayan seçtikten sonra.) web içeriğini tanımlayın

![WebView4][31]

* (İsteğe bağlı) iletinin üstünde görünür başlık yazın.
* HTML kodunuzu buraya yazın.
* Kaynak düzenleme modu düğmesini edition geçmek ve nasıl nasıl göründüğünü görmek için tıklayın.
* (Bir mağazaya veya herhangi bir tür, sağlayabilirsiniz kaynakları yönlendirme uygulama sayfasını açarak gibi belirli bir eylemi yapmak uygulama eylem düğmesi sağlar) eylem düğmesine görünür içerik yazma.
* Çıkış düğmesi görünür içerik yazma (çıkış düğmeyi tıklatarak, web görünümü kaybolur).
* Sonuç

![WebView5][32]

* Kullanıcı bildirimi ve tıklayın.
* Metin görünümü ile etkileşim arkasından bir açılır pencere görünür.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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

