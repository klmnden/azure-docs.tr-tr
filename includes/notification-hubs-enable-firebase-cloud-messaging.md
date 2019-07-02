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
ms.openlocfilehash: fef6122eceda213fb6353ada53033d0d1e27fd7e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509136"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Üzerinde **Firebase'i Android uygulamanıza ekleyin** sayfasında, aşağıdaki adımları uygulayın: 
    1. İçin **Android paketi adı**, değerini kopyalayın, **ApplicationId** uygulamanızın build.gradle dosyasına. Bu örnekte, bunun `com.fabrikam.fcmtutorial1app`. 

        ![Paket adı belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Seçin **Register app**. 
4. Seçin **indirme google-services.json**, dosyaya Kaydet **uygulama** klasörü seçin ve proje **sonraki**. 

    ![Google-services.json indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Aşağıdaki **yapılandırma değişiklikleri** Android Studio'da projenize. 
    1.  Proje düzeyi build.gradle dosyanıza (&lt;proje&gt;/build.gradle), şu deyimi ekleyin **bağımlılıkları** bölümü. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. Uygulama düzeyinde build.gradle dosyanıza (&lt;proje&gt;/&lt;uygulama-module&gt;/build.gradle), şu deyimi ekleyin **bağımlılıkları** bölümü. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.1'
        ```

    3. Aşağıdaki satırı bağımlılıklar bölümünden sonra uygulama düzeyinde build.gradle dosyanın sonuna ekleyin. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
    4. Seçin **Şimdi Eşitle** araç. 
 
        ![Build.gradle yapılandırma değişiklikleri](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. **İleri**’yi seçin. 
7. Seçin **bu adımı atlayın**. 

    ![Son adımı atlayın](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Google-services.json dosyasına indirilen yapmadıysanız, **uygulama** yapabileceğiniz klasör Android Studio projenizin benzeri bu sayfa. 
5. Geçiş **Cloud Messaging** en üstteki sekmedeki. 
6. Kopyalayıp kaydedin **sunucu anahtarı** daha sonra kullanmak için. Hub'ınızı yapılandırmak için bu değeri kullanın.
