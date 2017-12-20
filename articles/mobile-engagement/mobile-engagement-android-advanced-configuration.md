---
title: "Gelişmiş yapılandırma için Azure Mobile Engagement Android SDK"
description: "Android bildirim Azure Mobile Engagement Android SDK ile de dahil olmak üzere gelişmiş yapılandırma seçenekleri açıklanmaktadır"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 0301f71c76872714aa1bf727a6c21dd7a63db036
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Gelişmiş yapılandırma için Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
>
>

Bu yordam, Azure Mobile Engagement Android uygulamaları için çeşitli yapılandırma seçeneklerini yapılandırmak açıklar.

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>İzin gereksinimleri
Bazı seçenekler tümü burada başvuru ve satır içi belirli özelliği için listelenen belirli izinler gerektirir. Bu izinleri projenizin AndroidManifest.xml için hemen önce veya sonra eklemek `<application>` etiketi.

Burada aşağıdaki tablodan uygun izni doldurmanız aşağıdaki gibi aramak izni kod gerekir.

    <uses-permission android:name="android.permission.[specific permission]"/>


| İzin | Kullanıldığında |
| --- | --- |
| INTERNET |Gereklidir. Temel raporlama için |
| ACCESS_NETWORK_STATE |Gereklidir. Temel raporlama için |
| RECEIVE_BOOT_COMPLETED |Gereklidir. Cihaz yeniden başlatıldıktan sonra bildirimleri center göstermek için |
| WAKE_LOCK |Önerilir. WiFi kullanırken veya ekran kapalı olduğunda verilerin toplanmasını sağlar |
| TİTRET |İsteğe bağlı. Bildirim alındığında titreşimi sağlar |
| DOWNLOAD_WITHOUT_NOTIFICATION |İsteğe bağlı. Android büyük resmi bildirim sağlar |
| WRITE_EXTERNAL_STORAGE |İsteğe bağlı. Android büyük resmi bildirim sağlar |
| ACCESS_COARSE_LOCATION |İsteğe bağlı. Gerçek zamanlı konum raporlama sağlar |
| ACCESS_FINE_LOCATION |İsteğe bağlı. GPS tabanlı konum raporlama sağlar |

Android M ile başlayan [bazı izinler çalışma zamanında yönetilen](mobile-engagement-android-location-reporting.md#android-m-permissions).

Zaten kullanıyorsanız, ``ACCESS_FINE_LOCATION``, ayrıca kullanmanız gerekmez sonra ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android bildirim yapılandırma seçenekleri
### <a name="crash-report"></a>Kilitlenme raporu
Kilitlenme raporları devre dışı bırakmak için bu kodu arasında ekleyin `<application>` ve `</application>` etiketler:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Eşik veri bloğu
Varsayılan olarak, katılım hizmet raporları gerçek zamanlı olarak günlüğe kaydeder. Uygulama rapor günlüklerinizi sık farklılık, günlükleri arabellek ve üzerlerinde tümünü bir defada bir normal zaman temel ("modu veri bloğu" olarak adlandırılır) bildirmek için daha iyi olur. Bunu yapmak için bu kod arasında eklemek `<application>` ve `</application>` etiketler:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Veri bloğu modu biraz pil ömrünün artırır ancak Engagement İzleyicisi üzerinde bir etkisi vardır: tüm oturumları ve işleri süre (dolayısıyla, oturumlar ve işleri veri bloğu eşik görünmeyebilir daha kısa) veri bloğu eşik yuvarlanır. Veri bloğu eşiği 30000 (30s) uzun olmalıdır.

### <a name="session-timeout"></a>Oturum zaman aşımı
 Tuşuna basarak bir etkinlik sonlandırabilirsiniz **giriş** veya **geri** ayarlayarak telefon boşta veya başka bir uygulamaya atlayarak anahtar. Varsayılan olarak, son etkinlik sonunda on saniye oturumu sona erer. Bu, görüntüyü oluşturan kullanıcının Çekmeleri bağlandığınızda durum denetler bildirim, vb. kullanıcı çıkar ve hızlı bir şekilde, uygulamaya döndürür her zaman bir oturum bölme önler. Bu parametre değiştirmek isteyebilirsiniz. Bunu yapmak için bu kod arasında eklemek `<application>` ve `</application>` etiketler:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Günlük bildirimini devre dışı bırak
### <a name="using-a-method-call"></a>Yöntem çağrısı kullanma
Engagement'ın günlükleri göndermek durdurmak istiyorsanız, çağırabilirsiniz:

    EngagementAgent.getInstance(context).setEnabled(false);

Bu çağrı kalıcıdır: paylaşılan tercihleri dosyasını kullanır.

Bu işlev çağırdığınızda katılım etkinse durdurmak hizmet için bir dakika sürebilir. Hizmeti her başlatıldığında başlatın olmaz ancak uygulamayı başlatın.

Yeniden ile aynı işlevini çağırarak raporlama günlüğü etkinleştirebilirsiniz `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Kendi tümleştirme`PreferenceActivity`
Bu işlevi çağırmak yerine, ayrıca bu ayarı doğrudan var olan tümleştirebilirsiniz `PreferenceActivity`.

Tercihler dosyanız (istenen moduyla) kullanmak için katılım yapılandırabileceğiniz `AndroidManifest.xml` ile dosya `application meta-data`:

* `engagement:agent:settings:name` Anahtar paylaşılan tercihleri dosya adını tanımlamak için kullanılır.
* `engagement:agent:settings:mode` Anahtar paylaşılan tercihleri dosya modunu tanımlamak için kullanılır. Aynı modunda olarak kullanmak, `PreferenceActivity`. Mod bir sayı olarak geçirilmelidir: kodunuzda sabit bayrakları birlikte kullanıyorsanız, toplam değerini denetleyin.

Her zaman katılım kullanan `engagement:key` boolean anahtarı bu ayarı yönetmek için Tercihler dosya içinde.

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
