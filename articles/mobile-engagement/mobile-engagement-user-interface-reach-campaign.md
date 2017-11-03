---
title: "Azure Mobile Engagement kullanıcı arabirimi - Reach kampanya"
description: "Laern oluşturmak ve anında iletme bildirimi yönetmek için Azure Mobile Engagement kullanarak kampanyaları nasıl"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fc88db8db11d1ed12fa95c2087c9a32b21bf4de5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Oluşturma ve anında iletme bildirimi kampanyaları yönetme
Kullanıcı arabirimini ulaşma bölümünü bir anında iletme bildirimi göndermek için gereken tüm bilgileri sağlayarak karmaşık bir formülü yeni bir itme kampanya oluşturmak için kullanabilirsiniz. Anında iletme kampanya seçenekleri dört kampanya türlerine bağlı olarak biraz farklılık: Duyurular, anketler, veri iter ve Kutucuklar (yalnızca Windows Phone).

### <a name="option-applies-to"></a>Seçenek için geçerlidir:
* Diller: Tüm (Duyurular, anketler, veri gönderimleri, kutucukları)
* Kampanya: Tüm (Duyurular, anketler, veri gönderimleri, kutucukları)
* Bildirim: Duyurular, anketler
* İçeriği: Her kampanya türü için benzersiz olmalıdır
* İzleyici: Tüm (Duyurular, anketler, veri gönderimleri, kutucukları)
* Zaman çerçevesi: Duyurular, anketler, döşeme
* Sınama: Tüm (Duyurular, anketler, veri gönderimleri, kutucukları)

![Reach Campaign1][20]

## <a name="languages"></a>Diller
Dilleri açılan menüsünde, anında iletme farklı bir sürümünü farklı dillerde kullanacak şekilde ayarlanmış olan cihazlara göndermek için kullanabilirsiniz. Varsayılan olarak, tüm cihazlar aynı itme kullanmak üzere ayarlanmış hangi dilde bağımsız olarak alır. Kullanıcılar için farklı bir dil cihazını itme varsayılan dil sürümünü alır. Anında iletme kampanya seçeneklerinin birçoğu, her seçtiğiniz ek diller için alternatif içerik belirtmenizi sağlar. 

![Reach Campaign2][21]

### <a name="language-differences-apply-to"></a>Dil farklar geçerlidir:
* Diller: Benzersiz dilleri yanı sıra varsayılan dili seçilebilir.
* Kampanya: Aynı tüm diller için
* Bildirim: Varsayılan dil yanı sıra her dil için benzersiz olmalıdır
* İçeriği: Varsayılan dil yanı sıra her dil için benzersiz olmalıdır
* İzleyici: ayrı dil ölçüte göre filtre uygulanabilir
* Zaman çerçevesi: tüm diller için aynı
* Sınama: her dil için aynı anda gönderilebilir.

### <a name="supported-languages"></a>Desteklenen diller:
* Arapça (ar) 
* Bulgarca (bg) 
* Katalanca (ca) 
* Çince (zh) 
* Hırvatça (hr) 
* Czech (cs) 
* Danca (da) 
* Felemenkçe (nl) 
* İngilizce (TR) 
* Fince (fi) 
* Fransızca (fr) 
* Almanca (de) 
* Yunanca (el) 
* İbranice (.) 
* Hintçe (yüksek) 
* Macarca (hu) 
* Endonezya dili (ID) 
* İtalyanca () 
* Japonca (ja) 
* Korece (ko) 
* Letonca (lv) 
* Litvanca (lt) 
* Malay (macrolanguage) (ms) 
* Norveççe Bokmål (nb) 
* Lehçe (pl) 
* Portekizce (pt) 
* Rumence (ro) 
* Rusça (ru) 
* Sırpça (sr) 
* Slovakça (sk) 
* Slovence (sl) 
* İspanyolca (es) 
* İsveç dili (sv) 
* Tagalog dili (tl) 
* Tay dili (th) 
* Türkçe (tr) 
* Ukrayna dili (TR) 
* Vietnam dili (VI) 

## <a name="campaign"></a>Kampanya
Kampanya bölüm adı ve kategoriyi de kampanyanızın itme kampanya İzleyici bölümünü yoksay ve bunun yerine bu kampanyayı Reach API'sini (ve bazı öğeler düşük düzeyde anında API) aracılığıyla göndermek planlıyorsanız olarak ayarlamak için kullanabilirsiniz. Kategoriler denetim uygulama bildirimleri önceden tanımlanmış ayarlarınızı temel alan özel bildirim şablonu ile kullanılabilir. Varolan "Kategoriler" Reach API'sini aracılığıyla listesini elde edebilirsiniz.

