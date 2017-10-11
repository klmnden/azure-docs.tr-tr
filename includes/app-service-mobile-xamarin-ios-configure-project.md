#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Xamarin Studio'da iOS projesi yapılandırın
1. Xamarin.Studio içinde açmak **Info.plist**ve güncelleştirme **paket tanımlayıcısı** yeni uygulama kimliğiniz ile daha önce oluşturduğunuz paket kimliği

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Ekranı aşağı kaydırarak **arka plan modları**. Seçin **arka plan modlarını etkinleştir** kutusu ve **uzak bildirimler** kutusu.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Projenizi açmak için çözüm panelinde çift **proje seçenekleri**.
4. Altında **yapı**, seçin **iOS paket imzalama**, karşılık gelen kimliği seçin ve sağlama profili, ayarladığınız yukarı bu proje için.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Bu kod imzalama için proje yeni profili kullanmasını sağlar. Belge sağlama resmi Xamarin aygıt için bkz: [Xamarin cihaz sağlamayı].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Visual Studio'da iOS projesi yapılandırın
1. Visual Studio'da projeye sağ tıklayın ve ardından **özellikleri**.
2. Özellikler sayfalarında tıklatın **iOS uygulama** sekmesini tıklatın ve güncelleştirme **tanımlayıcısı** daha önce oluşturduğunuz kimliği.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. İçinde **iOS paket imzalama** sekmesinde, karşılık gelen kimliği seçin ve sağlama profili yalnızca ayarladığınız bu proje için.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Bu kod imzalama için proje yeni profili kullanmasını sağlar. Belge sağlama resmi Xamarin aygıt için bkz: [Xamarin cihaz sağlamayı].
4. Dosyayı açın ve ardından etkinleştirmek için Info.plist çift **RemoteNotifications** altında **arka plan modları**.

[Xamarin cihaz sağlamayı]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
