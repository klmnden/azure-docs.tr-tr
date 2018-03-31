---
title: Azure Mobile Engagement kullanıcı arabirimi - ulaşma içeriği
description: Anında iletme bildirimi kampanyaları Azure Mobile Engagement, farklı türdeki benzersiz içerik yönetmeyi öğrenin
services: mobile-engagement
documentationcenter: ''
author: piyushjo
manager: erikre
editor: ''
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 982cc66ffe98aa6dff8fe290cc1c2d4bad03c9ac
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Anında iletme bildirimi kampanyaları farklı türde benzersiz içeriği yönetme
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

İçerik Duyurular, anketler, veri iter ve Kutucuklar (yalnızca Windows Phone) değiştirmek için yeni bir reach kampanya içerik bölümünü kullanın. İçerik anında iletme kampanyalarını kampanya türüne belirli ayarıdır. 

### <a name="content-types"></a>İçerik türleri:
* Duyurular
* Anketler
* Veri gönderimleri
* Kutucuklar (yalnızca Windows Phone)

## <a name="content-of-announcements"></a>Duyurular içeriği
 ![Reach-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Duyurunuzun türünü seçin:
* Yalnızca bildirim: Basit standart bir bildirim değil. Bu kullanıcı üzerinde tıklatır, ek görünüm yok görünür, ancak yalnızca kendisine ilişkili eylemin oluşacağını anlamına gelir.
* Metin duyuru: metin görünümü bakın kullanıcıya prosese bir bildirimidir.
* Web duyuru: kullanıcının bir web görünümü göz prosese bir bildirimidir.

### <a name="see-also"></a>Ayrıca bkz.
* [Ulaşmaya - nasıl yapılır - duyuruları][Link 3] 

### <a name="about-web-view-announcements"></a>Web görünümü duyuruları hakkında:
Deseninin "{DeviceID}" Burada sağladığınız HTML kodunda veya JavaScript kodunda, duyuruyu görüntüleyen cihazın tanımlayıcısı ile otomatik olarak değiştirilir. Bu işlem arka ofisinizde barındırılan bir dış web hizmetindeki Azure Mobile Engagement cihaz tanımlayıcılarının alınması için kolay bir yoludur.
Bir tam ekran web görünümü oluşturmak isterseniz (sağladığımız varsayılan Eylem ve Çıkış düğmeleri olmadan), web görünümü duyurularının JavaScript kodundan aşağıdaki işlevleri kullanabilirsiniz: 

* Duyuru eylemini gerçekleştir: ReachContent.actionContent()
* duyurudan Çık: ReachContent.exitContent()

### <a name="choose-your-action"></a>Eylem seçin:
### <a name="about-action-urls"></a>Eylem URL'ler hakkında:
Hedeflenen cihazın işletim sistemi tarafından yorumlanabilen herhangi bir URL'nin bir eylem URL'si olarak kullanılabilir.
Herhangi bir ayrılmış uygulamanızın destekleyebileceği URL (örn. kullanıcıların belirli bir ekrana atlaması için) eylem URL'si olarak da kullanılabilir.
{DeviceID} deseninin her oluşumu, eylemi gerçekleştiren cihazın tanımlayıcısı ile otomatik olarak değiştirilir. Bu işlem arka ofisinizde barındırılan bir dış web hizmeti aracılığıyla Azure Mobile Engagement cihaz tanımlayıcılarını kolayca almak için kullanılabilir.

* **Android + iOS Eylemler**
  * Bir web sayfasını açın
  * http://\[web-site-domain\] 
  * Örnek:http://www.azure.com
  * Bir e-posta Gönder
  * mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\] 
  * Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
  * SMS gönder
  * SMS:\[telefon numarası\] 
  * Example:sms:2125551212
  * Telefon numarası çevir
  * Tel:\[telefon numarası\] 
  * Example:tel:2125551212
* **Android yalnızca Eylemler**
  * Play Store'dan bir uygulama indirin
  * Market://details?id=\[uygulama paketi\] 
  * Örnek: market://details?id=com.microsoft.office.word
  * Coğrafi olarak yerelleştirilmiş bir arama Başlat
  * geo:0,0?q=\[search query\] 
  * Örnek: geo:0, 0? q starbucks, paris =
