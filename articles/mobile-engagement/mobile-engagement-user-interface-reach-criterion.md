---
title: Azure Mobile Engagement kullanıcı arabirimi - ulaşma ölçütü
description: Azure Mobile Engagement kullanarak kullanıcılarınız için bir select alt anında iletme kampanyalarını göndermek için hedefleme ölçütleri kullanmayı öğrenin
services: mobile-engagement
documentationcenter: ''
author: piyushjo
manager: erikre
editor: ''
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 2adf473c6acea0f128eb14e2616748ff29d5d762
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Kullanıcılarınız için bir select alt anında iletme kampanyalarını göndermek için hedefleme ölçütleri kullanma
> [!IMPORTANT]
> Azure Mobile Engagement 31.03.2018 tarihinde kullanımdan kaldırılıyor. Bu sayfa, kısa bir süre sonra silinecek.
> 

"Yeni ölçüt" düğmesiyle belirli ölçütlere göre kitlenizi hedefleme biridir anında iletme bildirimleri ilgili gönderdiğiniz yardımcı Azure Mobile Engagement en güçlü açıklanan kavramlar herkes istenmeyen posta yerine müşteriler için yanıtlar. Standart ölçütlere göre kitlenizi sınırlamak ve kaç kişinin bildirim alırsınız belirlemek için gönderim benzetimini yapma.

**Ayrıca bkz.:**

* [UI belgeleri - Reach - yeni itme kampanya][Link 27]

## <a name="audience-criteria-can-include"></a>Hedef kitle ölçüt içerebilir:
* ** Technicals: ** hedeflemek analizler ve İzleyici bölümlerde görebilirsiniz aynı teknik bilgileri temel alarak. **Ayrıca bkz:** [UI belgeleri - Analytics'i][Link 15], [UI belgeleri - izleme][Link 16]
* **Konumu:** "gerçek zamanlı konum Raporlama" ile Coğrafı kullanan uygulamalar coğrafi konum bir ölçüt olarak GPS konumdan bir hedef kitleye seslenmek için kullanabilirsiniz. "Yavaş alan konumu Raporlama" çağrısı da cep telefonu konumdan bir hedef kitleye seslenmek için kullanılabilir ("gerçek zamanlı konum Raporlama" ve "Yavaş alan konumu Raporlama" etkinleştirilmesi gerekir SDK'dan). **Ayrıca bkz:** [SDK Belgeleri - iOS - tümleştirme][Link 5], [SDK Belgeleri - Android - tümleştirme][Link 5]
* **Reach geri bildirim:** kitlenizi ulaşma görüşleri Duyurular, anketler ve veri iter aracılığıyla önceki ulaşma bildirimleri kendi görüşleri göre hedefleyebilirsiniz. Bu, daha iyi hedef kitlenizi iki hafta sonra sağlar veya ilk kez oranla üç Kampanyalar ulaşmak. Zaten bir kampanya zaten belirli bir önceki kampanya almış kullanıcılara gönderilmemesi ayarlayarak benzer içeriği içeren bir bildirim alındı kullanıcıları filtrelemek için de kullanılabilir. Kullanıcılar bile dışlayabilirsiniz kimin yeni iter almasını hala etkin olan belirli bir kampanyaya eklenir. **Ayrıca bkz:** [UI belgeleri - Reach - anında içeriği][Link 29]
* **Yükleme izlemesi:** bilgileri kullanıcılarınızın uygulamanızın yüklendiği göre izleyebilirsiniz. **Ayrıca bkz:** [UI belgeleri - ayarlar][Link 20]
* **Kullanıcı profili:** , standart kullanıcı bilgilerine göre hedef olabilir ve oluşturduğunuz özel uygulama bilgisi tabanlı hedef olabilir. Bu, oturum açmış ve bunları yalnızca nasıl bunlar önceki kampanyaya yerine uygulama ayarlamaya istediniz belirli sorulara verdiğiniz yanıtlara kullanıcılar içerir. Tüm uygulama bilgilerini 's bu listede, uygulama gösteri için tanımlanmış.
* Segment: Şunları da yapabilirsiniz oluşturduğunuz kesimlerinde göre hedef tabanlı birden çok ölçüt içeren belirli bir kullanıcı davranışı. Uygulamanız için tanımlanan segmentlerinizi tümünün bu listede gösterilir. **Ayrıca bkz:** [UI belgeleri - parçaları][Link 18]
* **Uygulama bilgileri:** özel uygulama bilgisi etiketleri "Kullanıcı davranışı izlemek için ayarları" oluşturulabilir. **Ayrıca bkz:** [UI belgeleri - ayarlar][Link 20]

## <a name="example"></a>Örnek:
Yalnızca bir uygulama içi satın alma eylem gerçekleştirmiş kullanıcılarınızın alt kümesi için bir duyuru itmek istiyorsanız.

1. Uygulama Ayarları sayfasına gidin, "Uygulama bilgisini" menüsünü seçin ve "Yeni uygulama bilgileri" seçeneğini belirleyin
2. "İnAppPurchase" adlı yeni bir mantıksal uygulama bilgisi kaydetme
3. "Kullanıcı bir uygulama içi satın alma başarıyla gerçekleştirdiğinde, true olarak" Bu uygulama bilgisi ayarlamak, uygulamanızın yapın (sendAppInfo ("inAppPurchase",...) kullanarak işlevi)
4. Bunu uygulamanızdan yapmak istemiyorsanız, cihaz API'sini kullanarak arka ucunuzdan yapabilirsiniz)
5. Ardından, "kümesine inAppPurchase" olan kullanıcılar hedef kitleye sınırlama bir ölçüte göre bir duyuru oluşturmanız yeterlidir "true")

> [!NOTE]
> Uygulama bilgisi etiketleri başka ölçütlere göre hedefleme itme gönderilmeden önce ve bir gecikmeye neden olabilir, kullanıcıların cihazlarından bilgileri toplamak Azure Mobile Engagement gerektirir. Karmaşık gönderim yapılandırması (rozetleri güncelleştirme gibi) seçenekler de geciktirebilir iter. Mutlak hızlı itme yönteminde Azure Mobile Engagement bir "tek seferde" kampanyası itme API'sinden kullanmaktır. Uygulama bilgileri etiketleri sunucu tarafında depolandığından Reach Kampanya (ya da Reach API'sini veya kullanıcı arabirimini) için anında iletme ölçüt olarak yalnızca uygulama bilgisi etiketleri kullanarak sonraki en hızlı yöntemdir. Kampanya göndermek için aygıtlarını sorgulamak Azure Mobile Engagement sahip olduğundan hedefleme diğer ölçütleri itme kampanya için en esnek ancak yavaş anında yöntemini kullanmaktır.

