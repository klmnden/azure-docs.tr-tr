---
title: "Azure Mobile Engagement Android SDK tümleştirmesi"
description: "En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 26ba47b19f3a503693d60d344ad39b9eba74fe99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-engagement-reach-on-android"></a>Android'de Engagement Reach tümleştirme
> [!IMPORTANT]
> Tümleştirme katılım nasıl Android belge üzerinde bu kılavuzu izlemeden önce açıklanan tümleştirme yordamı izlemeniz gerekir.
> 
> 

## <a name="standard-integration"></a>Standart tümleştirme

Projenize SDK ulaşma kaynak dosyalarını kopyalayın:

* Dosyalarından kopyalamak `res/layout` klasörü teslim SDK'sı `res/layout` uygulamanızın klasör.
* Dosyalarından kopyalamak `res/drawable` klasörü teslim SDK'sı `res/drawable` uygulamanızın klasör.

Düzenleme, `AndroidManifest.xml` dosyası:

* Aşağıdaki bölümde ekleyin (arasında `<application>` ve `</application>` etiketleri):
  
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/plain" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/html" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
              <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
              <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
          </activity>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
              <action android:name="android.intent.action.BOOT_COMPLETED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
          </receiver>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
              <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
          </receiver>
* Önyüklemede tıklattınız değil Sistem bildirimleri yeniden yürütme izni gerekir (Aksi halde diskte tutulur ancak artık görüntülenmeyecek, gerçekten bu eklemek zorunda).
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* Kopyalama ve aşağıdaki bölümde düzenleme bildirimler (hem de uygulama ve sistem olanlar) için kullanılan bir simge belirtin (arasında `<application>` ve `</application>` etiketleri):
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> Bu bölüm **zorunlu** sistem bildirimleri Reach kampanyaları oluştururken kullanmayı planlıyorsanız. Android gösterilen gelen sistem bildirimleri simgeler olmadan engeller. Bu nedenle bu bölümde atlarsanız, son kullanıcılarınızın bunları almak mümkün olmaz.
> 
> 

