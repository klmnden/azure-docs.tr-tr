---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 0154aac14168c9d897698a15e31b3124b208db46
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50098123"
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
