
###API anahtarıyla Google Cloud Messaging projesi oluşturma

>[AZURE.NOTE] Bu yordamı tamamlamak için doğrulanmış e-posta adresine sahip bir Google hesabınız olmalıdır. Yeni bir Google hesabı oluşturmak için <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a> adresine gidin.

1. [Google Cloud Console](https://console.developers.google.com/project)’a gidin ve Google hesabı kimlik bilgilerinizle giriş yapın.

2. **Tüm Projeler**’e gidin ve ardından **Proje Oluştur**’a tıklayın.

3. Bir **Proje adı** girin ve **Oluştur**’a tıklayın

4. Proje oluşturulduktan sonra uzun bir sayısal değer olan **Proje numarasını** not aldığınızdan emin olun. Bunu Projenizin **Ayarlar** menüsündeki **IAM ve Yönetici bölümü** altında bulabilirsiniz; daha sonra bu bilgiye ihtiyacınız olacaktır. 
 
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)

5. Bundan sonra platformumuz tarafından Android cihazlara bildirim göndermek üzere kullanılacak Google Cloud messaging platformu için bir anahtar oluşturulacaktır. **API Yöneticisi** bölümüne gidin ve **Mobil API’leri** altındaki **Google Cloud Messaging** öğesine tıklayın. 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/gcm.png)

6. Sonraki sayfada **Etkinleştir** düğmesine tıklayın. Pano, kimlik bilgilerini oluşturmanızı ister. Bu nedenle **Kimlik Bilgilerine Git** düğmesine tıklayın. 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

6. İlk açılan kutuda **Google Cloud Messaging**, ikinci açılan kutuda **Web sunucusu**’nu seçin ve ardından **Bana hangi kimlik bilgileri gerekiyor?** öğesine tıklayın

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

7. **Projenize kimlik bilgileri ekleyin** sayfasında **API anahtarı oluştur**’a tıklayın.

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)

8. **API ANAHTARI** değerini not edin. Bu API anahtarı değerini daha sonra "Yerel Görderim" bölümünde yapılandırmak amacıyla kullanacaksınız. Şimdi **Bitti**’ye tıklayın.



<!--HONumber=Aug16_HO1-->


