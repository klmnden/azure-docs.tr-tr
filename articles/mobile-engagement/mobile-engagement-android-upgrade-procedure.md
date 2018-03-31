---
title: Azure Mobile Engagement Android SDK tümleştirmesi
description: En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 77047cb1dc39fa3c05f58550ceea74e78396157f
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="upgrade-procedures"></a>Yükseltme yordamları
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Uygulamanıza bizim SDK daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK çeşitli sürümleri eksik, birçok yordamı uygulamanız gerekebilir. Örneğin, 1.4.0 önce "Kimden 1.4.0 1.5.0 için" yordamını takip etmek için sahip 1.6.0 sonra "Kimden 1.5.0 1.6.0 için" yordamı geçiş ise.

Yükseltme, seçtiğiniz sürüm değiştirmek zorunda `mobile-engagement-VERSION.jar` yeni bir.

## <a name="from-420-to-421"></a>4.2.0 4.2.1 için
Bu adım SDK'ın herhangi bir sürümü üzerinde gerçekte yapılabilir, ulaşma etkinlikleri tümleştirdiğinizde güvenlik geliştirme olur.

Şimdi eklemelisiniz `exported="false"` tüm ulaşma etkinliklere.

Reach etkinlikleri şimdi şöyle görünmelidir, `AndroidManifest.xml`:

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

## <a name="from-400-to-410"></a>4.0.0 4.1.0'da için
SDK şimdi tanıtıcı yeni izni modelden Android M.

