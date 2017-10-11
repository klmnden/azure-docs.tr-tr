### <a name="grant-access-to-your-push-certificate-to-mobile-engagement"></a>Anında İletme Sertifikanıza Mobile Engagement için erişim izni verme
Mobile Engagement’ın sizin adınıza Anında İletme Bildirimleri göndermesine izin vermek için sertifikanıza erişim izni vermeniz gerekir. Bu da, sertifikanız yapılandırıp Mobile Engagement portalına girilerek gerçekleştirilir. [Apple belgeleri](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)’nde açıklandığı gibi p12 sertifikanız olduğundan emin olun

1. Mobile Engagement portalınıza gidin. Doğru yerde olduğunuzdan emin olun ve alttaki **Katıl** düğmesine tıklayın:
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. Engagement Portal’ınızdaki **Ayarlar** sayfasına tıklayın. Buradan, p12 sertifikanızı yüklemek için **Yerel Gönderim** bölümüne tıklayın:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. p12 sertifikanızı seçin, bunu yükleyip parolanızı yazın:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Uygulamanıza bildirim gönderme
Şimdi de, uygulamamıza Anında İletme Bildirimi gönderecek basit bir bildirim kampanyası oluşturacağız:

1. Mobile Engagement portalınızın **erişim** sekmesine gidin.
2. Bildirim kampanyanızı oluşturmak için **Yeni Duyuru**’ya tıklayın
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. Kampanyanızın ilk alanlarını ayarlayın:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * Kampanyanıza bir **Ad** verin 
   * **Teslim zamanını**, **Yalnızca uygulama dışında** olarak seçin: Bu, bazı metinleri oluşturan basit Apple anında iletilen bildirim türüdür.
   * Bildirim metninde önce ilk satır olacak **Başlık** metnini yazın.
   * Sonra da ikinci satır olacak **İleti**’yi yazın
4. Kaydırarak aşağı gidin ve içerik bölümünde **Yalnızca bildirim**’i seçin
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. En temel kampanya ayarını bitirdiniz. Şimdi aşağıya kayın ve anında iletme bildirimi kampanyanızı kaydetmek için **Oluştur** düğmesine tıklayın. 
6. Son olarak, anında iletme bildirimini göndermek için **Etkinleştir**’e tıklayın. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Bildirim merkezinde, iOS cihazınızdaki bildirimi aşağıdaki gibi alabilirsiniz:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Bu iOS cihazıyla eşleştirilmiş bir Apple Watch’unuz varsa, Apple Watch’unuzda bildirimi görürsünüz:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

