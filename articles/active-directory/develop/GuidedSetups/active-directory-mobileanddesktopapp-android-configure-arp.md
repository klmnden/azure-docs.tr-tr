---
title: "Azure AD v2 Android alma başlatıldı - yapılandırma | Microsoft Docs"
description: "Nasıl bir Android uygulaması bir erişim belirteci alın ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren API'larını çağırma"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: c09937582118ebcc5b8cbc1f43a0a2019f2f7a89
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgileri ekleme

Bu adımda, istemci kimliği projenize eklemeniz gerekir.

1.  Açık `MainActivity` (altında `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  İle başlayan satırı değiştirin `final static String CLIENT_ID` ile:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Aç:`app` > `manifests` > `AndroidManifest.xml`
4. Aşağıdaki etkinlik eklemek `manifest\application` düğümü. Bu kayıt bir `BrowserTabActivity` uygulamanızın kimlik doğrulamasını tamamladıktan sonra devam etmek işletim sistemi izin vermek için:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

### <a name="what-is-next"></a>Sonraki nedir

[Test ve doğrulama](active-directory-mobileanddesktopapp-android-test.md)
