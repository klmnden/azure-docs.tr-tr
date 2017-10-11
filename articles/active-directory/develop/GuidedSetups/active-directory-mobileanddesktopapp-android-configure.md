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
ms.openlocfilehash: 945b09ccdb7537987da33d32d94a3ccacd829ffd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a>(Hızlı) uygulama oluşturma
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Kurulum destekli seçeneğinin işaretli olduğundan emin olun
4.  Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Uygulama kayıt bilgilerinizi çözümünüze (Gelişmiş) ekleyin
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetmek için
2. Uygulamanız ve e-posta için bir ad girin 
3. Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4. Tıklatın `Add Platform`seçeneğini belirleyip `Native Application` ve kaydetme ulaştı.
5.  Açık `MainActivity` (altında `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Değiştir *[uygulama kimliği buraya girin]* başlayarak satırında `final static String CLIENT_ID` yalnızca kayıtlı uygulama kimliği:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Açık `AndroidManifest.xml` (altında `app`  >  `manifests`) aşağıdaki etkinlik eklemek `manifest\application` düğümü. Bu kayıtları bir `BrowserTabActivity` uygulamanızın kimlik doğrulamasını tamamladıktan sonra devam etmek işletim sistemi izin vermek için:
</li>
</ol>

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
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
İçinde `BrowserTabActivity`, yerine `[Enter the application Id here]` uygulama kimliğiyle
</li>
</ol>
