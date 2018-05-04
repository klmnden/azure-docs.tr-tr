

1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. Sonra, sağlanan yönergeleri izleyin.

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Proje ayarlarınızda **Genel** sekmesini seçin. Sonra, Sunucu API anahtarını ve İstemci Kimliğini içeren **google-services.json** dosyasını indirin.
5. Proje ayarlarınızda **Cloud Messaging** sekmesini seçin ve **Eski sunucu anahtarı**’nın değerini kopyalayın. Bu değeri bildirim merkezi erişim ilkesini yapılandırmak için kullanırsınız.
