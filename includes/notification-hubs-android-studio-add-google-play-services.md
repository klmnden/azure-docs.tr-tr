---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/05/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 540c9775ae56798ce2d2d87fed4924244c31412f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33835789"
---
1. Android Studio’nun araç çubuğundaki simgeye tıklayarak ya da menüde **Araçlar** > **Android** > **SDK Manager** ’a tıklayarak Android SDK Manager’ı açın. Projenizde kullanılan hedef Android SDK sürümünü bulup, **Paket Ayrıntılarını Göster**'e tıklayarak bunu açın ve henüz yüklenmemişse **Google API'ler**'i seçin.
2. **SDK Araçları** sekmesine tıklayın. Google Play Hizmeti’ni zaten yüklemediyseniz **Google Play Hizmetleri** ’ne, aşağıda gösterildiği gibi tıklayın. Ardından, yüklemek için **Uygula** ’ya tıklayın. 
   
    SDK yolunun sonraki bir adım için olduğunu unutmayın. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Uygulama dizinindeki **build.gradle** dosyasını açın.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. *Bağımlılıklar* ’ın altına bu satırı ekleyin: 
    
    ```java
        compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Araç çubuğundaki **Projeyi Gradle Dosyalarıyla Eşitle** simgesine tıklayın.
6. **AndroidManifest.xml** dosyasını açın ve bu etiketi *uygulama* etiketine ekleyin.
   
    ```java
    <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
    ```

