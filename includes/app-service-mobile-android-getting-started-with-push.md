1. İçinde **uygulama** projesi, dosyayı açma `AndroidManifest.xml`. Sonra aşağıdaki kodu ekleyin `application` etiketi açma:

    ```xml
    <service android:name=".ToDoMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
    <service android:name=".ToDoInstanceIdService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Dosyayı açmak `ToDoActivity.java`ve aşağıdaki değişiklikleri yapın:

    - İçeri aktarma deyimini ekleyin:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - Tanımını değiştirin `MobileServiceClient` gelen **özel** için **özel statik**, şimdi şöyle görünür:

        ```java
        private static MobileServiceClient mClient;
        ```

    - Ekleme `registerPush` yöntemi:

        ```java
        public static void registerPush() {
            final String token = FirebaseInstanceId.getInstance().getToken();
            if (token != null) {
                new AsyncTask<Void, Void, Void>() {
                    protected Void doInBackground(Void... params) {
                        mClient.getPush().register(token);
                        return null;
                    }
                }.execute();
            }
        }
        ```

    - Güncelleştirme **onCreate** yöntemi `ToDoActivity` sınıfı. Sonra bu kodu eklediğinizden emin olun `MobileServiceClient` örneği.

        ```java
        registerPush();
        ```

3. Bildirimleri işlemek için yeni bir sınıf ekleyin. Proje Gezgini'nde Aç **uygulama** > **java** > **proje ad bilgisayarınızı** düğümler ve paket adı düğümünü sağ tıklatın. Tıklatın **yeni**ve ardından **Java sınıfı**. Adı yazın `ToDoMessagingService`ve ardından Tamam'ı tıklatın. Ardından, sınıf bildirimi ile değiştirin:

    ```java
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    public class ToDoMessagingService extends FirebaseMessagingService {

        private static final int NOTIFICATION_ID = 1;

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            String message = remoteMessage.getData().get("message");
            if (message != null) {
                sendNotification("Notification Hub Demo", message);
            }
        }

        private void sendNotification(String title, String messageBody) {
            PendingIntent contentIntent = PendingIntent.getActivity(this, 0, new Intent(this, ToDoActivity.class), 0);
            Notification.Builder notificationBuilder = new Notification.Builder(this)
                    .setSmallIcon(R.drawable.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(messageBody)
                    .setContentIntent(contentIntent);
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            if (notificationManager != null) {
                notificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
            }
        }
    }
    ```

4. Belirteç güncelleştirmeleri işlemek için başka bir sınıf ekleyin. Oluşturma `ToDoInstanceIdService` java sınıfı ve sınıf bildirimi ile değiştirin:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Uygulamanıza anında iletme bildirimlerini desteklemek üzere güncelleştirilmiştir.
