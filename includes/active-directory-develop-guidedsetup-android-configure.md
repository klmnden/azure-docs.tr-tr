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
ms.openlocfilehash: da4944cc95750505e0099b7a987b78e132910460
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58125000"
---
## <a name="register-your-application"></a>Uygulamanızı kaydetme

Uygulamanızı iki yoldan biriyle sonraki iki bölümde açıklandığı gibi kaydedebilirsiniz.

### <a name="option-1-express"></a>1. seçenek: Hızlı

1. [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)'na gidin.
2. İçinde **uygulama adı**, uygulamanız için bir ad girin.
3. Emin **destekli Kurulum** onay kutusunu seçili ve ardından **Oluştur**.
4. Uygulama Kimliği alma yönergeleri izleyin ve kodunuzun yapıştırın.

### <a name="option-2-advanced"></a>2. seçenek: Gelişmiş

1. [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app)'na gidin.
2. **Uygulama Adı** kutusuna uygulamanız için bir ad girin.
3. **Destekli Kurulum** onay kutusunun işaretli olmadığından emin olun ve **Oluştur**’u seçin.
4. **Platform Ekle**’yi, **Yerel Uygulama**’yı ve **Kaydet**’i seçin.
5. Altında **uygulama** > **java** > **{konak}. { ad alanı}** açın `MainActivity`. 
6. Değiştirin *[uygulama kimliği buraya girin]* uygulamanızla / istemci kimliği:

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
