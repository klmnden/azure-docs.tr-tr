---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 01/04/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f00ca7ddf44a9d5b850cd47520970a0396a0c1b5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453105"
---
1. Android Studio’nun araç çubuğundaki simgeye tıklayarak ya da menüde **Araçlar** > **Android** > **SDK Manager** ’a tıklayarak Android SDK Manager’ı açın. Projenizde kullanılan hedef Android SDK sürümünü bulup, **Paket Ayrıntılarını Göster**'e tıklayarak bunu açın ve henüz yüklenmemişse **Google API'ler**'i seçin.
2. **SDK Araçları** sekmesine tıklayın. Google Play Hizmeti’ni zaten yüklemediyseniz **Google Play Hizmetleri** ’ne, aşağıda gösterildiği gibi tıklayın. Ardından, yüklemek için **Uygula** ’ya tıklayın. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Açık `build.gradle` uygulama dizinindeki dosya.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Bu satırı altında ekleyin `dependencies`:

    ```text
    compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Araç çubuğundaki **Projeyi Gradle Dosyalarıyla Eşitle** simgesine tıklayın.
6. **AndroidManifest.xml** dosyasını açın ve bu etiketi *uygulama* etiketine ekleyin.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
