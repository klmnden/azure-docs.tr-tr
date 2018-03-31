---
title: Azure Mobile Engagement kullanıcı arabirimi - ulaşma
description: Azure Mobile Engagement kullanarak anında iletme bildirimleri ile uygulamanızın kullanıcılarına ulaşın öğrenin
services: mobile-engagement
documentationcenter: ''
author: piyushjo
manager: erikre
editor: ''
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d999b83df7d9d467f08ce8ec72468c738e8acfa5
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Nasıl anında iletme bildirimleri ile uygulamanızın kullanıcılarına ulaşın
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu makalede **ULAŞMAK** sekmesinde **Mobile Engagement** portal. Kullandığınız **Mobile Engagement** izlemek ve mobil uygulamalarınızı yönetmek için portal. Portalı kullanmaya başlamak için önce oluşturmanız gerektiğini unutmayın bir **Azure Mobile Engagement** hesabı. Daha fazla bilgi için bkz: [bir Azure Mobile Engagement hesabı oluşturma](mobile-engagement-create.md).

UI ulaşma bölüm oluşturma/düzenleme/etkinleştirmek/bitiş/İzleyici yapabileceğiniz itme kampanya yönetim aracıdır ve anında iletme bildirimi kampanyaları ve ayrıca Reach API'sini (ve düşük düzeydeki itme API bazı öğeler) erişilebilen özelliklere istatistiklerini alın. API'ler veya kullanıcı arabirimini kullanıp kullanmadığınızı tümleştirmek gereken unutmayın Azure Mobile Engagement ve her platform için uygulamanıza ulaşma kullanabilmeniz için SDK ile Reach kampanyaları.

> [!NOTE]
> Birçok bölümlerini **Mobile Engagement** portal UI içeren **YARDIMINI Göster** düğmesi. Bir bölümü hakkında daha fazla kavramsal bilgi almak için bu düğmesine basın.
> 
> 

## <a name="four-types-of-push-notifications"></a>Dört tür anında iletme bildirimleri
1. Duyurular - uygulamanızı içinde başka bir konuma yönlendirmek kullanıcılara reklam iletileri göndermek için veya bir Web sayfası veya mağaza uygulamanızı dışında Göndereceğim olanak tanır. 
2. Anketler - sorular sorarak son kullanıcılardan bilgi toplamanızı sağlar.
3. Veri gönderimleri - bir ikili veya base64 veri dosyası göndermenizi sağlar. Veri gönderiminde yer alan bilgiler, kullanıcılarınızın geçerli deneyimini değiştirmek için uygulamanıza gönderilir. Uygulamanızın veri gönderimi verileri işlemek gerekir.

## <a name="campaign-details"></a>Kampanya ayrıntıları
Düzenleme, kopyalama, silme veya henüz adlarını gelerek etkinleştirilmedi Kampanyalar etkinleştirmek veya onları açmak için tıklatın. Adlarını gelerek zaten etkinleştirilmiş Kampanyalar kopyalayabilir veya bunları açmak için tıklatın. Ancak, etkinleştirildikten sonra bir kampanya değiştiremezsiniz.

![Reach1][18]

## <a name="reach-feedback"></a>Geri bildirim ulaşmak
Tıklayın **istatistikleri** Reach kampanya ayrıntılarını görmek için. **Basit** bir kampanyanın etkinleştirildiği sonra ne hakkında bir sütun çubuk grafik biçiminde görsel bir görünümünü sağlar. **Gelişmiş** görünümü itme kampanyayla ilgili daha ayrıntılı ayrıntıları sağlar. Bir test kampanya bir test cihazı gönderilen anında iletme yani gönderiyorsanız, bu ayrıntıları kullanılamaz. Bu ayrıntılar nasıl yorumlanacağı aşağıda verilmiştir:

1. **Gönderilen** -bu cihazlara gönderilen ileti sayısını belirtir. Bu sayı itme kampanya oluşturulurken belirtilen hedef kitle bağlıdır. Tüm hedef kitle belirtmezseniz, ardından bu itme kayıtlı cihaza gönderilir. Diğer tüm itme Hizmetleri gibi size doğrudan cihazlara anında iletme bildirimleri değil, ancak bunun yerine ilgili platformuna anında iletme belirli anında bildirim Hizmetleri (PNS - GCM/APNS/WNS) böylece cihazlara bildirimleri sunabilir. 
2. **Teslim** -bu Mobile Engagement SDK'sı tarafından alınan olarak başarıyla aygıta PNS tarafından sunulan ve onaylanan ileti sayısını belirtir. 
   
   *Teslim edildi nedenlerle basılmış sayısından daha az olan sayısı:*
   
   1. Kullanıcı uygulamayı CİHAZDAN kaldırdı. ancak PNS hakkında size anında PNS göndereceğiz zaman bilmiyor ileti bırakılır.
   2. Cihaz uygulama varsa ancak aygıtları kendilerini uzun bir süre için çevrimdışı, PNS cihaza ileti teslim başarısız olur. 
   3. İleti cihaza teslim ancak uygulama Mobile Engagement SDK iletinin içeriğini tanımıyor, sonra bu iletiyi bırakır. Bu bildirim uygulamadaki özelleştirme SDK ve açılan ileti, biz catch özel durum oluşturursa meydana gelebilir. Bu ayrıca cihaza uygulamanın Mobile Engagement SDK sürümü platformdan gönderilen anında iletme iletisi anlamak mümkün olmayan bir sürümünü kullanıyor ancak bildirim hizmeti platformundan gönderilen sonra uygulamayı yalnızca yükseltildiğinde bu ortaya çıkabilir. **Gelişmiş** sekmesi, kaç tane iletileri bırakılan olmadığını bildirir. 
   4. İOS cihazlarda iletileri bazen ya da cihaz düşük pil gücüyle değilse veya uygulama güç önemli miktarda uzak bildirimler işlerken kullanıp kullanmadığına teslim. Bu, iOS cihazları kısıtlamasıdır.   
