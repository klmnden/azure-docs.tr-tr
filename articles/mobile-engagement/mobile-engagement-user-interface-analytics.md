---
title: Azure Mobile Engagement kullanıcı arabirimi - Analytics'i
description: Azure Mobile Engagement kullanarak uygulamanız hakkındaki geçmiş verilerini çözümlemeyi öğrenin
services: mobile-engagement
documentationcenter: ''
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 6b2533ac-b8ec-4e35-872c-d563895bdc0c
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fceae1ffff40fc525170121181e21726fe2bd3f7
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-analyze-historical-data-about-your-application"></a>Uygulamanız hakkındaki geçmiş verilerini analiz etme
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu makalede **ANALYTICS** sekmesinde **Mobile Engagement** portal. Kullandığınız **Mobile Engagement** izlemek ve mobil uygulamalarınızı yönetmek için portal. Portalı kullanmaya başlamak için önce oluşturmanız gerektiğini unutmayın bir **Azure Mobile Engagement** hesabı.

UI Analytics bölüm, 24 saatte bir güncelleştirilir geçmiş verilere dayanan uygulamanızı ilgili olarak toplanan bilgiler sağlar. Bilgi çubuğu/satır/pasta grafikler, kılavuzları ve haritalar oluşan farklı panolarda görüntülenir. Verileri de .csv dosyaları olarak indirilebilir. Bu aynı bilgilerin çoğunu, gerçek zamanlı izleme bölümünde UI kullanılabilir ve ayrıca Analytics API'SİNDEN erişilebilir.

> [!NOTE]
> Birçok bölümlerini **Mobile Engagement** portal UI içeren **YARDIMINI Göster** düğmesi. Bir bölümü hakkında daha fazla kavramsal bilgi almak için bu düğmesine basın.

## <a name="standard-and-custom-analytics"></a>Standart ve özel analizi
Azure Mobile Engagement SDK'sı ile uygulamanızı tümleştirmek hemen çizilebilir, uygulamalar ile ilgili temel, standart analitik bilgileri kümesi sağlar. Azure Mobile Engagement Ayrıca, son kullanıcılarınızın davranışları hakkında istediğiniz ek özel analiz bilgileri toplayın olanağı sağlar. Bir etiket planı "oluşturulan özel etiketler (uygulama bilgisi)", oluşturarak bunu yapabilirsiniz **ayarları** böylece Azure Mobile Engagement, bu ek verileri toplayın.

## <a name="analytics"></a>Analiz
* Pano: yeni ve Etkinleri kullanıcılarınıza ve onların eğilimleri hakkında genel bilgileri gösterir.
* Kullanıcılar: Kullanıcılar, cihaz tanımlayıcılarına göre tanımlanır: Bu tanımlayıcı her cihaz için benzersizdir (yeni bir kullanıcı olduğundan gerçekte bir yeni aygıt). İlk oturumunu bu zaman aralığında gerçekleştirdiyse bir kullanıcı yeni bir belirli bir zaman aralığında kabul edilir. Kullanıcı son 7 günde en az bir oturum gerçekleştirdiyse elde tutulmuş kabul edilir. Etkin kullanıcılar en az bir oturum belirli bir süre boyunca yapılan kullanıcılardır. Göre aylık, haftalık, günlük veya saatlik zaman dönemi sıralama yapabilirsiniz. Tüm grafik şuna benzer ancak, uygulamanızın sürümü gibi farklı özelliklere göre filtre ve ardından bir süre Sırala izin verin. SDK tümleştirme tarafından toplanan standart bilgiler aşağıdakileri içerir: etkin kullanıcılar, yeni bir kullanıcı, oturum sayısı, her oturum, ülke, yerel öğeler, konum, dil taşıyıcı, aygıtlar, bellenimi hakkında teknik bilgi uzunluğu ağ (WIFI) , uygulama ve müşterileri tarafından kullanılan SDK sürümleri. Bu bilgi, gerçek zamanlı izleme bölümünden görüntülenebilir.

> [!NOTE]
> Yanlış ayarlanmış tarih, telefon sahip bir kullanıcı yanlış bir zaman diliminde gösterebilirsiniz şekilde kullanıcıların cihaz ayarlarını tarihinden süre dayanır.

