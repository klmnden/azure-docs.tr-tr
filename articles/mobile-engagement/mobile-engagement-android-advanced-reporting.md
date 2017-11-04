---
title: "Gelişmiş Azure Mobile Engagement Android SDK için raporlama seçenekleri"
description: "Analiz için Azure Mobile Engagement Android SDK yakalamak için Gelişmiş raporlama yapmak açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 2a1445afa2c2fca1a31ad9c012b9c8a917ebf65c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a>Gelişmiş Android katılım ile raporlama
> [!div class="op_single_selector"]
> * [Evrensel Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Bu konuda, Android uygulamanızdaki ek raporlama senaryolar açıklanmaktadır. Bu seçenekler oluşturduğunuz uygulama uygulayabileceğiniz [Başlarken](mobile-engagement-android-get-started.md) Öğreticisi.

## <a name="prerequisites"></a>Ön koşullar
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Tamamlandı öğretici kasıtlı olarak doğrudan ve basit ancak Gelişmiş seçebileceğiniz seçenekleri.

## <a name="modifying-your-activity-classes"></a>Değiştirme, `Activity` sınıfları
İçinde [Başlarken Öğreticisi](mobile-engagement-android-get-started.md), tüm yapmak için var olan yapmak için `*Activity` alt sınıfların devralır denk gelen `Engagement*Activity` sınıfları. Örneğin, eski etkinliklerinizi Genişletilmiş `ListActivity`, genişletme yapacağı `EngagementListActivity`.

> [!IMPORTANT]
> Kullanırken `EngagementListActivity` veya `EngagementExpandableListActivity`, aşağıdakilerden emin olun çağrı `requestWindowFeature(...);` çağırmadan önce yapılan `super.onCreate(...);`, aksi halde bir kilitlenme oluşur.
> 
> 

Bu sınıfları bulabilirsiniz `src` klasörünü ve bunları projenize kopyalayabilirsiniz. Ayrıca, sınıflardır **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatif yöntem: çağrı `startActivity()` ve `endActivity()` el ile
Olamaz ya da tekrar etmek istiyor musunuz, `Activity` sınıfları, bunun yerine başlangıç ve çağırarak etkinliklerinizi bitiş `EngagementAgent`'s doğrudan yöntemleri.

> [!IMPORTANT]
> Android SDK hiçbir zaman çağırır `endActivity()` uygulama kapalı olduğunda bile yöntemi (Android, uygulamaları hiçbir zaman kapalı). Bu nedenle, olan *yüksek oranda* çağırmak için önerilen `startActivity()` yönteminde `onResume` , geri çağırma *tüm* , etkinlikler ve `endActivity()` yönteminde `onPause()` , geri çağırma *Tüm* etkinliklerinizi. Bu oturumlar sızdırılmaz emin olmak için tek yoludur. Bir oturum sızmasını varsa (bir oturum bekleyen olduğu sürece hizmet bağlı kalır bu yana) katılım hizmet hiçbir zaman katılım arka ucundan bağlantısını keser.
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

Bu örnek benzer `EngagementActivity` sınıfı ve kaynak kodu sağlanır türevleri `src` klasör.

## <a name="using-applicationoncreate"></a>Application.onCreate() kullanma
Yerleştirdiğiniz içinde herhangi bir kod `Application.onCreate()` ve başka bir uygulamada geri aramalar katılım hizmeti dahil olmak üzere tüm uygulama işlemleri için çalıştırılır. Gereksiz bellek ayırma ve iş parçacıkları Engagement'ın işlemi veya yinelenen yayın alıcıları ya da Hizmetleri gibi istenmeyen yan etkileri olabilir.

Geçersiz kılarsanız `Application.onCreate()`, aşağıdaki kod parçacığını başında ekleme öneririz, `Application.onCreate()` işlevi:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Aynı şeyi yapmak `Application.onTerminate()`, `Application.onLowMemory()`, ve `Application.onConfigurationChanged(...)`.

Ayrıca genişletebilirsiniz `EngagementApplication` genişletme yerine `Application`: geri çağırma `Application.onCreate()` işlem denetimi yapar ve çağrıları `Application.onApplicationProcessCreate()` yalnızca geçerli işlem katılım hizmetini barındıran bir durumda değilse, aynı kuralları için diğer uygulama geri aramalar.

## <a name="tags-in-the-androidmanifestxml-file"></a>AndroidManifest.xml dosyasında etiketleri
AndroidManifest.xml dosyasında hizmet etiketinde `android:label` özniteliği telefonlarını "Hizmetleri çalışır" ekranında son kullanıcılara göründüğü gibi katılım hizmetin adını seçmenize olanak sağlar. Bu özniteliği ayarlamak önerilen `"<Your application name>Service"` (örneğin, `"AcmeFunGameService"`).

Belirtme `android:process` özniteliği sağlar katılım hizmet (uygulamanızın ana/kullanıcı Arabirimi iş parçacığı potansiyel olarak daha az yanıt getirir katılım aynı işlem içinde çalışan) kendi işleminde çalışır.

## <a name="building-with-proguard"></a>ProGuard ile oluşturma
Uygulama paketinizi ProGuard ile yapılandırdıysanız, bazı sınıfları tutmanız gerekir. Aşağıdaki yapılandırma parçacığını kullanabilirsiniz:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
