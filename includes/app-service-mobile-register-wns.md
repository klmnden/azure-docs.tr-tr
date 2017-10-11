
1. Visual Studio Çözüm Gezgini'nde, Windows mağazası uygulama projesine sağ tıklayın ve **deposu** > **uygulamayı mağaza ile ilişkilendir**.

    ![Uygulamayı Windows mağazası ile ilişkilendirme](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Sihirbazı'nda tıklatın **sonraki**ve Microsoft hesabınızla oturum açın. Uygulamanız için bir ad yazın **yeni bir uygulama adı yedek**ve ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni uygulama adı seçin, **sonraki**ve ardından **ilişkilendirmek**. Bu, uygulama bildirimine gerekli Windows Mağazası kayıt bilgilerini ekler.
4. Adım 1 ve 3, daha önce oluşturduğunuz aynı kayıt için Windows mağazası uygulaması kullanarak Windows Phone mağazası uygulama projesi için tekrarlayın.  
5. Gözat [Windows Geliştirme Merkezi](https://dev.windows.com/en-us/overview)ve Microsoft hesabınızla oturum açın. Yeni uygulama kaydında tıklatın **uygulamalarım**, genişletin ve ardından **Hizmetleri** > **anında iletme bildirimleri**.
6. Üzerinde **anında iletme bildirimleri** sayfasında, **Live Services sitesi** altında **Windows anında bildirim Hizmetleri (WNS) ve Microsoft Azure Mobile Apps**. Değerlerini Not **paket SID'si** ve *geçerli* değeri **uygulama gizli anahtarı**. 

    ![Geliştirici Merkezi'nde uygulama ayarı](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.
   >
   >