* **iOS yalnızca Eylemler**
  * App Store'dan bir uygulama indirin
  * http://itunes.apple.com/[country] /app/ [uygulama adı] /id [uygulama kimliği]? mt = 8 
  * Örnek:http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
  * Windows işlemleri
  * Bir web sayfasını açın
  * http://\[web-site-domain\] 
  * Örnek:http://www.azure.com
  * Bir e-posta Gönder
  * mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\] 
  * Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
  * (Skype Store App gereklidir) SMS gönder
  * SMS:\[telefon numarası\] 
  * Example:sms:2125551212
  * Bir telefon numarası (Skype Store App gereklidir) arama
  * Tel:\[telefon numarası\] 
  * Example:tel:2125551212
  * Play Store'dan bir uygulama indirin
  * MS-windows-deposu: PDP? PFN =\[uygulama paket kimliği\] 
  * Example:ms-windows-store:PDP?PFN=4d91298a-07cb-40fb-aecc-4cb5615d53c1
  * Bingmaps Arama Başlat
  * bingmaps:?q=\[search query\] 
  * Örnek: bingmaps:? q starbucks, paris =
  * Özel bir şema kullanın
  * \[Özel şema\]://\[özel şema parametreleri\] 
  * Example:myCustomProtocol://myCustomParams
  * Paket verileri (Store App gerekli okuma uzantısı) kullan
  * \[folder\]\[data\].\[extension\] 
  * Example:myfolderdata.txt

### <a name="build-a-tracking-url"></a>Bir izleme URL'si oluşturun:
* "Ayarlar" bölümüne bakın <UI Documentation> için bir izleme URL'si oluşturmak üzerinde yönerge kullanıcıların diğer uygulamalarınızdan birini indirmeye izin verir.

### <a name="define-the-texts-of-your-announcement"></a>Duyurunuzun metinlerini tanımlayın
Başlık, içerik ve düğmesi duyurunuzun metinlerini doldurun. Kullanıcılar bu kampanyaya nasıl yanıt, erişim görüş dayanarak gelecekteki bir kampanyanın bir izleyici hedefleyebilirsiniz. Dinleyici olup bu kampanyayı yalnızca, yanıtlanan, eylem yapılan veya Çıkılan gönderilen geribildirim bağlı olabilir.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - Reach - yeni itme ölçüt][Link 28]

## <a name="content-of-polls"></a>Anketler içeriği
![Reach-Content2][31] 

Başlık, açıklama ve düğmesi duyurunuzun metinlerini doldurun. Ardından, sorular ve sorularınızın yanıtlarını seçenekleri ekleyin.
Kullanıcılar bu kampanyaya nasıl yanıt, erişim görüş dayanarak gelecekteki bir kampanyanın bir izleyici hedefleyebilirsiniz. Dinleyici olup bu kampanyayı yalnızca, yanıtlanan, eylem yapılan veya Çıkılan gönderilen bağlı olabilir. Dinleyici da yoklama yanıt geri bildirim, soru ve yanıt seçim ölçütü olarak kullanıldığı temel alabilir.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - Reach - yeni itme ölçüt][Link 28]

## <a name="content-of-data-pushes"></a>Veri gönderimleri içeriği
![Reach-Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Verilerinizin türünü seçin:
* Metin
* İkili veriler
* Base64 veri

### <a name="define-the-content-of-your-data"></a>Verilerinizin içeriğini tanımlayın
* Metin veri göndermeyi seçtiyseniz, kopyalamak ve metin "içerik" kutusuna yapıştırın.
* İkili veya base64 veri göndermeyi seçtiyseniz, dosyanızı karşıya yüklemek için "dosyanızı karşıya yükle" düğmesini kullanın.
* Kullanıcılar bu kampanyaya nasıl yanıt, erişim görüş dayanarak gelecekteki bir kampanyanın bir izleyici hedefleyebilirsiniz. Dinleyici olup bu kampanyayı yalnızca, yanıtlanan, eylem yapılan veya Çıkılan gönderilen bağlı olabilir.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - Reach - yeni itme ölçüt][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>İçerik döşeme (yalnızca Windows Phone)
![Reach-Content4][33]

### <a name="define-the-content-of-your-tile"></a>Kutucuğunuzun içeriğini tanımlayın
Döşeme yükü uygulamanızı Windows Phone cihazlarda döşemesinin görüntülenecek metindir.
Döşeme push yerel gönderim Windows Phone için Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) sürümüdür. Döşeme anında iletme türü yanıt yok yalnızca gönderme türü ve gelecekteki Kampanyalar İzleyici döşeme itme kampanya sonuçlarına böylece oluşturulamıyor. 

### <a name="see-also"></a>Ayrıca bkz.
* [API belgelerine - ulaşma API - yerel gönderim][Link 4]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

