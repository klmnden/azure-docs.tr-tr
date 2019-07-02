---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd202c98ed605209f5600965ecdb6c0b4c03c17e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509120"
---
1. Android Studio'da **Araçları** menüsünü ve ardından **SDK Yöneticisi**. 
2. Projenizde kullanılan Android SDK'sının hedef sürümü seçin. Ardından **Paket ayrıntılarını göster**. 

    ![Android SDK Yöneticisi - select hedef sürümü](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Seçin **Google API'leri**, zaten yüklü değilse.

    ![Android SDK Yöneticisi - Google API'leri seçili](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Geçiş **SDK Tools** sekmesi. Google Play Hizmetleri henüz yüklemediyseniz, seçin **Google Play Hizmetleri** aşağıdaki görüntüde gösterildiği gibi. Ardından **Uygula** yüklemek için. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Yöneticisi - seçili olan Google Play Hizmetleri](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Görürseniz **değişikliği onaylayın** iletişim kutusunda **Tamam**. Bileşen yükleyici istenen bileşenleri yükler. Seçin **son** bileşenler yüklendikten sonra.
4. Seçin **Tamam** kapatmak için **yeni projeler için ayarları** iletişim kutusu.  
5. Build.gradle dosyasını açın **uygulama** dizin ve ardından altındaki şu satırı ekleyin `dependencies`. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Seçin **Şimdi Eşitle** araç çubuğunda simge.

    ![Gradle ile eşitleme](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. AndroidManifest.xml dosyasını açın ve ardından aşağıdaki etiketine ekleyin *uygulama* etiketi.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
