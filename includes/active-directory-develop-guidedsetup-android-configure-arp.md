---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: b8f961ad3fe4550b915253746d0f4f677c593a8c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214490"
---
## <a name="add-the-applications-registration-to-your-code"></a>Uygulamanın kayıt için kodunuzu ekleyin

Bu adımda, uygulamayı eklemeniz gereken / istemci kimliği, projenize.

1. Açık `MainActivity` (altında `app`  >  `java`  >  *`{host}.{namespace}`*)
2. `final static String CLIENT_ID` ile başlayan satırın yerine aşağıdakini koyun:
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