* Bekletme: Bir kullanıcı ilk oturumunu bu zaman aralığında gerçekleştirdiyse bir belirli bir zaman aralığında elde tutulmuş kabul edilir. Elde tutulmuş kullanıcıların (ve yeni kullanıcıların) sayıldığı zaman aralıklarını saat, gün, hafta veya ay olarak değiştirebilirsiniz. Kullanıcı bekletme analytics cohorts üzerinde oluşturulmuştur. Belirli bir dönemdeki algılanan tüm yeni kullanıcıları içeren bir kohort kümesidir (yani, bu süre boyunca ilk oturumuna gerçekleştiren kullanıcı kümesini). 1 gün, 2 gün, 4 gün, 7 gün veya 1 aylık cohorts kullanırız. Bir kohort verildiğinde, her 1 gün, 2-gün, 4 gün, 7 gün veya 1 aylık, Azure Mobile Engagement kohort ve misiniz ait tüm kullanıcılar kümesi hesaplar hala etkin (yani, en az bir oturum boyunca kullanıcılar kümesi). Bu kullanıcı kümesi kohort sürüm adı verilir. (Azure Mobile Engagement, kullanıcılarınızın kaç uygulamanızı kullanmaya devam gösterebilir, ancak yalnızca platform belirli deposu kullanıcılarınızın kaç Windows mağazası, app - Örneğin, GooglePlay, iTunes kaldırılması anlayabilirsiniz vs.).
* Oturumlar: Tek bir kullanıcı tarafından uygulama kullanımı. Oturumlar, kullanıcılar tarafından gerçekleştirilen etkinlikleri serisi oluşturulur (bir etkinlik genellikle uygulamanın bir ekran kullanımını ilişkilidir, ancak bu SDK uygulamada bütünleştirilmiştir şekilde bağlı olarak değişebilir). Bir kullanıcı aynı anda yalnızca bir etkinlik gerçekleştirebilirsiniz: kullanıcı ilk etkinliğini başlar ve kendisine son etkinliğini bitirdiğinde durur hemen bir oturumu başlatır. Bir kullanıcı birden fazla birkaç saniye herhangi bir etkinlik yapmadan kalırsa, kendi etkinlik dizisini iki ayrı oturumlara ayrılır.
* Etkinlikler: Her ekranda, uygulamanızdaki her ekran adlarını ve kullanıcılar uzunluğu ayırın. Etkinlikler kendi uygulamanız için ayarladığınız "uygulama bilgisini" etiketleri karşılık gelir özel bir analitik seçeneği şunlardır:
* Kullanıcı yolu: kullanıcılarınızın uygulama etkinlikleri (ekranları) içinde nasıl gezindiğini gösterir. Ayrıntı düzeyini ayarlamak için kaydırıcıyı hareket ettirebilirsiniz. Mavi düğümler uygulamanızın etkinliklerini ifade eder. Kendi boyutu süresi orantılıdır kullanıcıların bunlar içinde geçirdiği. Beyaz düğümler oturum başlangıcını ve bitişini ifade eder. Kırmızı düğümler çökmeleri (crash) ifade eder. Bağlantılar uygulamanızın etkinlikleri arasındaki (veya etkinlikler ile çökmeler arasındaki) geçişleri ifade eder. Bir düğüm veya verileriniz hakkında daha fazla bilgi içeren bir araç ipucunu görüntülemek için bir bağlantıya tıklayarak: belirli bir ekranda, geçiş sayısı ve kaynak etkinliğinden hedef etkinliğine geçiş yüzdesi harcanan zamanı. (Bir---60%---> B A etkinliğindeki kullanıcıların B etkinliğine gittiği anlamına gelir süresi % 60.) Bunu açıklamak istediğiniz kadar grafiği yeniden düzenleyebilirsiniz; Her bir değişiklik yaptığınızda konumu kaydedilir. Grafiği aydınlatmak için çökmeleri (crash) gösterebilir veya gizleyebilirsiniz.
* Olayları: uygulama bir kullanıcı tarafından yapılan belirli eylemler. Olayları dağıtımını kullanıcı oturum başına olay sayısı olarak gösterilir. Bir olay anlık bir eylemi, örneğin, bir düğmeye veya bildirim alma tıklama temsil eder. (Olayların anlamı SDK uygulamada nasıl tümleştirildiğine üzerinde bağlıdır.) Bir olay bir oturum ya da iş sırasında oluşabilir veya tek başına olabilir.
* İşlerini: Eylem uzunluğu odak dışında olaylara benzer. Örneğin, iş yükü veya web hizmetine yapılan bir çağrı içerik süreyi hakkında teknik bilgiler söyleyebilirsiniz. Ayrıca, kullanıcının bir formu doldurun, bir hesap oluşturun veya satın alma süreyi gösterebilirsiniz. Bir işi bir görevin süresini temsil eder, örneğin, bir indirme görevinin süresi veya saati bir başlık ekranda görüntülenir. (İşlerin anlamı, SDK uygulamada nasıl tümleştirildiğine üzerinde bağlıdır.) İşlerini genellikle (yani, herhangi bir kullanıcı etkinliği olmadan) bir oturumun kapsamı dışında gerçekleştirilen arka plan görevleri ile ilişkilendirilir.
* Technicals: Hakkında teknik bilgiler aygıtları izlemek, yerel ayar, taşıyıcı, ağ, aygıt, bellenim, gibi ve ekran kullanıcıların aygıtlarından boyutunu ve uygulamanızı sürümü ve uygulamanızda kullanılan SDK sürümü, uygulamanızın kullanıcılarının.
* Hatalar: Uygulama kilitlenme uygulamaya neden olmaz teknik hatalarla ilgili bilgi. Bir hata, bir ağ hatası veya hatalı işleme gibi anlık bir sorunu temsil eder. (Olayların anlamı SDK uygulamada nasıl tümleştirildiğine üzerinde bağlıdır.) Hata bir oturum ya da iş sırasında oluşabilir veya tek başına olabilir.
* Çökme (Crash): Uygulamanızı çökmesine neden hatalar hakkında bilgi. Bir kilitlenme uygulamanın beklenen işlevlerini gerçekleştirmeyi durdurduğu ve durdurulması gerektiği beklenmeyen bir durumdur. Bir çökme genellikle uygulamadaki bir hatadan kaynaklanır.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Bekletme genel bakış erişme
![Analytics3][12]

