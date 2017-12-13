---
title: "Notification Hubs son dakika haberleri Eğitmen - Android"
description: "Android cihazları son dakika haberi bildirimleri göndermek için Azure hizmet veri yolu bildirim hub'ları kullanmayı öğrenin."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3b3fc05cfec2b20501a28f3d76f474ccd49e27e8
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Son dakika haberleri göndermek için Notification Hubs kullanma
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış
Bu konu Azure Notification Hubs son dakika haberi bildirimleri bir Android uygulaması yayını için nasıl kullanılacağını gösterir. Tamamlandığında, ilgilendiğiniz haber kategorileri dökümü için kaydedilecek olması ve yalnızca bu kategorilerin anında iletme bildirimlerini almak. Bu sayıda uygulama için genel bir desen bildirimleri daha önce bunları, örneğin, RSS Okuyucu, müzik fanlar, vb. için uygulamaları ilgi derlendiğinden kullanıcı gruplarını gönderilmesini sahip olduğu bir senaryodur.

Yayın senaryoları etkin bir veya daha fazla dahil ederek *etiketleri* bir kayıt bildirim hub'ı oluştururken. Bildirimler için bir etiket gönderildiğinde, kaydettiğiniz tüm etiket cihazlarda bildirim alırsınız. Etiketleri yalnızca dizeleri olduğundan, bunlar önceden hazırlanması gerekmez. Etiketler hakkında daha fazla bilgi için başvurmak [bildirim hub'ları Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Ön koşullar
Bu konu, oluşturduğunuz uygulama inşa edilmiştir [Notification Hubs ile çalışmaya başlama][get-started]. Bu öğreticiye başlamadan önce önce tamamlamış olmalıdır [Notification Hubs ile çalışmaya başlama][get-started].

## <a name="add-category-selection-to-the-app"></a>Kategori seçimi için uygulama ekleme
İlk adım, kullanıcının kaydetmek için kategoriler seçmesine olanak sağlayan, varolan ana etkinlik için kullanıcı Arabirimi öğeleri eklemektir. Bir kullanıcı tarafından seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında bir aygıt kaydı bildirim hub'ınıza seçili kategorileri etiketler oluşturulur.

1. Res/layout/activity_main.xml dosyanızı açın ve içeriğini aşağıdakilerle değiştirin:
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. Res/values/strings.xml dosyanızı açın ve aşağıdaki satırları ekleyin:
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    Main_activity.xml grafik düzeninizi gibi görünmelidir:
   
    ![][A1]