3. **Görüntülenen** -bu bir sistem itme/çıkış-in-uygulama bildirimi bildirim merkezinde veya mobil uygulama içinde bir uygulama bildirimi biçiminde cihazdaki uygulama kullanıcı için başarılı bir şekilde gösterilen iletilerinin sayısını belirtir.  **Gelişmiş** sekmesini söyleyin, sistem bildirimleri kaç tane ve uygulama içi bildirimler kaç tane. 
   
   *Görüntülenen nedenlerle (görüntülenecek bekleniyor) teslim edildi sayısından daha az olan Say*
   
   1. Bildirim kampanyası üzerinde bir bitiş tarihi olsaydı sonra bildirim teslim edildi ancak açmak ve uygulama kullanıcıya görüntülemek için zaman gelen, hiçbir zaman gösterilen şekilde süresi mümkündür.   
   2. Bildirim bir uygulama bildirimi uygulamanın uygulama kullanıcı oturum açtığında sonra bildirim yalnızca görüntülenir. Uygulama kullanıcı uygulamayı burada açmadığını durumlarda SDK bildirim teslim ancak uygulama açılıncaya kadar henüz görüntülenen rapor eder. 
   3. Bir uygulama bildirimi bildirim ise ve aynı zamanda bildirim olarak raporlanır sonra belirli bir etkinlik/ekran gösterilecek yapılandırılmış teslim ancak kadar teslim edilmedi kullanıcı belirli bir ekranda uygulama açar. 
4. **Kullanıcı etkileşimleri** -bu uygulama kullanıcı ile etkileşime iletilerinin sayısını belirtir ve eylem yapılan veya Çıkılan iletileri içerecektir. 
   
   * *Uygulama kullanıcı eylemi bir bildirim aşağıdaki yollardan biriyle yapabilirsiniz:*
     
     1. Bildirim bir sistem/çıkış-in-uygulama bildirimi veya uygulama kullanıcı bildirimi tıklattığında salt bildirim olarak gönderilen bir uygulama bildirimi ise.
     2. Bir metin veya web görünümü ya yoklamalar içeren bir uygulama bildirimi bildirim ise, uygulama kullanıcının bildirim eylem düğmesine tıklar.
     3. Web görünümü [Android yalnızca] bir URL'de bulunan uygulama Kullanıcı bildirim web görünümü içeren bir uygulama bildirimi ise
   * *Uygulama kullanıcı aşağıdaki yollardan biriyle bildiriminde çıkabilirsiniz:*
     
     1. Bildirim Kapat düğmesini doğrudan tıklatarak. 
     2. Hemen geçirmeyi veya bildirim siliniyor. 
     3. Metin/web içeriği ve anketler ile uygulama bildirimleri normalde uygulama kullanıcı için iki adımlı bir işlem görüntülenir. Bir bildirim ilk göreceği ve bunlar üzerinde tıkladığınızda, sonraki metin/web/yoklama içeriğin görürler. Uygulama kullanıcı ya da bu adımları bildiriminde çıkabilirsiniz ve Gelişmiş Görünüm ayrıntılar bu yakalar. 
5. **İşleme alınan** -bu uygulamanın kullanıcı tarafından açıkça işleme alınan iletilerin sayısını belirtir. Bu, bildirim gönderilen ileti tarafından kaç uygulama kullanıcılarınızın ilgileniyor söylerken en ilginç sayısıdır. 

> [!NOTE]
> İOS & Windows kullanıcı uygulamanın varsa platformları açın ve bir "Her zaman" Kampanya Kampanya edildi sonra uygulama ve uygulama içi bildirimler dışında her ikisi de aynı anda görüntülenen mümkündür. Bu teslim edildi ' daha yüksek bir görüntülenen sayısı neden olabilir. Kullanıcı etkileşim isterseniz bildirim sonra bile kullanıcı etkileşimleri/Actioned sayısı Eylemler teslim edildi büyük olabilir. 
> 
> 

![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

