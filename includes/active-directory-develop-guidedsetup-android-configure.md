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
ms.openlocfilehash: f0e584a4a4a54fc04b5539b56d5c901bfaa42bcc
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293749"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme
Uygulamanızı iki yoldan biriyle sonraki iki bölümde açıklandığı gibi kaydedebilirsiniz.

### <a name="option-1-express"></a>Seçenek 1: Express
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  İçinde **uygulama adı**, uygulamanız için bir ad girin.

3. Emin **destekli Kurulum** onay kutusunu seçili ve ardından **Oluştur**.

4. Uygulama Kimliği alma yönergeleri izleyin ve kodunuzun yapıştırın.

### <a name="option-2-advanced"></a>Seçenek 2: Gelişmiş 
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).
2. İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin. 

3. Emin **destekli Kurulum** onay kutusunun temizlenmiş ve ardından **Oluştur**.

4. Seçin **Platform Ekle**seçin **yerel uygulama**ve ardından **Kaydet**.

5. Altında **uygulama** > **java** > **{konak}. { ad alanı}** açın `MainActivity`. 

6.  Değiştirin *[uygulama kimliği buraya girin]* uygulamanızla / istemci kimliği:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Altında **uygulama** > **bildirimlerini**açın *AndroidManifest.xml* dosya.

8. İçinde `manifest\application`, aşağıdaki etkinliği ekleyin. `BrowserTabActivity` Microsoft kimlik doğrulaması tamamlandıktan sonra uygulamanıza geri çağırmaya izin veren etkinlik:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
9. İçinde `BrowserTabActivity`, değiştirin `[Enter the application Id here]` uygulamayla / istemci kimliği