> [!WARNING]
> Kampanya otomatik olarak göndermez, Reach kampanya "Kampanyası" bölümünde "Yoksay İzleyici API aracılığıyla kullanıcılara bir gönderim" seçeneğini kullanırsanız, el ile ulaşmak API üzerinden göndermek gerekir.

![Reach Campaign3][22]

### <a name="option-applies-to"></a>Seçenek için geçerlidir:
* Ad: tüm
* Kategori: Duyurular, anketler
* Hedef kitleyi yok sayın anında iletme, API üzerinden kullanıcılara gönderilecek: tüm

## <a name="notification"></a>Bildirim
Bildirim bölümü, anında iletme dahil etmek için temel ayarları ayarlamak için kullanabileceğiniz: gönderme, ileti, bir uygulama görüntüsü başlığı veya atlanabilir tüm ise. Birçok bildirim ayarlarını Cihazınızı platforma özgüdür. "Uygulama" veya "uygulama dışında" bir gönderim seçebilirsiniz veya her ikisini de. (Kullanıcılar can "katılımı" veya "çevirin" dışında "uygulamasının" işletim sistemi cihazlarını düzeyi iter ve Azure Mobile Engagement bu ayarı geçersiz kılmak mümkün olmaz unutmayın. Ayrıca "uygulama" Reach API'sini işler ve "uygulama dışı" iter unutmayın. Anında iletme API "uygulama dışında" iter çok işlemek için kullanılabilir.) İter, resim veya HTML içeriğini, uygulamanızı dışında veya başka bir konuma uygulamanız (Android SDK 2.1.0 veya gerekli sonraki hedefi kategorileri) içinde bağlama için ayrıntılı bağlantılar dahil olmak üzere özelleştirilebilir. Simge veya iOS rozet değiştirmek ve metin veya web içerik (html içerik, URL bağlantı başka bir konum içinde veya dışında uygulama ile popup) gönderin. Ayrıca, Android cihazları çaldır veya itme Titret. (Doğru gerekeceğini unutmayın Android SDK izinleri bildirim halka veya bir cihazı Titret dosyaya.) Var. şu anda hiçbir endüstri Android "büyük resimdeki" boyutları için standart ekran boyutlarına her cihazda farklı ancak neredeyse her ekran boyutuna 400 x 100 resimleri iş beri.

### <a name="delivery-types"></a>Teslim türleri:
* Yalnızca uygulama dışında: kullanıcı uygulamayı kullanmadığında bildirim teslim edilecek.
* Uygulama yalnızca bildirim dışı Apple veya Google (GCM ya da APNS sertifikası) bir sertifika gerektirir.
* Uygulama yalnızca: yalnızca uygulama çalışırken bildirim görüntülenir.
* Bildirim Capptain teslim sistemi kullanıcı erişmek için kullanır. Düzen/görünümünü, anında tam olarak özelleştirebilirsiniz.
* Herhangi bir zaman: Ya da uygulama veya çalıştığı bildirim göndermek bu seçeneği sağlar.

![Reach Campaign4][23]

### <a name="option-applies-to"></a>Seçenek için geçerlidir:
* Bildirim: Duyurular, anketler

## <a name="content"></a>İçerik
İçerik bölümü içeriği Duyurular, anketler, veri iter ve Kutucuklar (yalnızca Windows Phone) değiştirmek için kullanabilirsiniz. İçerik anında iletme kampanyalarını kampanya türüne belirli ayarıdır. 

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - ulaşmak - içerik gönderme][Link 29]

![Reach Campaign5][24]

## <a name="audience"></a>Hedef kitle
Hedef kitle bölümü, standart bir kampanya veya kampanyanızı özelleştirilmiş ölçütlere göre sınırları sınırlamak için öğe listesi tanımlamak için kullanabilirsiniz. Standart kitlenizi sınırlamak için seçenekleri kümesi, yeni veya eski kullanıcılara veya yalnızca yerel gönderim kullanıcılara anında iletme olanak sağlar. Anında iletme alan kullanıcıların sayısını sınırlamak için bir kota de ayarlayabilirsiniz. Hedef kullanıcılar için bir veya daha fazla ölçüt eklemek için kampanyanızı nasıl filtre için ifade el ile düzenleyebilirsiniz. Bir hedef kitle ifadesi el ile yazabilirsiniz. Bu tür bir ifade, ölçütler arasındaki ilişkiyi açıkça tanımlamanız gerekir. Bir ölçüt büyük harfle başlamalı ve boşluk içeremez bir tanımlayıcı tarafından tanımlanır. Ölçütler arasındaki ilişkiyi kullanılarak tanımlanabilir 'and', 'or', 'not' işleçleri yanı sıra '(',')'. Örnek: "Criterion1 veya (Criterion1 ve değil Criterion2)".

