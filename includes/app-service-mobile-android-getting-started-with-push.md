1. İçinde **uygulama** projesi, dosyayı açma `AndroidManifest.xml`. Sonraki iki adımda kodla  *`**my_app_package**`*  projeniz için uygulama paketi adıyla. Bu değeri `package` özniteliği `manifest` etiketi.
2. Varolan sonra aşağıdaki yeni izinleri ekleyin `uses-permission` öğe:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Sonra aşağıdaki kodu ekleyin `application` etiketi açma:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Dosyayı açmak *ToDoActivity.java*ve aşağıdaki içeri aktarma deyimini ekleyin:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Aşağıdaki özel değişken sınıfına ekleyin. Değiştir  *`<PROJECT_NUMBER>`*  önceki yordamda ve uygulamanızın Google tarafından atanan proje numarası ile.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Tanımını değiştirin *MobileServiceClient* gelen **özel** için **ortak statik**, şimdi şöyle görünür:

        public static MobileServiceClient mClient;
7. Bildirimleri işlemek için yeni bir sınıf ekleyin. Proje Gezgini'nde Aç **src** > **ana** > **java** düğümler ve paket adı düğümünü sağ tıklatın. Tıklatın **yeni**ve ardından **Java sınıfı**.
8. İçinde **adı**, türü `MyHandler`ve ardından **Tamam**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. MyHandler dosyasında, sınıf bildirimi ile değiştirin:

        public class MyHandler extends NotificationsHandler {
10. İçin aşağıdaki içeri aktarma deyimlerini ekleyin `MyHandler` sınıfı:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Bu üye sonraki eklemek `MyHandler` sınıfı:

        public static final int NOTIFICATION_ID = 1;
12. İçinde `MyHandler` sınıfı, geçersiz kılmak için aşağıdaki kodu ekleyin **onRegistered** Cihazınızı mobil hizmet bildirim hub'ı ile kaydeder yöntemi.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. İçinde `MyHandler` sınıfı, geçersiz kılmak için aşağıdaki kodu ekleyin **onReceive** , alındığında görüntülemek için bildirime neden olan yöntemi.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. Geri TodoActivity.java dosyasında güncelleştirme **onCreate** yöntemi *ToDoActivity* bildirim işleyici sınıfını sınıfı. Sonra bu kodu eklediğinizden emin olun *MobileServiceClient* örneği.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Uygulamanıza anında iletme bildirimlerini desteklemek üzere güncelleştirilmiştir.