Konum özelliklerini kullanmak veya büyük resmi bildirimleri okuyun [Bu bölümde](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Yeni izni model ek olarak, çalışma zamanında yapılandırma konumu özellikleri artık desteklenmektedir.
Biz yine bildirimi parametreleri konumu için uyumludur, ancak artık kullanılmıyor. Çalışma zamanı yapılandırmasını kullanmak için aşağıdaki bölümlerden kaldırın, ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

okuyup [bu güncelleştirilmiş yordamı](mobile-engagement-android-integrate-engagement.md#location-reporting) çalışma zamanı yapılandırma kullanmayı.

## <a name="from-300-to-400"></a>3.0.0 4.0.0 için
### <a name="native-push"></a>Yerel gönderim
İtme kampanya herhangi bir türde yerel gönderim kimlik bilgilerini yapılandırmak için yerel gönderim (GCM/ADM) şimdi de için uygulama içi Bildirimlerde kullanılır.

Aksi halde zaten Lütfen izleyin [bu yordamı](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Reach tümleştirme değiştirildi ``AndroidManifest.xml``.

Bu değiştirin:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Tarafından

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Yapıldığında büyük olasılıkla yükleme ekran şimdi duyuru (metin/web içerik ile) ya da bir yoklama tıklatın.
Bu 4.0.0 içinde çalışmak bu kampanyalar için eklemeniz gerekir:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Kaynaklar
Yeni katıştırmak `res/layout/engagement_loading.xml` projenize dosya.

## <a name="from-240-to-300"></a>2.4.0 3.0.0 için
Aşağıdaki nasıl Azure Mobile Engagement tarafından desteklenen bir uygulamaya Capptain SAS tarafından sunulan Capptain hizmetinden bir SDK tümleştirmesi geçirileceğini açıklar. Önceki bir sürümünden geçiş yapıyorsanız, lütfen 2.4.0 için ilk geçirmek için Capptain web sitesine başvurun ve sonra aşağıdaki yordamı uygulayın.

> [!IMPORTANT]
> Capptain ve Mobile Engagement aynı Hizmetleri değildir ve aşağıda verilen yordamı yalnızca istemci uygulaması geçirmek nasıl vurgular. Uygulama SDK'yı geçirme verilerinizi Capptain sunucularından Mobile Engagement sunucuya geçişi YAPILMAZ.
> 
> 

### <a name="jar-file"></a>JAR file
Değiştir `capptain.jar` tarafından `mobile-engagement-VERSION.jar` içinde `libs` klasörü.

### <a name="resource-files"></a>Kaynak dosyaları
Bizim sağladığımız her kaynak dosyası (önüne `capptain_`) yenilerini tarafından değiştirilmesi gereken (önekine sahip `engagement_`).

Bu dosyaları özelleştirdiyseniz, özelleştirme yeni dosyalarda yeniden uygulamak sahip **kaynak dosyalarında tüm tanımlayıcıları da yeniden adlandırıldığı**.

### <a name="application-id"></a>Uygulama Kimliği
Şimdi Engagement SDK'sı tanımlayıcıları uygulama tanımlayıcısı gibi yapılandırmak için bir bağlantı dizesi kullanır.

Kullanmak zorunda `EngagementAgent.init` Başlatıcısı etkinliklerinizi şöyle yöntemi:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure portalında görüntülenir.

Lütfen herhangi çağrısına kaldırın `CapptainAgent.configure` olarak `EngagementAgent.init` o yöntemi değiştirir.

`appId` Artık kullanılarak yapılandırılabilir `AndroidManifest.xml`.

Lütfen bu bölümünden kaldırın, `AndroidManifest.xml` , varsa:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API’si
Yeniden adlandırılacak her çağrı için herhangi bir Java sınıf bizim SDK'sının sahiptir; Örneğin, `CapptainAgent.getInstance(this)` kaydedilmelidir `EngagementAgent.getInstance(this)`, `extends CapptainActivity` kaydedilmelidir `extends EngagementActivity` vb....

Varsayılan aracı tercih dosyalarını ile tümleşik varsayılan dosya adı şimdi varsa, `engagement.agent` ve anahtar `engagement:agent`.

Web duyuruları oluştururken, Javascript bağlayıcı sunulmuştur `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Çok sayıda değişiklik vardır oldu, hizmet artık paylaşılmayan ve alıcıları çok değildir verilebilir artık.

Hizmet bildirimi artık daha kolaydır; hedefi filtre ve içindeki tüm meta veri kaldırıp eklemek `exportable=false`.

Ayrıca her şeyi engagement kullanılacağı adlandırılır.

Şimdi gibi görünüyor:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Test günlüklerinin sağlamak istiyorsanız, meta verileri artık uygulama etiketi taşındı ve yeniden adlandırıldı:

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

Tüm diğer meta verileri yalnızca yeniden adlandırıldığı, tam listesini (indirmelere rename yalnızca kullandığınız olanlar) aşağıdadır:

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Bu değişikliği kaldırmak zorunda SDK Google Play ve izleme SmartAd kaldırıldı:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Reach etkinlikleri şöyle bildirilir:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

Özel erişim etkinlikler varsa, ya da eşleşecek şekilde hedefi eylemleri yalnızca değiştirmeye ihtiyaç `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` veya `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Yayın alıcıları yeniden adlandırıldığı artı şimdi eklediğimiz `exported=false`. Yeni belirtimi, (indirmelere rename yalnızca kullandığınız olanlar) alıcılarla tam listesi aşağıdadır:

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Bu bölümde kaldırmanız gerekir böylece alıcı izleme kaldırıldı:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Unutmayın yayın alıcı uygulanmasını bildirimi **EngagementMessageReceiver** deki `AndroidManifest.xml`. Göndermek ve rasgele XMPP iletileri rasgele XMPP varlıklardan kaldırmak için API ve cihazlar arasında ileti gönderme ve alma için API kaldırılmış olmasıdır. Böylece, aşağıdaki geri çağırmaları gelen silmek de, **EngagementMessageReceiver** uygulama:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

ve

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

üzerinde herhangi bir çağrısına silme **EngagementAgent** için:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

ve

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard
Proguard yapılandırma rebranding tarafından etkilenebilir, kuralları şimdi gibi arıyorsunuz:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