* Büyük resim kullanarak sistem bildirimleri ile Kampanyalar oluşturursanız, aşağıdaki izinleri ekleyin gerekir (sonra `</application>` etiketi) eksikse:
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * Android M ve uygulamanızı Android API düzeyi 23 ya da daha hedefleyip hedeflemediği ``WRITE_EXTERNAL_STORAGE`` izni kullanıcı onayı gerektirir. Lütfen okuyun [Bu bölümde](mobile-engagement-android-integrate-engagement.md#android-m-permissions).
* Sistem bildirimleri için cihazın halka ve/veya Titret Reach kampanya de belirtebilirsiniz. Bunun çalışması için aşağıdaki izin bildirildiğinden emin olun zorunda (sonra `</application>` etiketi):
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  Bu izin olmadan Android engeller sistem bildirimleri engeller halka veya Reach kampanya Yöneticisi'nde vibrate seçeneğini işaretli gösterilir.

## <a name="native-push"></a>Yerel gönderim
Reach modülünün yapılandırılmış, cihazda Kampanyalar şekilde alabilmesi için yerel gönderimi yapılandırmanız gerekir.

Android iki hizmet destekliyoruz:

* Google Play aygıtları: kullanım [Google Cloud Messaging] izleyerek [tümleştirmek GCM katılım Kılavuzu ile nasıl](mobile-engagement-android-gcm-integrate.md) Kılavuzu.
* Amazon aygıtları: kullanım [Amazon Device Messaging] izleyerek [tümleştirmek ADM katılım Kılavuzu ile nasıl](mobile-engagement-android-adm-integrate.md) Kılavuzu.

Amazon ve Google Play aygıtlar, geliştirme için 1 AndroidManifest.xml/APK içindeki tüm öğeler için olası hedef istiyorsanız. Ancak GCM kod bulursanız, Amazon gönderirken, bunlar uygulamanızı reddedebilir.

Bu durumda birden çok APKs kullanmanız gerekir.

**Uygulamanızı şimdi almak ve reach kampanyaları görüntülemek hazır!**

## <a name="how-to-handle-data-push"></a>Veri gönderimi nasıl ele alınacağını
### <a name="integration"></a>Tümleştirme
Reach veri gönderimleri almak, uygulamanızın istiyorsanız, bir alt sınıfı oluşturmak zorunda `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` ve içinde referans `AndroidManifest.xml` dosyası (arasında `<application>` ve/veya `</application>` etiketleri):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Geçersiz kılabilirsiniz `onDataPushStringReceived` ve `onDataPushBase64Received` geri aramalar. Örnek aşağıda verilmiştir:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategori
Kategori parametresi bir veri gönderme kampanya oluşturduğunuzda isteğe bağlıdır ve filtre veri iter sağlar. Veri gönderimleri, farklı türlerde işleme birkaç yayın alıcılar varsa kullanışlıdır veya farklı tür itmek istiyorsanız, `Base64` veri ve ayrıştırmadan önce kendi türünü tanımlamak istiyorsanız.

### <a name="callbacks-return-parameter"></a>Geri aramalar dönüş parametresi
Dönüş parametresi düzgün işlenecek yönergelere `onDataPushStringReceived` ve `onDataPushBase64Received`:

* Bir yayın alıcı döndürmelidir `null` içinde veri gönderimi nasıl ele alınacağını bilmiyorsa geri çağırma. Kategori veya yayın Alıcınız veri gönderimi işleyip işlemeyeceğini belirlemek için kullanmanız gerekir.
* Yayın alıcı birini döndürmelidir `true` içinde veri gönderimi kabul ederse geri çağırma.
* Yayın alıcı birini döndürmelidir `false` içinde veri gönderimi algılar ancak herhangi bir nedenle atar geri çağırma. Örneğin, sonuç `false` zaman alınan veri geçerli değil.
* Bir alıcı döndürür yayın varsa `true` başka döndürür while `false` aynı veri gönderimi için davranış tanımsızdır, hiçbir zaman, yapmanız gerekir.

Dönüş türü yalnızca ulaşma istatistikleri kullanılır:

* `Replied`yayın alıcıları birini ya da döndürülürse artırılır `true` veya `false`.
* `Actioned`yalnızca yayın alıcıları birini döndürülürse artırılır `true`.

## <a name="how-to-customize-campaigns"></a>Kampanyalar özelleştirme
Kampanyalar özelleştirmek için Reach SDK'ın sağlanan düzenleri değiştirebilirsiniz.

Düzenleri kullanılan tüm tanımlayıcıları tutmak ve özellikle metin görünümleri ve görüntü görünümleri için bir tanımlayıcı kullanın görünüm türlerini tutun. Bazı görünümler, yalnızca kendi türü değiştirilemez böylece alanları göstermek veya gizlemek için kullanılır. Lütfen sağlanan düzenleri görünümünde türünü değiştirmek istiyorsanız, kaynak kodunu kontrol edin.

### <a name="notifications"></a>Bildirimler
Bildirimler iki tür vardır: farklı düzeni dosyaları kullanan sistem ve uygulama içi bildirimler.

#### <a name="system-notifications"></a>Sistem bildirimleri
Sistem bildirimleri kullanmanız gerekir özelleştirmek için **kategorileri**. Atlayabilirsiniz [kategorileri](#categories).

#### <a name="in-app-notifications"></a>Uygulama bildirimleri
Varsayılan olarak, bir uygulama bildirimi geçerli etkinliği kullanıcı arabirimi sayesinde Android yöntemi dinamik olarak eklenen görünümdür `addContentView()`. Bu bildirim bir katmana çağrılır. Uygulamanız herhangi bir düzende değiştirmenizi gerektirmediği için bildirim yer paylaşımları hızlı tümleştirme için mükemmeldir.

Bildirim yer paylaşımları görünümünü değiştirmek için yalnızca dosyasını değiştirebilirsiniz `engagement_notification_area.xml` gereksinimlerinize.

> [!NOTE]
> Dosya `engagement_notification_overlay.xml` bir bildirim katmana oluşturmak için kullanılan sunucudur dosyayı içeren `engagement_notification_area.xml`. (Katmana içinde bildirim alanında konumlandırma ettirilmesi gibi), gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Bir etkinlik düzeni bir parçası olarak bildirim düzeni içerir
Yer paylaşımları hızlı tümleştirme için harika ancak kullanışsız veya özel durumlarda yan etkileri olabilir. Bir katmana sistem düzeyinde özel etkinlikler için yan etkileri önlemek kolaylaşır bir etkinlik, özelleştirilebilir.

Varolan düzeninizi Android sayesinde bizim bildirim düzeni dahil karar verebilirler **dahil** deyimi. Aşağıdaki değiştirilmiş bir örneğidir `ListActivity` düzenini yalnızca içeren bir `ListView`.

**Engagement tümleştirmesi önce:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Engagement tümleştirmesi sonra:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Bu örnekte, özgün düzeni bir liste görünümü en üst düzey öğesi olarak kullanılan bu yana bir üst öğe kapsayıcısı eklediğimiz. Ayrıca eklediğimiz `android:layout_weight="1"` ile yapılandırılmış bir liste görünümü aşağıdaki görünüm eklemek için `android:layout_height="fill_parent"`.

Engagement Reach SDK'sı, bildirim düzeni bu etkinlikte bulunan ve bu etkinlik için bir katmana eklemez otomatik olarak algılar.

> [!TIP]
> Uygulamanızda bir ListActivity kullanırsanız, görünür bir Reach katmana tıkladığınız için liste görünümünde öğeleri artık tepki engeller. Bu bilinen bir sorundur. Bu sorunu çözmek için bildirim düzeni düzeninde kendi listesi etkinliği gibi önceki örnekteki katıştırmak için öneririz.
> 
> 

##### <a name="disabling-application-notification-per-activity"></a>Etkinlik başına uygulama bildirimini devre dışı bırakma
Etkinliğiniz için eklenecek katmana istemediğiniz ve kendi düzende bildirim düzeni eklemezseniz, bu etkinlik bir katmana devre dışı bırakabilir `AndroidManifest.xml` ekleyerek bir `meta-data` bölümüne aşağıdaki örnekte ister:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorileri
Sağlanan düzenleri değiştirdiğinizde, tüm bildirimlerinizi görünümünü değiştirin. Kategoriler, bildirimler için çeşitli hedeflenen görünüyor (büyük olasılıkla davranışları) tanımlamanıza olanak sağlar. Bir kategori Reach kampanya oluşturduğunuzda belirtilebilir. Bu belgenin sonraki bölümlerinde açıklanan kategoriler de duyuruları ve yoklamaları, özelleştirmenize olanak tanır olduğunu aklınızda bulundurun.

Bildirimlerinizi için bir kategori işleyici kaydetmek için uygulama başlatıldığında bir çağrı eklemeniz gerekir.

> [!IMPORTANT]
> Android: işlem özniteliği hakkında uyarı okuyun \<android sdk katılım işlem\> nasıl tümleştirmek engagement Android konuda devam etmeden önce.
> 
> 

Aşağıdaki örnek, önceki uyarı onaylanır ve bir alt sınıfı kullanın varsayar `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

`MyNotifier` Nesne, bildirim kategori işleyici uygulamasıdır. Her iki uygulaması olan `EngagementNotifier` arabirimi veya varsayılan uygulaması bir alt sınıfı: `EngagementDefaultNotifier`.

Aynı bildirim birkaç kategorisi işleyebilir, bunları şöyle kaydedebilirsiniz dikkat edin:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Varsayılan Kategori uygulaması değiştirmek için aşağıdaki örnekte, uygulamanızı gibi kaydedebilirsiniz:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

İşleyici kullanılan geçerli kategorisi çoğu yöntemleri içinde geçersiz parametre olarak geçirilen `EngagementDefaultNotifier`.

Olarak ya da geçirilir bir `String` parametresi veya dolaylı bir `EngagementReachContent` olan nesne bir `getCategory()` yöntemi.

Üzerinde yöntemleri tanımlayarak bildirim oluşturma işlemi çoğunu değiştirebilirsiniz `EngagementDefaultNotifier`, daha gelişmiş özelleştirme için teknik belgeler ve kaynak kodu göz çekinmeyin.

##### <a name="in-app-notifications"></a>Uygulama bildirimleri
Yalnızca belirli bir kategorideki için alternatif düzenleri kullanmak istiyorsanız, bunu aşağıdaki örnekteki uygulayabilirsiniz:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Örnek `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Gördüğünüz gibi katmana görünümü standart olandan farklı tanımlayıcısıdır. Her düzeni yer paylaşımları için benzersiz bir tanımlayıcı kullanmak önemlidir.

**Örnek `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Gördüğünüz gibi bildirim alanında görünümü standart olandan farklı tanımlayıcısıdır. Her düzeni bildirim alanları için benzersiz bir tanımlayıcı kullanır önemlidir.

Bu basit örnek kategorisinin ekranın en üstte gösterilen uygulama (veya uygulama) bildirim yapar. Bildirim alanında kendisini kullanılan standart tanımlayıcıları değişmemiştir.

Değiştirmek istediğiniz tanımlanacak varsa `EngagementDefaultNotifier.prepareInAppArea` yöntemi. Teknik belgeler ve kaynak kodunu aramak için önerilen `EngagementNotifier` ve `EngagementDefaultNotifier` bu Gelişmiş özelleştirme düzeyi istiyorsanız.

##### <a name="system-notifications"></a>Sistem bildirimleri
Genişletme tarafından `EngagementDefaultNotifier`, geçersiz kılabilirsiniz `onNotificationPrepared` varsayılan uygulama tarafından hazırlanan bildirim değiştirmek için.

Örneğin:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Bu örnekte "devam eden" kategorisi kullanıldığında, devam eden bir olay görüntülenmesini içerik için bir sistem bildirimi yapar.

Oluşturmak istiyorsanız `Notification` nesne baştan geri dönebilirsiniz `false` yöntemi ve çağrı `notify` kendiniz `NotificationManager`. Bu durumda, tutmanızı önemli bir `contentIntent`, `deleteIntent` ve tarafından kullanılan bildirim tanımlayıcısı `EngagementReachReceiver`.

Bu tür bir uygulama doğru bir örneği burada verilmiştir:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Yalnızca bildirim duyuruları
Yalnızca duyuru kılarak özelleştirilebilir bir bildirim tıklayarak Yönetim `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` hazırlıklı değiştirmek için `Intent`. Bu yöntemi kullanarak bayrakları kolayca ayarlamak sağlar.

Örneğin eklemek `SINGLE_TOP` bayrağı:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Bu yöntem, eylem URL'si olmadan duyuru kullanılarak çağrılamaz. böylece arka planda ise, sistem bildirimleri eylemi olmadan URL şimdi uygulama başlatma eski katılım kullanıcılar için lütfen unutmayın. Bu amaç özelleştirirken düşünmelisiniz.

Ayrıca uygulayabilirsiniz `EngagementNotifier.executeNotifAnnouncementAction` sıfırdan.

##### <a name="notification-life-cycle"></a>Bildirim yaşam döngüsü
Varsayılan Kategori kullanırken, bazı yaşam döngüsü yöntemleri üzerinde denir `EngagementReachInteractiveContent` nesne istatistikleri rapor ve kampanya durumunu güncelleştirin:

* Bildirim uygulamada görüntülenen ya da durum çubuğunda put `displayNotification` yöntemi (hangi istatistikleri raporları) çağrılır tarafından `EngagementReachAgent` varsa `handleNotification` döndürür `true`.
* Bildirim kapatıldığında, `exitNotification` yöntemi çağrıldığında, istatistik bildirilir ve sonraki Kampanyalar şimdi işlenebilir.
* Bildirim tıkladıysanız `actionNotification` olan çağrılır, istatistik bildirilir ve ilişkili amacı başlatılır.

Uygulamanıza `EngagementNotifier` varsayılan davranışı, atlar başınıza bu yaşam döngüsü yöntemlerini çağırmaya sahip. Aşağıdaki örneklerde varsayılan davranış olduğu atlanır bazı durumlarda gösterilmektedir:

* Genişletme yok `EngagementDefaultNotifier`, sıfırdan kategori işleme uygulanan örn.
* Sistem bildirimleri için geçersiz kılınmış `onNotificationPrepared` ve değiştirdiğiniz `contentIntent` veya `deleteIntent` içinde `Notification` nesnesi.
* Uygulama bildirimleri için geçersiz kılınmış `prepareInAppArea`, en az eşlemek mutlaka `actionNotification` , U.I birine denetler.

> [!NOTE]
> Varsa `handleNotification` bir özel durum, içeriği silinir atar ve `dropContent` olarak adlandırılır. Bu durum istatistiklerine bildirilir ve sonraki Kampanyalar şimdi işlenebilir.
> 
> 

### <a name="announcements-and-polls"></a>Duyuruları ve yoklamaları
#### <a name="layouts"></a>Düzenleri
Değiştirebileceğiniz `engagement_text_announcement.xml`, `engagement_web_announcement.xml` ve `engagement_poll.xml` metin Duyurular, web duyuruları ve yoklamaları özelleştirmek için dosyaları.

Bu dosyalar başlık alanını ve düğme alanının için iki ortak düzenleri paylaşır. Başlığa ilişkin Düzen `engagement_content_title.xml` ve eponymous drawable dosyası için arka plan kullanır. Eylem ve çıkış düğmeleri düzeni `engagement_button_bar.xml` ve eponymous drawable dosyası için arka plan kullanır.

Bir yoklamada soru düzeni ve seçimleri dinamik olarak birkaç kez kullanarak şişirileceğini `engagement_question.xml` sorular için Düzen dosyasını ve `engagement_choice.xml` seçimler dosya.

#### <a name="categories"></a>Kategoriler
##### <a name="alternate-layouts"></a>Alternatif düzenleri
Bildirimler gibi kampanya kategori duyuruları ve yoklamaları için alternatif düzenleri sağlamak için kullanılabilir.

Örneğin, bir metin duyuru için bir kategori oluşturmak için genişletebilirsiniz `EngagementTextAnnouncementActivity` ve referans `AndroidManifest.xml` dosyası:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Hedefi filtre kategorisinde varsayılan duyuru etkinlik farkı yapmak için kullanılır.

Belirli bir kategorideki sağ etkinliği gidermek için hedefi sistem Reach SDK'sını kullanır ve çözümleme başarısız olursa, geri varsayılan kategorisine göre döner.

Uygulamak sahip `MyCustomTextAnnouncementActivity`, yalnızca düzenini değiştirme (ancak aynı tanımlayıcılarını görüntüle tutmak) istiyorsanız, yalnızca aşağıdaki örnekte sınıfı gibi tanımlamak gerekir:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Metin duyuruları varsayılan kategorisini değiştirmek için yalnızca Değiştir `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` , uygulamanız tarafından.

Web duyuruları ve yoklamaları için benzer bir şekilde özelleştirilebilir.

Web duyuruları için genişletebilirsiniz `EngagementWebAnnouncementActivity` ve etkinlik bildirmek `AndroidManifest.xml` aşağıdaki örnekte ister:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Anketler için genişletebilirsiniz `EngagementPollActivity` ve declare, içinde `AndroidManifest.xml` aşağıdaki örnekte ister:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Sıfırdan uygulama
Aşağıdakilerden birini genişletmeden duyuru (ve yoklama) etkinliklerinizi için kategoriler uygulayabilirsiniz `Engagement*Activity` Reach SDK tarafından sağlanan sınıfları. Örneğin, aynı görünümleri standart düzenleri kullanmayan bir düzen tanımlamak istiyorsanız kullanışlıdır.

Gibi gelişmiş bildirim özelleştirme için standart uygulama kaynak koduna bakmanız önerilir.

Göz önünde bulundurmanız gereken bazı şeyler şunlardır: ulaşma içerik tanıtıcısı olan bir ek parametre (hedefi filtre karşılık gelen) belirli bir amaç etkinlikle başlayacaktır.

Kampanya web sitesinde oluşturulurken belirtilen alanları içeren içerik nesnesini almak için bunu yapabilirsiniz:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

STATISTICS için içerik görüntülendiği bildirmelisiniz `onResume` olay:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Sonra ya da çağırmayı unuttunuz mu `actionContent(this)` veya `exitContent(this)` arka plana etkinlik geçmeden önce içerik nesne üzerinde.

Ya da yok çağırırsanız `actionContent` veya `exitContent`, istatistikleri (yani bir kampanya hiçbir analytics) gönderilen olmaz ve uygulama işlemini yeniden başlatılana kadar daha da önemlisi, sonraki Kampanyalar bildirilmez.

Yönlendirme veya başka yapılandırma değişiklikleri yapabilir kodunu aktivite arka plana veya değil, standart uygulama içeriği kullanıcı etkinliği ayrılsa çıktı olarak bildirilen emin yapar giden olup olmadığını belirlemek hassas ( basarakyada`HOME`veya `BACK`) ancak yönlendirmesini değişirse değil.

Uygulama ilginç parçası şöyledir:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Aradığınız varsa, gördüğünüz gibi `actionContent(this)` Etkinliğin bitiş sonra `exitContent(this)` herhangi bir etkisi olmadan güvenli bir şekilde çağrılabilir.

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
