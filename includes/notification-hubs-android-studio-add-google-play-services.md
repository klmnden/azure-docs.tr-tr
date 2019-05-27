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
ms.openlocfilehash: fb27386881e89cd9056d0efccb7d3c301867bd83
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156860"
---
1. İçinde **Android Studio**seçin **Araçları** seçin ve menü **SDK Yöneticisi**. 
2. Projenizde kullanılan Android SDK'sının hedef sürümü seçin ve seçin **Paket ayrıntılarını göster**. 

    ![Android SDK Yöneticisi - select hedef sürümü](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Seçin **Google API'leri**, zaten yüklü değilse.

    ![Android SDK Yöneticisi - Google API'leri seçili](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Geçiş **SDK Tools** sekmesi. Google Play Hizmeti'ni zaten yüklemediyseniz seçin **Google Play Hizmetleri** aşağıdaki görüntüde gösterildiği gibi. Ardından, yüklemek için **Uygula** ’ya tıklayın. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Yöneticisi - seçili olan Google Play Hizmetleri](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Görürseniz **değişikliği onaylayın** iletişim kutusunda **Tamam**. Bileşen yükleyici istenen bileşenleri yükler. Seçin **son** bileşenler yüklendikten sonra.
4. Seçin **Tamam** kapatmak için **yeni projeler için ayarları** iletişim kutusu.  
5. Açık `build.gradle` dosyası **uygulama** dizini altındaki şu satırı ekleyin `dependencies`. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Seçin **Şimdi Eşitle** araç çubuğunda simgesi.

    ![Gradle ile eşitleme](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. **AndroidManifest.xml** dosyasını açın ve bu etiketi *uygulama* etiketine ekleyin.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
