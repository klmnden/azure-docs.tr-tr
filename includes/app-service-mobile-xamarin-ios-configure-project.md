---
ms.openlocfilehash: a69df0cc9ea14a2c9fa172c77663afb1d6861f9b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097550"
---
#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Xamarin Studio'da iOS projesi yapılandırma
1. Xamarin.Studio içinde açın **Info.plist**ve güncelleştirme **paket grubu tanımlayıcısı** yeni uygulama kimliğiniz ile daha önce oluşturduğunuz paket kimliği ile

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Ekranı aşağı kaydırarak **arka plan modları**. Seçin **arka plan modlarını etkinleştir** kutusu ve **uzak bildirimler** kutusu.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Açmak için çözüm panelinde projenizin çift **proje seçenekleri**.
4. Altında **derleme**, seçin **iOS paket grubu imzalama**, karşılık gelen bir kimliği'ni seçin ve sağlama profili, ayarladığınız yukarı bu proje için.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Bu, kod imzalama için projeyi yeni profili kullanmasını sağlar. Belge resmi Xamarin cihaz sağlama için bkz: [Xamarin cihaz sağlama].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Visual Studio'da iOS projesi yapılandırma
1. Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**.
2. Özellikler Sayfaları'nda tıklatın **iOS uygulama** sekme ve güncelleştirme **tanımlayıcı** daha önce oluşturduğunuz Kimliğine sahip.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. İçinde **iOS paket grubu imzalama** sekmesinde karşılık gelen bir kimliği'ni seçin ve sağlama profili yeni ayarladığınız bu proje için.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Bu, kod imzalama için projeyi yeni profili kullanmasını sağlar. Belge resmi Xamarin cihaz sağlama için bkz: [Xamarin cihaz sağlama].
4. Info.plist açın ve ardından etkinleştirmek için çift **RemoteNotifications** altında **arka plan modları**.

[Xamarin cihaz sağlama]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/