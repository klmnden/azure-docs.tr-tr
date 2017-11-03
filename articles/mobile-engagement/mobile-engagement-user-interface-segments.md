---
title: "Azure Mobile Engagement kullanıcı arabirimi - parçaları"
description: "Oluşturma ve Azure Mobile Engagement kullanarak kullanım düzenlerini tanımlamak için kullanıcı kesimleri yönetme hakkında bilgi edinin"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 087f4a1fef420abe9669f8dfe2b84c7a847ce263
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Oluşturma ve kullanım düzenlerini tanımlamak için kullanıcı kesimleri yönetme
Bu makalede **KESİMLERİ** sekmesinde **Mobile Engagement** portal. Kullandığınız **Mobile Engagement** izlemek ve mobil uygulamalarınızı yönetmek için portal.

UI kesimleri bölümü farklı bir davranışı ve uygulamadan elde edebilirsiniz ve kesimleri API aracılığıyla da erişebilirsiniz analytics göre kullanıcılarınızın kesimlere üzerinde çalışmanıza olanak sağlar. Segment ilk 24 saat sonra oluşturuldukları ve bunların en son analytics bilgilere göre 24 saatte yeniden hesaplanır. Kesimi hesaplanan sonra her gün bir "günlük geçmişine gün" grafiğini görüntüler.

> [!NOTE]
> Birçok bölümlerini **Mobile Engagement** portal UI içeren **YARDIMINI Göster** düğmesi. Bir bölümü hakkında daha fazla kavramsal bilgi almak için bu düğmesine basın.
> 
> 

## <a name="create-segments"></a>Kesimleri oluşturun
En fazla 10 üzerinde belirli bir dönem yukarı geçmişte 60 güne analytics bölümünden kriterlerine göre bir segment oluşturabilirsiniz. Örneğin, belirli sayfalarını görüntülenemez veya son 10 gün içerisinde, uygulamanızın içinde belirli içerik arama kişilerin dayalı bir segment oluşturabilirsiniz. Bu bilgiler analytics bölümünde kullanılabilir. Bu nedenle, bir segment oluşturmanız ve ardından uygulamaya geri dönün getirmek için bu kullanıcı alt kümesini hedeflemek için bir anında iletme bildirimi ayarlamak için kullanabilirsiniz. 

> [!NOTE]
> Bir segment hesaplanan sonra olamaz düzenlenemez; Bunu yalnızca (kopyalanmış) kopyalanması veya (silinmiş) yok. Aynı uygulamanın (aynı AppID) içinde bir segment kopyalanabilen ve (ile farklı bir AppID) diğer uygulamalara da kopyalanabilir. 

 ![segments1][35] 

## <a name="examples-segments"></a>Örnekler parçaları
 ![segments2][36]

Parçaları, son kullanıcılar, uygulamanızın segmentlere ayırmanıza olanak sağlar.
Kullanıcılarınızın kesimlere bir önemli pazarlama stratejidir. Azure Mobile Engagement geçmiş verileri alın ve özel kesimleri oluşturmanıza olanak sağlar. Bu güçlü bir araç, uygulamanızda müşterilerinizin deneyimi hakkında bilgi edinmek sağlar. Kolayca segmentlerinizi çözümlemek ve segmentlerinizi itme hedefleri olarak kullanın.
Bir ortak kullanın-push kullanıcılarınıza deposu uygulamanızda oranı teşvik eden bir bildirim göndermek istediğiniz durumdur. Tüm son kullanıcılarınız için bir bildirim göndermek yerine, uygulamanızı her gün için son bir ayda kullandınız ve iyi kullanıcı deneyimini beklendiğinden yalnızca kullanıcılar belirtirsiniz bir segment oluşturabilirsiniz. Daha az, hedeflenmiş anında iletme bildirimleri göndermek için daha iyi ROI alıyorum.

 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Oluşturabileceğiniz kesimleri önemli Azure Mobile Engagement öğelere dayanmaktadır:
