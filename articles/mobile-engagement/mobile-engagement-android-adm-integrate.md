---
title: "Azure Mobile Engagement Android SDK tümleştirmesi"
description: "En son güncelleştirmeler ve yordamlar için Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 43987962ea2b7b825b88643d18b4db65f1f1670e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-integrate-adm-with-engagement"></a>ADM katılım ile tümleştirme
> [!IMPORTANT]
> Tümleştirme katılım nasıl Android belge üzerinde bu kılavuzu izlemeden önce açıklanan tümleştirme yordamı izlemeniz gerekir.
> 
> Bu belge, yalnızca, zaten Reach modülünün ve Amazon aygıtları göndermek için plan tümleşik yararlıdır. Uygulamanızda Reach kampanyaları tümleştirmek için önce okuyun nasıl android'de Engagement Reach tümleştirmek için.
> 
> 

## <a name="introduction"></a>Giriş
ADM tümleştirme uygulamanızı Amazon Android cihazları hedeflerken edilmesini sağlar.

SDK her zaman gönderilen ADM yüklerini içeren `azme` veri nesnesindeki anahtar. Böylece, uygulamanızda başka bir amaçla ADM kullanırsanız, bu anahtarı temel iter filtreleyebilirsiniz.

> [!IMPORTANT]
> Android 4.0.3 çalıştıran yalnızca Amazon Kindle cihazlar veya Amazon Device Messaging; yukarıdaki desteklenir Ancak, bu kod diğer aygıtlarda güvenli bir şekilde tümleştirilebilir.
> 
> 

## <a name="sign-up-to-adm"></a>ADM için kaydolun
Yapmadıysanız, Amazon hesabınızdaki ADM etkinleştirmeniz gerekir.

Yordamı, ayrıntılı: [ <https://developer.amazon.com/sdk/adm/credentials.html>].

Yordamı tamamladığınızda, şunları alırsınız:

* Aygıtlarınızı itme yapabilmek katılım için OAuth kimlik (istemci kimliği ve istemci gizli anahtarı).
* Uygulamanıza tümleşik bir API anahtarı.

## <a name="sdk-integration"></a>SDK tümleştirmesi
### <a name="managing-device-registrations"></a>Cihaz kayıtlarını yönetme
Her bir cihaz kayıt komutu ADM sunuculara göndermesi gerekir, aksi halde bunlar ulaşılamıyor.

Zaten kullanıyorsanız [ADM istemci Kitaplığı]ve zaten [ADM tümleşik] için android-sdk-adm-alma doğrudan gidebilirsiniz.

ADM henüz değil bütünleştirdiyseniz, katılım uygulamanızda etkinleştirmek için daha basit bir yol vardır:

Düzenleme, `AndroidManifest.xml` dosyası:

* Amazon ad ekleme dosya şu şekilde başlamanız gerekir:
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* İçinde `<application/>` etiketi, bu bölümde ekleyin:
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* Amazon etiket ekledikten sonra proje derleme hedefi Android 2.1 ise bir derleme hatası olabilir. Kullanmak zorunda bir **Android 2.1 +** hedef yapı (endişelenmeyin, hala sahip bir `minSdkVersion` 4'e ayarlayın).
* ADM API anahtarı izleyerek bir varlık tümleştirmek [bu yordamı].

Sonraki bölümlerde yönergeleri izleyin.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Katılım itme hizmetine kayıt kimliği iletişim kurmak ve bildirimlerin
Katılım itme hizmetini cihaza kayıt kimliğini iletişim kurmak ve kendi bildirimleri almak için aşağıdakileri ekleyin, `AndroidManifest.xml` içinde dosya `<application/>` (ADM katılım olmadan kullanmak olsa bile) etiketi:

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Aşağıdaki izinlere sahip olmak, `AndroidManifest.xml` (önce `</application>` etiketi).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a>GRANT katılım OAuth kimlik bilgileri
OAuth kimlik bilgilerinizi (istemci kimliği ve istemci gizli anahtarı) Engagement portalında gönderin.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM istemci Kitaplığı]:https://developer.amazon.com/sdk/adm/setup.html
[ADM tümleşik]:https://developer.amazon.com/sdk/adm/integrating-app.html
[bu yordamı]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
