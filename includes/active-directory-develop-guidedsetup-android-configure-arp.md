---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: a2ca767edf1ee3e4ba2d3185d66a871ceff21a6c
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293817"
---
## <a name="add-the-applications-registration-to-your-code"></a>Uygulamanın kayıt için kodunuzu ekleyin

Bu adımda, uygulamayı eklemeniz gereken / istemci kimliği, projenize.

1.  Açık `MainActivity` (altında `app`  >  `java`  >  *`{host}.{namespace}`*)
2.  İle başlayan satırı değiştirin `final static String CLIENT_ID` ile:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Açık: `app` > `manifests` > `AndroidManifest.xml`
4. Aşağıdaki etkinlik eklemek `manifest\application`. `BrowserTabActivity` Microsoft kimlik doğrulama işlemini tamamladıktan sonra uygulamanıza geri arama sağlar:

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