Bekletme genel bakış birkaç kartları içine ortasında her belirli bir bekletme dönemi için genel bakış gösteren ayrılmıştır. 2 gün Bekletme dönemi örnekte görülür. Diğer kartlar 4 gün ve 7 gün bekletme süreleri gösterir.

## <a name="understanding-the-retention-overview-cards"></a>Bekletme genel bakış kartları anlama
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Her bir kart 3 ana bölümden oluşur:
1. 1: kohort ve süresi kabul
2. 2-4: Geçerli dönem için bekletme
3. 5: bir mini geçmişi

### <a name="here-is-detailed-information-about-each-element"></a>Her öğe hakkında ayrıntılı bilgiler aşağıdadır:
1. Kohort ve süresi: Bu başlık kohort türü sağlar. Burada "2 günlük süre" kullanıcı davranış üzerinde 2 gün görüneceğini, 2 gün aşağıdaki bloklarında bağlandıklarında gün ve olup olmadığını 2 döneminde gelen kullanıcılar gelir. Yukarıdaki örnekte kullanıcılar Kasım 22nd ve 21st arasında etkinliğini göz önünde bulundurur.
2. Bu bekletme oranı 21 ve Kasım 22 üzerinden 19 ve Kasım 20 gelen kullanıcılar için sağlar. Burada size yeni kullanıcılar 19 ve 20 arasında olan 3 üzerinden 21st arasındaki 22nd, 1 etkin kullanıcı vardı.
3. Bu görsel göstergesi, grafik temsil yukarıdaki aynı bilgileri sağlar. (Üçüncü dairenin % 33 arasında sayıdır.) Renk ek bilgiler verir: yeşil gösterir bu sayı, önceki hesaplamanın dışında artmaktadır. Sarı azalan kararlı ve kırmızı anlamına gelir.
4. Bu hesaplama için kullanılan değerleri gösterir.
5. Elde tutma değerlerinin geçmişinin bir mini budur. Bu değerlerin nasıl gelişen, geniş bir görünüm için geçmişte görmenize olanak sağlar.

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