* Olay: hedefleri, bir belirli olay katından fazla bir hafta gerçekleşen uygulamasının bir segment oluşturun. 
* Oturumu: 5 defadan fazla geçen hafta uygulama kullanan kullanıcı kesimini oluşturun.
* Etkinliği: bir sayfa veya içeriği 10 saat geçen ay sayısından fazla veya az kullanılan kullanıcılar bir parçasını oluşturun.
* Proje: bir iş birden fazla günde tamamladınız kullanıcılar bir parçasını oluşturur.
* Kilitlenme: çökmeyle 10 kereden fazla geçen hafta olan tüm kullanıcıların bir segment oluşturun. (Bu bir apology veya hatta kupon kesimle itme!)
* Hata: bir hata 100 kez birden fazla son 3 gün beklendiğinden tüm kullanıcıların bir segment oluşturun.
* Uygulama bilgileri: son 25 gün içinde gerçekleşen özel bir uygulama bilgisi hedefleyen bir segment oluşturun.
  
  ![segments4][38]

Segment bir en iyi şekilde kullanmak için özelleştirilmiş bir tümleştirme SDK'ın "Uygulama bilgisini" etiketlerin etiketleme bir plan ile uygulamanızda yapmış olmanız gerekir.
Daha sonra arabirim giriş sayfasına gidin, istediğiniz ve "Kesimleri" bölümünü tıklatın uygulamayı seçin.

1. "Kesimleri" bölümü seçin.
2. "Yeni kesim" tıklayarak yeni bir segment oluşturmak için düğmesini.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Gerçek yaşam örnek: "Oturum" bilgilerine dayalı basit bir segment oluşturma
Uygulamanızı en az 50 kez geçen hafta içinde kullanmış son kullanıcılar bir parçasını oluşturun. Buradan, en az 30 saniye uygulamanızda oturum başına harcanan son kullanıcılar bulun. Bu, uygulamanızda olumlu bir deneyim olan tüm son kullanıcılar gösterir. Sonra oluşturulan segment deposundaki uygulamanızı değerlendirmelerini isteyin için bu son kullanıcılara bildirim göndermek için kullanılabilir.

 ![segments5][39]

1. Segmentinize "Segment" listesinde hızla bulmak için bir ad verin.
2. "Oluştur" düğmesine tıklayın.
   
   ![segments6][40]

Oturumu seçin.

 ![segments7][41]

1. "Geçen hafta" süreyi seçin.
2. İleri'ye tıklayın.
   
   ![segments8][42]
3. Listenin arasında ilgili işleç seçin: =; ≥, ≤.
4. İstediğiniz sayısı girin.
5. İstediğiniz olayı seçin. 
6. İleri'ye tıklayın.
   Bu örnek, geçen hafta içinde eşleşecek şekilde ayarlanmış en az 50 oturumları yapmış kullanıcılar.
   
   ![segments9][43]

Oturum kesimlemesi için ölçüt olarak oturumu başına uzunluğu seçebilirsiniz.

1. İşleç listeden seçin.
2. Oturum başına uzunluğu belirtin.
3. İleri'ye tıklayın.
   Bu örnekte, yazacaktır tüm oturumları üzerinden, geçişi bölümüne bölümlenmiş, 30 saniyeden oturum başına harcanan kullanıcılar'ı seçin.
   
   ![segments10][44]

Ölçütünüzle tam Huni içinde almak için ad ve Son'u tıklatın.

 ![segments11][45]

Ölçütünüzle ayar tamamlandığında segment Huni içinde görüntülenir.
Bir segment çözümleme verilerini dayandığından, kesimleri günde bir kez hesaplanır.
Bu örnekte, %47,7 toplam son kullanıcılar, ölçütle eşleşen. İyi bir deneyim olmuş ve bunları mağazada uygulama değerlendirmek için bunları isteyen bir bildirim gönderme varsa daha yüksek bir derecelendirme sağlamak büyük olasılıkla kullanıcılar olmaları.

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

