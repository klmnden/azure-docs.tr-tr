---
title: "Azure Mobile Engagement Android SDK tümleştirmesi"
description: "En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 35bd92e52b7a02f58620a03156902f9f91be57ae
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-integrate-engagement-on-android"></a>Android'de katılım tümleştirme
> [!div class="op_single_selector"]
> * [Windows Evrensel](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Bu yordam, Engagement analizi ve izleme işlevlerine Android uygulamanızdaki etkinleştirmek için en basit yolu açıklar.

> [!IMPORTANT]
> En düşük Android SDK API düzey 10 veya daha yüksek olmalıdır (Android 2.3.3 ya da daha yüksek).
> 
> 

Aşağıdaki adım etkinleştirir için kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve Technicals ilgili tüm istatistikleri işlem için gereken günlükleri rapor yeterli değildir. Olaylar, hatalar ve işleri gibi diğer istatistikleri işlem için gereken günlükleri rapor katılım API kullanarak el ile yapılması gerekir (bkz [, Android API etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-android-use-engagement-api.md) Bu istatistikler olduğundan uygulamaya bağlıdır.

## <a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Hizmet ve Engagement SDK'sı Android projenize ekleme
Android SDK Yükle [burada](https://aka.ms/vq9mfn) almak `mobile-engagement-VERSION.jar` ve bunların içine yerleştirin `libs` klasörü Android projenizin (henüz yoksa libs klasörüne oluşturma).

> [!IMPORTANT]
> Uygulama paketinizi ProGuard ile yapılandırdıysanız, bazı sınıfları tutmanız gerekir. Aşağıdaki yapılandırma parçacığını kullanabilirsiniz:
> 
> -Ortak sınıfı tutmak * android.os.IInterface genişletir-sınıfı com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {tutun
> 
> <methods>; }
> 
> 

Başlatıcı etkinliğin aşağıdaki yöntemini çağırarak katılım bağlantı dizenizi belirtin:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Bağlantı dizesi, uygulamanız için Azure portalında görüntülenir.

* Eksikse, aşağıdaki Android izinleri ekleyin (önce `<application>` etiketi):
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* Aşağıdaki bölümde ekleyin (arasında `<application>` ve `</application>` etiketleri):
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* Değişiklik `<Your application name>` , uygulamanızın adı.

> [!TIP]
> `android:label` Özniteliği telefonlarını "Hizmetleri çalışır" ekranında son kullanıcılar için görünür olarak katılım hizmetin adını seçin olanak tanır. Bu öznitelik ayarlamak için önerilen `"<Your application name>Service"` (örneğin `"AcmeFunGameService"`).
> 
> 

Belirtme `android:process` özniteliği sağlar katılım hizmet (uygulamanızın ana/kullanıcı Arabirimi iş parçacığı potansiyel olarak daha az yanıt yapacak şekilde katılım aynı işlem içinde çalışan) kendi işleminde çalışır.

> [!NOTE]
> Yerleştirdiğiniz içinde herhangi bir kod `Application.onCreate()` ve diğer uygulama geri aramalar katılım hizmeti dahil olmak üzere tüm uygulama işlemleri için çalışır. (Örneğin, gereksiz bellek ayırma ve Engagement'ın işlemde, yinelenen yayın alıcılar veya hizmetleri iş parçacıkları) istenmeyen yan etkileri olabilir.
> 
> 

Geçersiz kılarsanız `Application.onCreate()`, aşağıdaki kod parçacığını başında eklemek için önerilen, `Application.onCreate()` işlevi:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Aynı şeyi yapmak `Application.onTerminate()`, `Application.onLowMemory()` ve `Application.onConfigurationChanged(...)`.

Ayrıca genişletebilirsiniz `EngagementApplication` genişletme yerine `Application`: geri çağırma `Application.onCreate()` işlem denetimi yapar ve çağrıları `Application.onApplicationProcessCreate()` yalnızca geçerli işlem katılım hizmetini barındıran bir durumda değilse, aynı kuralları için diğer uygulama geri aramalar.

## <a name="basic-reporting"></a>Temel raporlama
### <a name="recommended-method-overload-your-activity-classes"></a>Önerilen yöntem: aşırı yükleme, `Activity` sınıfları
Rapor kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve teknik istatistikleri işlem katılım tarafından gerekli tüm günlüklerin etkinleştirmek için yalnızca tüm yapmanız gerekir, `*Activity` alt sınıfları devralır denk gelen `Engagement*Activity` (örneğin sınıfları eski etkinliklerinizi geçerse `ListActivity`, onu genişletir yapma `EngagementListActivity`).

**Katılım:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Katılım ile:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> Kullanırken `EngagementListActivity` veya `EngagementExpandableListActivity`, aşağıdakilerden emin olun çağrı `requestWindowFeature(...);` çağırmadan önce yapılan `super.onCreate(...);`, aksi takdirde bir kilitlenme meydana gelir.
> 
> 

Bu sınıfları bulabilirsiniz `src` klasörünü ve bunları projenize kopyalayabilirsiniz. Ayrıca, sınıflardır **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatif yöntem: çağrı `startActivity()` ve `endActivity()` el ile
Olamaz ya da tekrar etmek istiyor musunuz, `Activity` sınıfları, bunun yerine başlangıç ve çağırarak etkinliklerinizi bitiş `EngagementAgent`'s doğrudan yöntemleri.

> [!IMPORTANT]
> Android SDK hiçbir zaman çağırır `endActivity()` uygulama kapalı olduğunda bile yöntemi (Android, uygulamaları gerçekte hiçbir zaman kapalı). Bu nedenle, olan *yüksek oranda* çağırmak için önerilen `startActivity()` yönteminde `onResume` , geri çağırma *tüm* , etkinlikler ve `endActivity()` yönteminde `onPause()` , geri çağırma *Tüm* etkinliklerinizi. Bu oturumlar sızmaması emin olmak için tek yoludur. Bir oturum sızmasını varsa (bir oturum bekleyen olduğu sürece hizmet bağlı kalır bu yana) katılım hizmet hiçbir zaman katılım arka ucundan bağlantısını keser.
> 
> 

Örnek aşağıda verilmiştir:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Bu örnek için çok benzer `EngagementActivity` sınıfı ve kaynak kodu sağlanır türevleri `src` klasör.

## <a name="test"></a>Test etme
Şimdi bir öykünücü veya cihaz ve mobil uygulamanızı çalıştırarak ve bir oturum izleme sekmesinde kayıtları doğrulanıyor Lütfen tümleştirmenize doğrulayın.

Sonraki bölümlerde isteğe bağlıdır.

## <a name="location-reporting"></a>Konum raporlama
Konumları bildirilmesini istiyorsanız, birkaç satırlı yapılandırma eklemeniz gerekir (arasında `<application>` ve `</application>` etiketleri).

### <a name="lazy-area-location-reporting"></a>Yavaş alan konumu raporlama
Yavaş alan konumu raporlama ülke, bölgeye ve yere göre cihazlara ilişkili rapor oluşturmaya olanak tanır. Bu tür konumu raporlama yalnızca ağ konumlarını (hücre kimliği veya WIFI göre) kullanır. Aygıt alanına oturum başına en fazla bir kez bildirilir. GPS hiçbir zaman kullanılmaz ve bu nedenle bu konumu rapor çok az (no değil söylemeniz) türü pil üzerindeki etkisi.

Bildirilen alanları kullanıcıları, oturumlar, olayları ve hataları ile ilgili coğrafi istatistikleri hesaplamak için kullanılır. Reach kampanyaları ölçütü olarak de kullanılabilir.

Yavaş alan konumu raporlama etkinleştirmek için bu yordamda daha önce bahsedilen yapılandırmayı kullanarak bunu yapabilirsiniz:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Veya kullanmaya devam edebileceğiniz ``ACCESS_FINE_LOCATION`` zaten uygulamanızda kullanın.

### <a name="real-time-location-reporting"></a>Gerçek zamanlı konum raporlama
Gerçek zamanlı konum raporlama enlem ve boylam cihazlara ilişkili rapor oluşturmaya olanak tanır. Varsayılan olarak, bu tür konumu raporlama yalnızca ağ konumlarını (hücre kimliği veya WIFI göre) kullanır ve uygulama ön planda (yani oturumu sırasında) çalıştığında raporlama yalnızca etkindir.

Gerçek zamanlı konumlarının *değil* istatistikleri hesaplamak için kullanılır. Gerçek zamanlı coğrafi yalıtma kullanımına izin vermek için kendi tek amacı olan \<Reach-İzleyici-bölge sınırlaması\> Reach kampanyaları ölçütü.

Gerçek zamanlı konum raporlama etkinleştirmek için bu yordamda daha önce bahsedilen yapılandırmayı kullanarak bunu yapabilirsiniz:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Veya kullanmaya devam edebileceğiniz ``ACCESS_FINE_LOCATION`` zaten uygulamanızda kullanın.

#### <a name="gps-based-reporting"></a>Raporlama GPS dayalı
Varsayılan olarak, gerçek zamanlı konum raporlama ağ tabanlı konum yalnızca kullanır. (Olan daha kesin) tabanlı GPS konumları kullanımını etkinleştirmek için yapılandırma nesnesi kullanın:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Arka plan raporlama
Uygulama ön planda (yani oturumu sırasında) çalıştırdığında, varsayılan olarak, gerçek zamanlı konum raporlama yalnızca etkindir. Aynı zamanda arka planda raporlamayı etkinleştirmek için yapılandırma nesnesi kullanın:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Tabanlı ağ konumları yalnızca uygulama arka planda çalıştığında raporlanır, GPS etkin olsa bile.
> 
> 

Kullanıcı, cihaz yeniden başlatılırsa, arka plan konum rapor durdurulur, önyükleme sırasında otomatik olarak yeniden sağlamak için bunu ekleyebilirsiniz:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M izinleri
Android M ile başlayarak, bazı izinler çalışma zamanında yönetilir ve kullanıcı onayı gerekiyor.

Android API düzeyi 23 hedefliyorsanız çalışma zamanı izinleri yeni uygulama yüklemeleri için varsayılan olarak kapatılır. Aksi takdirde, varsayılan olarak açılır.

Kullanıcı etkinleştirebilir/aygıt ayarları menüsünden bu izinleri devre dışı bırakabilir. Arka plan işlemleri uygulamasının sistem menüsünden izinleri kapatma devre dışı bırakır, bu bir sistem davranıştır ve anında iletme arka planda alma yeteneğini üzerinde hiçbir etkisi olmaz.

Mobile Engagement bağlamında, çalışma zamanında onayı iste izinler şunlardır:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* `WRITE_EXTERNAL_STORAGE`(yalnızca Android API düzeyini 23 bunu hedeflerken)

Harici depolama yalnızca ulaşma büyük resmi özelliği için kullanılır. Görürseniz yalnızca Mobile Engagement için ancak büyük resmi özelliği devre dışı bırakma, kullandıysanız kullanıcılar kesintiye uğratan olması için bu izin isteyen, kaldırabilirsiniz.

Konum özellikler için standart sistem iletişim kutusunu kullanarak kullanıcı izni istemeniz gerekir. Kullanıcı onaylarsa, bildirmeniz gerekir ``EngagementAgent`` gerçek zamanlı olarak bu değişikliği dikkate almak için (Aksi halde değişikliği kullanıcı başlatır uygulama başlatıldığında işlenir).

İzinleri istemek ve sonuç için pozitif varsa iletmek için uygulamanızın bir etkinlikte kullanılacak bir kod örneği işte ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a>Gelişmiş raporlama
Uygulama belirli olaylar, hatalar ve işleri rapor istiyorsanız, isteğe bağlı olarak, katılım API yöntemlerini kullanmanız gerekebilir `EngagementAgent` sınıfı. Bu sınıfın bir nesnesi çağırarak alınma olabilir `EngagementAgent.getInstance()` statik yöntemi.

Katılım API tüm Engagement'ın gelişmiş özelliklerinden kullanacak şekilde sağlar ve nasıl ayrıntılı Android katılım API kullanmak için (teknik belgeleri olarak yanı `EngagementAgent` sınıfı).

## <a name="advanced-configuration-in-androidmanifestxml"></a>Gelişmiş yapılandırmasında (AndroidManifest.xml)
### <a name="wake-locks"></a>Uyandırma kilitleri
İstatistikleri Wifi kullanırken gerçek zamanlı veya ekran kapalıyken gönderildiğinden emin olmak istiyorsanız, aşağıdaki isteğe bağlı iznini ekleyin:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Kilitlenme raporu
Kilitlenme raporları devre dışı bırakmak istiyorsanız, bunu ekleyin (arasında `<application>` ve `</application>` etiketleri):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Eşik veri bloğu
Varsayılan olarak, katılım hizmet raporları gerçek zamanlı olarak günlüğe kaydeder. Uygulamanızı günlükleri çok sık bildirirse, günlükleri arabellek ve (Buna "veri bloğu modu" denir) tümünü bir defada bir normal zaman üzerinde temel bildirmek için daha iyi olur. Bunu yapmak için bu ekleyin (arasında `<application>` ve `</application>` etiketleri):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Veri bloğu modu biraz pil ömrünün artırabilirsiniz ancak Engagement İzleyicisi üzerinde bir etkisi vardır: tüm oturumları ve işleri süre (dolayısıyla, oturumlar ve işleri veri bloğu eşik görünmeyebilir daha kısa) veri bloğu eşik yuvarlanır. Bir veri bloğu eşikten artık 30000 (30s) kullanılması önerilir.

### <a name="session-timeout"></a>Oturum zaman aşımı
Varsayılan olarak, bir oturum sona erdi 10'luk (oluşan genellikle ev veya Geri tuşuna basarak, telefon boşta ayarlayarak veya başka bir uygulamaya atlayarak), son etkinlik sonunda değil. Bu (ki çekme bir görüntüyü oluşturan bağlandığınızda durum kontrol edebilirsiniz bir bildirim, vs.) her kullanıcı çıkış saat ve uygulamaya çok hızlı bir şekilde dönmek oturum bölme önlemek için yapılır. Bu parametre değiştirmek isteyebilirsiniz. Bunu yapmak için bu ekleyin (arasında `<application>` ve `</application>` etiketleri):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Günlük bildirimini devre dışı bırak
### <a name="using-a-method-call"></a>Yöntem çağrısı kullanma
Engagement'ın günlükleri göndermek durdurmak istiyorsanız, çağırabilirsiniz:

            EngagementAgent.getInstance(context).setEnabled(false);

Bu çağrı kalıcıdır: paylaşılan tercihleri dosyasını kullanır.

Bu işlev çağırdığınızda katılım etkinse durdurmak hizmet için 1 dakika sürebilir. Hizmeti her başlatıldığında başlatın olmaz ancak uygulamayı başlatın.

Yeniden ile aynı işlevini çağırarak raporlama günlüğü etkinleştirebilirsiniz `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Kendi tümleştirme`PreferenceActivity`
Bu işlevi çağırmak yerine, ayrıca bu ayarı doğrudan var olan tümleştirebilirsiniz `PreferenceActivity`.

Tercihler dosyanız (istenen moduyla) kullanmak için katılım yapılandırabileceğiniz `AndroidManifest.xml` ile dosya `application meta-data`:

* `engagement:agent:settings:name` Anahtar paylaşılan tercihleri dosya adını tanımlamak için kullanılır.
* `engagement:agent:settings:mode` Anahtar paylaşılan tercihleri dosya modunu tanımlamak için kullanıldığında, aynı modunda olarak kullanması gereken, `PreferenceActivity`. Mod bir sayı olarak geçirilmelidir: kodunuzda sabit bayrakları birlikte kullanıyorsanız, toplam değerini denetleyin.

Her zaman engagement kullanmayı `engagement:key` boolean anahtarı bu ayarı yönetmek için Tercihler dosya içinde.

Aşağıdaki örnekte `AndroidManifest.xml` varsayılan değerleri gösterir:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Ekleyebileceğiniz sonra bir `CheckBoxPreference` , tercih yerleşiminde aşağıdakine benzer:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
