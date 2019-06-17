---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: e66e7d4646b650d2d811d3807db04605dfddeded
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66141211"
---
1. İçinde **uygulama** dosyasını açın, proje `AndroidManifest.xml`. Sonra aşağıdaki kodu ekleyin `application` etiketiyle:

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

2. Dosyayı açmak `ToDoActivity.java`, aşağıdaki değişiklikleri yapın:

    - İçeri aktarma deyimini ekleyin:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - Tanımını değiştirme `MobileServiceClient` gelen **özel** için **özel statik**, artık şu şekilde görünür:

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

    - Güncelleştirme **onCreate** yöntemi `ToDoActivity` sınıfı. Sonra bu kodu eklediğinizden emin olun `MobileServiceClient` oluşturulur.

        ```java
        registerPush();
        ```

3. Bildirimleri işlemek için yeni bir sınıf ekleyin. Proje Gezgini'nde açın **uygulama** > **java** > **your-proje-namespace** düğümleri ve paket adı düğümünü sağ tıklatın. Tıklayın **yeni**ve ardından **Java sınıfı**. Ad alanına yazın `ToDoMessagingService`ve ardından Tamam'a tıklayın. Ardından, sınıf bildirimi ile değiştirin:

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

4. Belirteç güncelleştirmeleri işlemek için başka bir sınıf ekleyin. Oluşturma `ToDoInstanceIdService` java sınıfı ve sınıf bildirimiyle değiştirin:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Uygulamanızı anında iletme bildirimlerini desteklemek için güncelleştirilmiştir.
