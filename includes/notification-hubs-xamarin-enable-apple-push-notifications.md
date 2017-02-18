

Apple Anında İletilen Bildirim Servisi (APNS) aracılığıyla uygulamayı anında iletme bildirimlerine kaydetmek için, Apple’ın geliştirici portalında proje için yeni bir anında iletme sertifikası, Uygulama Kimliği ve sağlama profili oluşturmanız gerekir. Uygulama Kimliği, uygulamanızın anında iletme bildirimleri gönderip almasını sağlayan yapılandırma ayarları içerir. Bu ayarlar Apple Anında İletilen Bildirim Servisi’nde (APNS) anında iletme bildirimleri gönderilip alınırken kimlik doğrulaması yapmak için gereken anında iletme bildirimi sertifikasını içerir. Bu kavramlarla ilgili daha fazla bilgi için resmi [Apple Anında İletilen Bildirim Servisi](http://go.microsoft.com/fwlink/p/?LinkId=272584) belgelerine bakın.

#### <a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Anında iletme sertifikası için Sertifika İmzalama İsteği dosyası oluşturma
Bu adımlar, sertifika imzalama isteği oluştururken size yol gösterir. Bu işlem APNS ile birlikte kullanılacak bir anında iletme sertifikası oluşturmak için kullanılır.

1. Mac’inizde Anahtar Zinciri Erişimi aracını çalıştırın. **Yardımcı Programlar** klasöründen veya başlatma panelindeki **Diğer** klasöründen açılabilir.
2. **Anahtar Zinciri Erişimi**’ne tıklayın, **Sertifika Yardımcısı**’nı genişletin ve **Sertifika Yetkilisinden Sertifika İste...** öğesine tıklayın.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. **Kullanıcı E-posta Adresi**’nizi ve **Ortak Ad**’ı seçin, **Diske kaydedilmiş**’in seçili olduğundan emin olun ve ardından **Devam**’a tıklayın. Gerekmediğinden **CA E-posta Adresi**’ni boş bırakın.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Sertifika İmzalama İsteği (CSR) dosyası için **Farklı Kaydet**’e bir ad yazın, **Nerede**’de konum seçin ve ardından **Kaydet**’e tıklayın.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Böylece CSR dosyası istediğiniz konuma kaydedilir; varsayılan konum Masaüstü’dür. Bu dosya için seçilen konumu unutmayın.

#### <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme
Apple’da uygulamanız için yeni bir Açık Uygulama Kimliği oluşturun ve ayrıca anında iletme bildirimleri için yapılandırın.  

1. Apple Geliştirici Merkezi’ndeki [iOS Sağlama Portalı](http://go.microsoft.com/fwlink/p/?LinkId=272456)’na gidin, Apple kimliğinizle oturum açın, **Tanımlayıcılar**’a, **Uygulama kimlikleri**’ne ve son olarak da **+** işaretine tıklayarak yeni uygulamayı kaydedin.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)
2. Yeni uygulamanız için aşağıdaki üç alanı güncelleştirin ve ardından **Devam**’a tıklayın:
   
   * **Ad**: Uygulamanızı için **Uygulama Kimliği Açıklaması** bölümündeki **Ad** alanına açıklayıcı bir ad yazın.
   * **Paket Tanımlayıcı **: **Açık Uygulama Kimliği** bölümünün altında, **Paket Tanımlayıcı** öğesini `<Organization Identifier>.<Product Name>` biçiminde, [Uygulama Dağıtım Kılavuzu](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8)’nda söz edildiği gibi girin. Bu değer, uygulamanızın XCode, Xamarin veya Cordova projesinde kullanılan değerle aynı olmalıdır.
   * **Anında İletme Bildirimleri**: **Anında İletme Bildirimleri**’ni **Uygulama Hizmetleri** bölümünde denetleyin.
     
     ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
3. Uygulama Kimliğini Onayla ekranında ayarı gözden geçirin ve doğruladıktan sonra **Gönder**’e tıklayın
4. Yeni Uygulama Kimliğini gönderdikten sonra **Kayıt tamamlandı** ekranını görürsünüz. **Bitti**’ye tıklayın.
5. Geliştirici Merkezi’nde, Uygulama Kimlikleri altında henüz yeni oluşturmuş olduğunuz , uygulama kimliğini bulup satırına tıklayın. Uygulama kimliği satırına tıklanması uygulama ayrıntılarını gösterir. Alttaki **Düzenle** düğmesine tıklayın.
6. Ekranın altına gidin ve **Geliştirme bildirimi SSL Sertifikası** altındaki **sertifika Oluştur...** düğmesine tıklayın.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
       This will display the "Add iOS Certificate" assistant.
   
   > [!NOTE]
   > Bu öğretici geliştirme sertifikası kullanır. Aynı işlem üretim sertifika kaydedildiğinde de kullanılır. Bildirimleri gönderirken aynı sertifika türünü kullandığınızdan kesinlikle emin olun.
   > 
   > 
7. **Dosya Seç**’e tıklayın ve anında iletme sertifikanız için CSR’yi kaydettiğiniz konuma göz atın. Ardından **Oluştur**’a tıklayın.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
8. Portal tarafından sertifika oluşturulduktan sonra **İndir** düğmesine tıklayın.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
       This downloads the signing certificate and saves it to your computer in your Downloads folder.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Varsayılan olarak, indirilen geliştirme sertifikası dosyası **aps_development.cer** olarak adlandırılır.
   > 
   > 
9. İndirilen **aps_development.cer** bildirim sertifikasına çift tıklayın. Yeni sertifika Anahtar Zinciri’ne aşağıda gösterildiği gibi yüklenir:
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Sertifikanızdaki ad farklı olabilir, ancak **Apple Geliştirme iOS Bildirim Hizmetleri:** ile önek alacaktır.
   > 
   > 
10. Control tuşunu basılı tutarken, Anahtar Zinciri Erişimi’nde **Sertifikalar** kategorisinde yeni oluşturduğunuz bildirim sertifikasına tıklayın. **Dışarı Aktar**’a tıklayın, dosyaya ad verin, **.p12** biçimini seçin ve **Kaydet**’e tıklayın.
    
    Dışarı aktarılan .p12 anında iletme sertifikasının dosya adını ve konumunu unutmayın. Bu konum Klasik Azure Portalı’na yüklenerek APNS kimlik doğrulamasını etkinleştirmek için kullanılacaktır.

#### <a name="create-a-provisioning-profile-for-the-app"></a>Uygulama için bir sağlama profili oluşturun
1. <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Sağlama Portalı</a>’na geri dönün, **Sağlama Profilleri**’ni ve **Tümü**’nü seçin, ardından da yeni profil oluşturmak için **+** düğmesine tıklayın. Böylece, **iOS Hazırlama Profili** Sihirbazı başlatılır
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. **iOS Uygulaması Geliştirme**’yi, **Geliştirme** altında hazırlama profili türü olarak seçin ve **Devam**’a tıklayın.
3. Daha sonra, az önce **Uygulama Kimliği** açılan listesinden oluşturduğunuz uygulama kimliğini seçin ve **Devam**’a tıklayın
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. **Sertifikaları Seç** ekranında, kod imzalama için kullanılan geliştirme sertifikanızı seçin ve **Devam**’a tıklayın. Bu, yeni oluşturduğunuz bildirim sertifikası değil, imzalama sertifikasıdır.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Sonra da, testte kullanmak üzere **Cihazlar**’a ve **Devam**’a tıklayın
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Son olarak, profil için **Profil Adı**’nda bir ad seçin, **Oluştur**’a tıklayın.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)



<!--HONumber=Feb17_HO1-->


