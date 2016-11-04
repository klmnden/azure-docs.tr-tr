### GCM API Anahtarınıza Mobile Engagement erişimi verin
Mobile Engagement’ın sizin adınıza anında iletme bildirimleri göndermesine izin vermek için API Anahtarınıza erişim izni vermeniz gerekir. Bu da, anahtarınızın yapılandırıp Mobile Engagement portalına girilerek gerçekleştirilir.

1. Klasik Azure Portalı’nızdan bu proje için kullanmakta olduğunuz uygulamadan emin olun ve alttaki **Katıl** düğmesine tıklayın:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Sonra da, GCM Anahtarınızı girmek için **Ayarlar** -> **Yerel Gönderim**’e tıklayın:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. **GCM Ayarları** bölümünde, **API Anahtarı**’nın ön tarafındaki **Düzenle** simgesine aşağıda gösterildiği gibi tıklayın:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. Açılan pencerede, daha önce aldığınız GCM Sunucu Anahtarını yapıştırın **Tamam**’a tıklayın.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Uygulamanıza bildirim gönderme
Şimdi de, uygulamamıza anında iletme bildirimi gönderen basit bir anında iletme bildirimi kampanyası oluşturacağız.

1. Mobile Engagement portalınızın **REACH** sekmesine gidin.
2. Anında iletme bildirimi kampanyanızı oluşturmak için **Yeni duyuru**’ya tıklayın.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. Kampanyanızın ilk alanını şu adımlarla ayarlayın:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    a. Kampanyanızı adlandırın.
   
    b. **Teslimat türü**’nü *Sistem bildirimi -> Basit* olarak seçin: Bir başlık ve küçük bir metin satırından oluşan basit Android anında iletme bildirimi türüdür.
   
    c. Uygulama başlatılsın ya da başlatılmasın uygulamanın bildirim almasını sağlamak için **Teslimat zamanı**’nı *Her zaman* olarak seçin.
   
    d. Bildirim metnine gönderimde koyu olacak **Başlık** metnini yazın.
   
    e. Ardından, **İleti**’yi yazın.
4. Kaydırarak aşağı gidin ve **İçerik** bölümünde **Yalnızca bildirim**’i seçin.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Olabilecek en temel kampanya ayarını bitirdiniz. Şimdi kaydırarak yeniden aşağı gidin ve **Oluştur** düğmesine tıklayarak kampanyanızı kaydedin.
6. Son adım: anında iletme bildirimlerini gönderecek kampanyanızı etkinleştirmek için **Etkinleştir**’e tıklayın.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

<!--HONumber=Sep16_HO3-->


