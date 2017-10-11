---
title: "Azure Mobile Engagement Android SDK tümleştirmesi"
description: "En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: 0282abbf44406cac89c13520bc2a4e375817ed1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-gcm-with-mobile-engagement"></a>GCM mobil katılım ile tümleştirme
> [!IMPORTANT]
> Tümleştirme katılım nasıl Android belge üzerinde bu kılavuzu izlemeden önce açıklanan tümleştirme yordamı izlemeniz gerekir.
> 
> Bu belge, yalnızca, zaten Reach modülünün ve Google Play aygıtları göndermek için plan tümleşik yararlıdır. Uygulamanızda Reach kampanyaları tümleştirmek için önce okuyun nasıl android'de Engagement Reach tümleştirmek için.
> 
> 

## <a name="introduction"></a>Giriş
GCM tümleştirme uygulamanızı edilmesini sağlar.

SDK her zaman gönderilen GCM yüklerini içeren `azme` veri nesnesindeki anahtar. Böylece, uygulamanızda başka bir amaçla GCM kullanırsanız, bu anahtarı temel iter filtreleyebilirsiniz.

> [!IMPORTANT]
> Yalnızca Android 2.2 çalıştıran cihazlar veya üzeri yüklü ve Google sahip Google Play sahip etkin arka plan bağlantı GCM; kullanarak gönderilemez Ancak, bu kod cihazlarda (yalnızca hedefleri'ı kullanır) güvenli bir şekilde desteklenmeyen tümleştirebilirsiniz.
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>API anahtarıyla Google Cloud Messaging projesi oluşturma
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>SDK tümleştirmesi
### <a name="managing-device-registrations"></a>Cihaz kayıtlarını yönetme
Her bir cihaz kayıt komutu Google sunucularına göndermesi gerekir, aksi halde bunlar ulaşılamıyor.

Bir cihaz da GCM bildirim (uygulama kaldırılırsa cihaz otomatik olarak kaydettirilmemiş) kaydını kaldırabilirsiniz.

Kullanmazsanız [Google Play SDK] veya, zaten kayıt hedefi kendiniz gönderme, otomatik olarak sizin için kaydedilecek katılım yapabilirsiniz.

Bu ayarı etkinleştirmek için aşağıdakileri ekleyin, `AndroidManifest.xml` içinde dosya `<application/>` etiketi:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Katılım itme hizmetine kayıt kimliği iletişim kurmak ve bildirimlerin
Katılım itme hizmetini cihaza kayıt kimliğini iletişim kurmak ve kendi bildirimleri almak için aşağıdakileri ekleyin, `AndroidManifest.xml` içinde dosya `<application/>` (cihaz kayıtları kendiniz yönettiğiniz olsa bile) etiketi:

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Aşağıdaki izinlere sahip olmak, `AndroidManifest.xml` (sonra `</application>` etiketi).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>GCM API Anahtarınıza Mobile Engagement erişimi verin
İzleyin [bu kılavuzda](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) GCM API anahtarınıza Mobile Engagement erişim vermek için.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
