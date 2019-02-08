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
ms.openlocfilehash: e6b949824ec5da60c5e2485be830e61d156a11ff
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55830704"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Üzerinde **Firebase'i Android uygulamanıza ekleyin** sayfasında, aşağıdaki adımları uygulayın: 
    1. İçin **Android paketi adı**, değerini kopyalayın, **ApplicationId** uygulamanızın **build.gradle** dosya. Bu örnekte, bunun `com.fabrikam.fcmtutorial1app`. 

        ![Paket adı belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Seçin **Register app**. 
4. Seçin **indirme google-services.json**, dosyaya Kaydet **uygulama** klasörü seçin ve proje **sonraki**. 

    ![Google-services.json indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Aşağıdaki **yapılandırma değişiklikleri** Android Studio'da projenize. 
    1.  İçinde **proje düzeyi build.gradle** dosyası (&lt;proje&gt;/build.gradle), şu deyimi ekleyin **bağımlılıkları** bölümü. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. İçinde **uygulama düzeyinde build.gradle** dosyası (&lt;proje&gt;/&lt;uygulama-module&gt;/build.gradle), şu deyimi ekleyin  **bağımlılıkları** bölümü. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.1'
        ```

    3. Sonuna aşağıdaki satırı ekleyin **uygulama düzeyinde build.gradle** dependenices bölümü sonra dosya. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
 
        ![Build.gradle yapılandırma değişiklikleri](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. Seçin **sonraki** sayfasında. 
7. Seçin **bu adımı atlayın** sayfasında. 

    ![Son adımı atlayın](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)1. 
8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. İndirmediyseniz, **google-services.json** doyasını **uygulama** yapabileceğiniz, Android Studio proejct klasörü, bu sayfada benzeri. 
5. Geçiş **Cloud Messaging** en üstteki sekmedeki. 
6. Kopyalayıp kaydedin **sunucu anahtarı** daha sonra kullanmak için. Bildirim hub'ınızı yapılandırmak için bu değeri kullanın.
