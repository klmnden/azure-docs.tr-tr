
1. [Google Cloud Console](https://console.developers.google.com/project)’a gidin, Google hesabı kimlik bilgilerinizle giriş yapın. 
2. **Proje Oluştur**’a tıklayıp bir proje adı yazın ve sonra **Oluştur**’a tıklayın. İstendiyse, SMS Doğrulamasını uygulayın ve **Oluştur**’a yeniden tıklayın.
   
    ![Yeni proje oluşturma](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Yeni **Proje adı**’nızı yazıp **Proje oluştur**’a tıklayın.
3. **Yardımcı Programlar ve Diğerleri** düğmesine ve ardından **Proje Bilgileri**’ne tıklayın. **Proje Numarası**’nı not alın. Bu değeri, istemci uygulamasında `SenderId` değişkeni olarak ayarlamalısınız.
   
    ![Yardımcı programlar ve daha fazlası](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. Proje panosunun **Mobil API'ler** altında **Google Cloud Messaging**’e tıklayın, sonraki sayfada **API etkinleştir**’e tıklayıp hizmet koşullarını kabul edin. 
   
    ![GCM etkinleştirme](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![GCM etkinleştirme](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Proje panosunda, **Kimlik Bilgileri** > **Kimlik Bilgisi Oluştur** > **API Anahtarı**’na tıklayın. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. **Yeni anahtar oluştur**’da **Sunucu anahtarı**’na tıklayın, anahtarınız için bir ad yazın ve **Oluştur**’a tıklayın.
7. **API ANAHTARI** değerini not edin.
   
    Azure’un GCM ile kimlik doğrulaması yapmasını ve uygulamanız adına anında iletme bildirimleri göndermesini etkinleştirmek için bu API anahtarını kullanın.

