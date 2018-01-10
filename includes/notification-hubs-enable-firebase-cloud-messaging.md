

1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projeniz oluşturulduktan sonra **Firebase'i Android uygulamanıza ekleyin**'e tıklayın ve verilen talimatları uygulayın.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Firebase konsolunda projenize ait dişli çark simgesine ve ardından **Proje Ayarları**'na tıklayın.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Tıklatın **genel** proje ayarlarınızı sekmesini tıklatın ve ardından indirme **google services.json** Server API anahtarı ve istemci kimliğini içeren dosya
5. Tıklatın **Cloud Messaging** sekmesi, proje ayarlarınızı ve değerini kopyalayın **eski sunucu anahtarı**. Bu değer, bildirim hub'ı erişim ilkesini yapılandırmak için kullanılır.