> [!NOTE]
> Kampanyalarda bulunan büyük bir izleyici ile tarama hedefleme sunucu tarafı özellikle aynı anda birden çok Kampanyalar başlatmayı denerseniz yavaş olabilir.

* Mümkünse, yalnızca bir kampanya aynı anda başlatın.
* En fazla yalnızca dört Kampanyalar aynı anda başlatın.
* Yalnızca etkin kullanıcılara anında iletme (onay kutusu "yalnızca yerel gönderim kullanılarak ulaşılabilen kullanıcıları devreye" ve "yalnızca etkin kullanıcıları göster") böylece hala uygulamasını yüklemediyseniz ve kullanmak, kullanıcılarınızın taranacak gerekir.
  Kitlenizi tanımlandıktan sonra bu itme kaç kullanıcı alacak bulmak için benzetim düğmesini kullanabilirsiniz. Bu, büyük olasılıkla (Bu kullanıcıların rastgele bir örneği temel alarak bir tahmindir) Bu İzleyici tarafından hedeflenen bilinen kullanıcıların sayısını hesaplayın. Uygulamayı kaldırmış kullanıcıların da bu hedef kitleye dahil olduğu, ancak ulaşılamıyor unutmayın.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - Reach - yeni itme ölçüt][Link 28]

![Reach Campaign6][25]

### <a name="edit-expression"></a>İfade Düzenle
![Reach Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a>Hedef kitle seçeneğinizi uygulandığı sınırı:
* Yalnızca bir kullanıcı alt kümesini göster: tüm (Duyurular, anketler, veri iter, kutucukları)
* Yalnızca eski kullanıcıları göster: tüm (Duyurular, anketler, veri iter, kutucukları)
* Yalnızca yeni kullanıcıları göster: tüm (Duyurular, anketler, veri iter, kutucukları)
* Yalnızca boştaki kullanıcıları göster: Duyurular, anketler, döşeme
* Yalnızca etkin kullanıcıları göster: tüm (Duyurular, anketler, veri iter, kutucukları)
* Yalnızca yerel gönderim kullanılarak ulaşılabilen kullanıcıları devreye sok: Duyurular, anketler

## <a name="time-frame"></a>Zaman çerçevesi
Gönderim veya zaman çerçevesi kampanya hemen başlatmak için boş bırakabilirsiniz ayarlamak için zaman çerçevesi bölüm kullanabilirsiniz. Son kullanıcılar saat dilimi kullanarak kampanya kullanıcılarınızın Asya'da beklediğiniz ve kampanyanızı için zaman çerçevesi dünyadaki tüm saat dilimleri eşleşen kadar bir seferde iter küçük toplu göndermek'den önceki bir gün başlayabilir, unutmayın. İstek süresi telefonunuzdan itme başlatmadan önce olduğundan son kullanıcının saat dilimi kullanarak da gecikmeler kampanyalarda neden olabilir.

> [!NOTE]
> Bitiş tarihi önbelleğe alabilir olmadan kampanyaları iter yerel olarak hala görünen ve bunları sonra el ile tam kampanyalar. Özel bir bitiş saati kampanyalar için bu davranışı önlemek için.

### <a name="see-also"></a>Ayrıca bkz.
* [Ulaşmaya - nasıl yapılır – planlama][Link 3] 

![Reach Campaign8][27]

### <a name="settings-apply-to"></a>Ayarlar için geçerlidir:
* Zaman çerçevesi: Duyurular, anketler, döşeme

## <a name="test"></a>Test etme
Test bölümü kampanya kaydetmeden önce bu itme kendi test aygıta göndermek için kullanabilirsiniz. Tüm özel diller bu kampanya için yapılandırdıysanız, her dilde göndererek test edebilirsiniz. "Hesabım" test aygıttan ayarlayabilirsiniz.

> [!NOTE]
> "Test etmek için" düğmesini kullandığınızda veriler günlüğe kaydedilir hiçbir sunucu tarafı iter, veriler yalnızca gerçek anında iletme kampanyalarını için günlüğe kaydedilir.

### <a name="see-also"></a>Ayrıca bkz.
* [UI belgeleri - hesabım][Link 14]

![Reach Campaign9][28]

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

