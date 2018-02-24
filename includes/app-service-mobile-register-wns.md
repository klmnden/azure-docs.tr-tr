
1. Visual Studio Çözüm Gezgini'nde, Windows mağazası uygulama projesine sağ tıklayın. Ardından **deposu** > **uygulamayı mağaza ile ilişkilendir**.

    ![Uygulamayı Windows mağazası ile ilişkilendirme](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Sihirbazı'nda seçin **sonraki**. Ardından Microsoft hesabınızla oturum açın. İçinde **yeni bir uygulama adı yedek**, uygulamanız için bir ad yazın ve ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni uygulama adını seçin. Seçin **sonraki**ve ardından **ilişkilendirmek**. Bu işlem, uygulama bildirimine gerekli Windows mağazası kayıt bilgilerini ekler.
4. Adım 1 ve 3, daha önce oluşturduğunuz aynı kayıt için Windows mağazası uygulaması kullanarak Windows Phone mağazası uygulama projesi için tekrarlayın.  
5. Git [Windows Geliştirme Merkezi](https://dev.windows.com/en-us/overview)ve ardından Microsoft hesabınızla oturum açın. İçinde **uygulamalarım**, yeni uygulama kaydı'nı seçin. Ardından **Hizmetleri** > **anında iletme bildirimleri**.
6. Üzerinde **anında iletme bildirimleri** sayfasında **Windows anında bildirim Hizmetleri (WNS) ve Microsoft Azure Mobile Apps**seçin **Live Services sitesi**.  Değerlerini Not **paket SID'si** ve *geçerli* değeri **uygulama gizli anahtarı**. 

    ![Geliştirici Merkezi'nde uygulama ayarı](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Yok Bu değerleri kimseyle paylaşmayın veya uygulamanızla dağıtın.
   >
   >