3. Şimdi bir sınıf oluşturun **bildirimleri** aynı pakette, **MainActivity** sınıfı.
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    Bu sınıf yerel depolama almak için bu aygıtın haber kategorilerini depolamak için kullanır. Ayrıca, bu kategorileri kaydetmeye yönelik yöntemler içerir.
4. İçinde **MainActivity** sınıfı kaldırmak için özel alanlar **NotificationHub** ve **GoogleCloudMessaging**, ve bir alan ekleyin **bildirimleri**:
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. Ardından **onCreate** yöntemi başlatılması kaldırmak **hub** alan ve **registerWithNotificationHubs** yöntemi. Hangi örneği başlatılamıyor aşağıdaki satırları ekleyin **bildirimleri** sınıfı. 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`ve `HubListenConnectionString` ile zaten ayarlanmalıdır `<hub name>` ve `<connection string with listen access>` , bildirim hub'ı adı ve bağlantı dizesinde yer tutucularını *DefaultListenSharedAccessSignature* daha önce edindiğiniz.

    > [AZURE.NOTE] Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığı için istemci uygulamanızı dinleme erişim için anahtar yalnızca dağıtmanız. Bildirimler, ancak var olan kayıtlar için kaydetmek için uygulamanızın değiştirilemez erişimi etkinleştirir dinleyin ve bildirim gönderilemiyor. Tam erişim anahtarı, bir güvenli arka uç hizmetinde bildirimleri gönderme ve var olan kayıtlar değiştirmek için kullanılır.


1. Ardından, aşağıdaki içeri aktarmaları ekleyin ve `subscribe` abone ol düğmesini yöntemini, olay'ı tıklatın:
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    Bu yöntem kategorileri ve kullandığı bir liste oluşturur **bildirimleri** listesi yerel depolama alanına depolar ve karşılık gelen kaydetmek için sınıfı, bildirim hub'ınıza etiketler. Kategoriler değiştirildiğinde kaydı yeni kategoriler yeniden oluşturulur.

Uygulamanızı kategorileri kümesi cihazın yerel depolama depolamak ve kullanıcı kategorisi seçimine değiştiğinde bildirim hub'ınızla kaydolmak için sunulmuştur.

## <a name="register-for-notifications"></a>Bildirimler için kaydolun
Yerel depolamada depolanan kategorileri kullanarak başlangıçtaki bildirim hub'ı ile adımları kaydedin.

> [!NOTE]
> Google Cloud Messaging (GCM) tarafından atanan RegistrationId herhangi bir zamanda değiştiğinden bildirimlerinin sık bildirim hataları önlemek için kayıt olmalıdır. Bu örnek uygulama başladıktan her zaman bir bildirime kaydolur. Sık çalıştırılan uygulamalar için günde bir kereden fazla, büyük olasılıkla bir günden az itibaren önceki kayıt aktarılırsa bant genişliğinden tasarruf etmek kayıt atlayabilirsiniz.
> 
> 

1. Sonuna aşağıdaki kodu ekleyin **onCreate** yönteminde **MainActivity** sınıfı:
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    Bu uygulama her başlatıldığında kategorileri yerel depodan alır ve bu kategorilerin bir kayda istekleri emin olur. 
2. Ardından güncelleştirme `onStart()` yöntemi `MainActivity` gibi sınıfı:
   
    @Overridekorumalı void onStart() {
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }
   
    Bu, önceden kaydedilmiş kategorileri durumlarına dayalı ana etkinlik güncelleştirir.

Uygulama tamamlanmıştır ve kategorileri kümesi kullanıcı kategorisi seçimine değiştiğinde bildirim hub'ınızla kaydolmak için kullanılan aygıt yerel depoda depolayabilirsiniz. Ardından, biz bu uygulamaya kategori bildirimleri gönderebilir bir arka uç tanımlayacaksınız.

## <a name="sending-tagged-notifications"></a>Etiketli bildirimleri gönderme
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırın ve bildirimler oluşturma
1. Android Studio'da uygulama oluşturun ve bir cihaz veya öykünücü başlatın.
   
    Uygulama kullanıcı Arabirimi sağlayan bir dizi değiştirir sağlar Not abone olmak için kategorilerini seçin.
2. Bir veya daha fazla kategorileri değiştirir etkinleştirin ve ardından **abone ol**.
   
    Uygulama seçili kategorileri etiketlerine dönüştürür ve bildirim hub'ından seçili etiketleri için yeni bir cihaz kaydı ister. Kayıtlı kategorileri döndürülen ve bir bildirim görüntülenir.
3. .NET konsol uygulaması çalıştırarak yeni bir bildirim gönderin.  Alternatif olarak, bildirim hub'ınıza hata ayıklama sekmesini kullanarak etiketli şablon bildirimleri gönderebilir [Azure portal].
   
    Seçili kategorileri için bildirimleri bildirimleri görünür.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide biz kategoriye göre son dakika haberleri yayın öğrendiniz. Diğer gelişmiş Notification Hubs senaryoları vurgulayın aşağıdaki öğreticiler birini Tamamlanıyor göz önünde bulundurun:

* [Yerelleştirilmiş son dakika haberleri yayınlamak için Notification Hubs'ı kullanma]
  
    Gönderen yerelleştirilmiş bildirimlerini etkinleştirmek için en son haberleri uygulama genişletin öğrenin.

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Yerelleştirilmiş son dakika haberleri yayınlamak için Notification Hubs'ı kullanma]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure portal]: https://portal.azure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