![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Ölçüt seçenekleri için geçerlidir:
* **Technicals**     
* Bellenim name: ürün bilgisi adı
* Bellenim sürümü: bellenim sürümü
* Cihaz modeli: cihaz modeli
* Aygıt üreticisi: aygıt üreticisi
* Uygulama sürümü: uygulama sürümü
* Taşıyıcı Adı: tanımsız taşıyıcı adı
* Taşıyıcı Ülke: taşıyıcı ülke, tanımlanmamış
* Ağ türü: ağ türü
* Yerel ayar: yerel ayar
* Ekran boyutu: ekran boyutu
* **Konum**      
* Alan bilinen son: ülke, bölge, Yerleşim Yeri
* Gerçek zamanlı coğrafi yalıtma: POIs listesi (adı, Eylemler), döngüsel POI (adı, enlem, boylam, RADIUS metre içinde)
* **Geri bildirim ulaşmak**     
* Duyuru geri bildirim: duyuru, geri bildirim
* Yoklama geri bildirim: yoklama, geri bildirim
* Yoklama yanıt geri bildirim: yoklama yanıt geri bildirim, soru, seçim
* Verileri gönderme geri bildirim: veri gönderimi, geri bildirim
* **Yükleme izlemesi**     
* Depo: Deposu, tanımlanmamış
* Kaynak: Kaynak, tanımlanmamış
* **Kullanıcı profili**     
* Cinsiyetiniz: Erkek veya kadın, tanımlanmamış
* Doğum Tarihi: işleci, undefined tarihi
* Katılımı: true veya false, tanımlanmamış
* **Uygulama bilgileri**      
* Dizesi: tanımsız dize
* Tarihi: işleci, undefined tarihi
* Tamsayı: işleç, sayı, tanımlanmamış
* Boole değeri: true veya false değerini, tanımlanmamış
* **Segment**    
* Segment adını (aşağı açılan listeden), dışlama (Bu segmentin parçası olmayan kullanıcıları hedefleyin).

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

