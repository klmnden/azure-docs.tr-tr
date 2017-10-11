---
title: "Azure Mobile Engagement kullanıcı arabirimi - giriş"
description: "Varolan bir uygulama ile Azure Mobile Engagement kullanarak projeleri yönetmeyi öğrenin"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: aff578d2-40f6-43e4-b0ea-7d2674cb28a1
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0f15cb975f57f6f5cab12d5118ff50a6fab14388
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-your-existing-application-and-projects"></a>Var olan uygulama ve projeleri yönetme
Bu makalede **giriş** sayfasında **Mobile Engagement** portal. Kullandığınız **Mobile Engagement** izlemek ve mobil uygulamalarınızı yönetmek için portal. Portalı kullanmaya başlamak için önce oluşturmanız gerektiğini unutmayın bir **Azure Mobile Engagement** hesabı. 

Giriş sayfasına ulaşmak için tıklatın **ev** üzerinde sayfasının sol üst. Tüm seçilen koleksiyonunun bir parçası olan uygulamalar listesini içerir. Bu sayfa yalnızca hızlı bir genel bakış uygulamalarınızın bakın.

Giriş sayfası, hesabınız içinde herhangi bir uygulama içeren tüm projeleriniz de içerir. Herkes bir hesabı oluşturarak UI giriş sayfasına erişebilirsiniz, ancak diğer kullanıcıların özel uygulamalarınızda erişimi için bunları sırayla izin vermek gereken unutmayın **My projeleri**.

Karşılaştırma grafiği seçilen uygulamalar için de görüntüleyebilirsiniz. Veya, bir proje ile seçilen uygulamalar için karşılaştırma grafiği görüntülemek için seçin.

![Home1][0]

## <a name="my-applications"></a>Uygulamalarım
Uygulamalarınızın Hızlı Bakış ayrıntılı Şerit seçenekleri görüntülemek için açmak istiyor musunuz hangi uygulamanın seçmenize olanak sağlar. Uygulamanız en son ziyaret edilen Şerit konumda geri dönmek için uygulamanızın adına tıklayın veya uygulamanızın "Ayarlar" sayfasına doğrudan gitmek için dişli simgesine tıklayın. Arama, filtre veya uygulamaları tablolarda görüntülenen bilgileri sıralayabilirsiniz. Ayrıca, sürükleyin ve sütun başlıklarının sırasını değiştirmek için bırakın.

Bunun yanı sıra, uygulamalarınızı bakış içerir:

* **Yeni kullanıcılar eğilimi**: son iki hafta içindeki yeni kullanıcı evrimi.
* **Etkin kullanıcılar**: son 30 gün içindeki etkin kullanıcı sayısı.
* **Etkin kullanıcılar eğilimi**: son iki hafta içindeki etkin kullanıcı evrimi.
* **Oturumları**: tek bir kullanıcı tarafından gerçekleştirilen uygulama kullanımını oturumdur, saati, kullanıcı durmasına kadar kullanarak kullanıcı başlatır.
* **Oturum eğilimleri**: son iki hafta içindeki oturum evrimi.

Bir uygulama tıkladığınızda, izleme ve kullanıcı Arabirimi aracılığıyla uygulamalarınızı yönetmeye başlayabilirsiniz. Örneğin:    

* [Uygulamanız hakkında gerçek zamanlı verileri izleyin](mobile-engagement-user-interface-monitor.md)
* [Uygulamanız hakkındaki geçmiş verilerini çözümleyin](mobile-engagement-user-interface-analytics.md)
* [Kullanım düzenlerini tanımlamak için kullanıcı kesimleri oluşturun ve yönetin](mobile-engagement-user-interface-segments.md)
* [Anında iletme bildirimleri ile uygulamanızın kullanıcılarına ulaşın](mobile-engagement-user-interface-reach.md)

## <a name="my-projects"></a>My projeleri
Projeleri uygulamalarınızı gruplamak ve diğer kullanıcılara uygulamalarınızı erişmek için izinleri vermek için kullanabilirsiniz. Diğer kullanıcılara e-posta adresi sağlayarak izinlerini verin. **Yeni proje** düğmesi yalnızca "name" ve "Açıklama" Yeni projenizin girerek yeni bir proje oluşturmanıza olanak sağlar. Proje oluşturulduktan sonra ad ve açıklama ürününüzün düzenleyin ve bu projede görmek istediğiniz tüm uygulamaları seçmek için proje adı tıklatabilirsiniz.

![Home6][60]

Roller içerir:

* **Görüntüleyici**: A Görüntüleyicisi, yalnızca bir projeyle ilişkili uygulamaları görüntüleyebilir bir kullanıcıyı bileşenidir. Bir Görüntüleyici analytics erişmek ve veri izlemek ve Reach sonuçlarına bakın. Bir Görüntüleyici herhangi bir bilgiyi değiştirmek veya uygulamaları ya da kullanıcıları yönetme olamaz. Bir Görüntüleyici oluşturulamıyor veya Reach kampanya durumunu değiştirin.
* **Geliştirici**: bir geliştirici olan her şeyi bir Görüntüleyici yapmak yanı sıra uygulamaları yönetmek bir kullanıcı. Bir geliştirici etkinleştirmek ve uygulamaları devre dışı, uygulamalarınızın bilgileri (örneğin, paket ve imza) değiştirin ve Reach kampanyaları oluşturabilen. Bir geliştirici, kullanıcılar yönetemez.
* **Yönetici**: bir yönetici haklarına sahip bir geliştirici yapmak yanı sıra kullanıcıları yönetme her şeyi bir kullanıcı. Yönetici kullanıcılar bir projeye katılmak için davet edebilirsiniz, kullanıcı rolleri değiştirebilirsiniz ve projenin bilgilerini değiştirebilirsiniz. Uygulama düzeyi izinleri "ayarlar" ayarlanmış olabilir.

Bu proje bir parçası olan tüm uygulamaları görüntülemek için bir proje üzerinde'ı tıklatın. Aşağıdaki resimde, seçilen uygulamalar için karşılaştırma grafik gösterir.

![Ev2][3]

## <a name="see-also"></a>Ayrıca bkz.
* [Kavramları][Link 6]
* [Sorun giderme kılavuzu hizmeti][Link 24]

<!--Image references-->
[0]: ./media/mobile-engagement-user-interface-home/home0.png
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[60]: ./media/mobile-engagement-user-interface-home/home6.png
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
