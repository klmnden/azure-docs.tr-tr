---
title: "Azure Mobile Engagement Android SDK'sı için raporlama konumu"
description: "Azure Mobile Engagement Android SDK için raporlama konumu yapılandırmayı açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 777d5719cce505b55dfb61c91dcac7e713b077a9
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Azure Mobile Engagement Android SDK'sı için raporlama konumu
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Bu konuda, Android uygulamanız için raporlama konumu yapmak açıklar.

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Konum raporlama
Konumları bildirilmesini istiyorsanız, birkaç satırlı yapılandırma eklemeniz gerekir (arasında `<application>` ve `</application>` etiketleri).

### <a name="lazy-area-location-reporting"></a>Yavaş alan konumu raporlama
Yavaş alan konumu raporlama ülke, bölgeye ve yere göre cihazlarla ilişkilendirilmiş raporlamayı etkinleştirir. Bu tür konumu raporlama yalnızca ağ konumlarını (hücre kimliği veya WIFI göre) kullanır. Aygıt alanına oturum başına en fazla bir kez bildirilir. GPS hiçbir zaman kullanılmaz ve bu nedenle bu tür bir konum rapor düşük pil gücüyle etkisi.

Bildirilen alanları kullanıcıları, oturumlar, olayları ve hataları ile ilgili coğrafi istatistikleri hesaplamak için kullanılır. Reach kampanyaları ölçütü olarak de kullanılabilir.

Bu yordamda daha önce bahsedilen yapılandırmayı kullanarak raporlama yavaş alan konumu etkinleştirin:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ayrıca bir konuma izni belirtmeniz gerekir. Bu kodu kullanır ``COARSE`` izin:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Uygulamanızı gerektiriyorsa, kullanabileceğiniz ``ACCESS_FINE_LOCATION`` yerine.

### <a name="real-time-location-reporting"></a>Gerçek zamanlı konum raporlama
Gerçek zamanlı konum raporlama enlem ve boylam cihazlarla ilişkilendirilmiş raporlamayı etkinleştirir. Varsayılan olarak, bu tür konumu raporlama yalnızca hücre kimliği veya WIFI göre ağ konumlarını kullanır. Raporlama uygulama ön planda (örneğin, bir oturum sırasında) çalıştığında etkindir.

Gerçek zamanlı konumlarının *değil* istatistikleri hesaplamak için kullanılır. Gerçek zamanlı coğrafi yalıtma kullanımına izin vermek için kendi tek amacı olan \<Reach-İzleyici-bölge sınırlaması\> Reach kampanyaları ölçütü.

Gerçek zamanlı konum raporlama etkinleştirmek için katılım bağlantı dizesi Başlatıcısı etkinliğinde ayarladığınız bir kod satırını ekleyin. Sonuç aşağıdaki gibi görünür:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>Raporlama GPS dayalı
Varsayılan olarak, gerçek zamanlı konum raporlama ağ tabanlı konum yalnızca kullanır. Daha kesin, GPS tabanlı konumlarda kullanımını etkinleştirmek için yapılandırma nesnesi kullanın:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Arka plan raporlama
Uygulama ön planda (örneğin, bir oturum sırasında) çalıştırdığında, varsayılan olarak, gerçek zamanlı konum raporlama yalnızca etkindir. Ayrıca raporlama arka planda etkinleştirmek için bu yapılandırma nesnesini kullanın:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Arka planda uygulamayı çalıştırdığında, yalnızca ağ tabanlı konumlar raporlanır, GPS etkin olsa bile.
> 
> 

Kullanıcı cihazını yeniden başlatılırsa, arka plan konum rapor durdurulur. Önyükleme sırasında otomatik olarak yeniden yapmak için bu kodu ekleyin.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Ayrıca aşağıdaki izni eksikse eklemeniz gerekir:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M izinleri
Android M ile başlayarak, bazı izinler çalışma zamanında yönetilir ve kullanıcı onayı gerekir.

Android API düzeyi 23 hedefliyorsanız, çalışma zamanı izinleri yeni uygulama yüklemeleri için varsayılan olarak kapalıdır. Aksi halde bunlar varsayılan olarak etkinleştirilir.

Etkinleştirebilir/aygıt ayarları menüsünden bu izinleri devre dışı bırakabilir. Sistem menüsünden izinleri kapatma sistem davranıştır ve anında iletme arka planda alma yeteneğini üzerinde hiçbir etkisi olmaz uygulamasının arka plan işlemleri sonlandırır.

Mobile Engagement konumu raporlama bağlamında, çalışma zamanında onayı iste izinler şunlardır:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

İzinleri standart sistem iletişim kutusunu kullanarak kullanıcıdan isteyin. Kullanıcı onaylarsa, söyleyin ``EngagementAgent`` hesaba bu değişikliği gerçek zamanlı gerçekleştirilecek. Aksi takdirde değişikliği kullanıcı uygulamayı başlatır sonraki zaman işlenir.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
