---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e047ba57a61d2f327544ec795f640f5066962f6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509932"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. Üzerinde **Firebase'i Android uygulamanıza ekleyin** sayfasında, aşağıdaki adımları uygulayın: 
1. 
    1. İçin **Android paketi adı**, paketiniz için bir ad girin. Örneğin: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Paket adı belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)

2. Seçin **Register app**. 
1. 
1. Seçin **indirme google-services.json**. Ardından dosyasına kaydedin **uygulama** seçin ve proje klasöründe **sonraki**. 

    ![Google-services.json indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)

6. **İleri**’yi seçin. 
7. Seçin **bu adımı atlayın**. 

    ![Son adımı atlayın](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)

8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

4. İndirmediyseniz, **google-services.json** yapabileceğiniz dosyası, bu sayfada indirme. 

1. Geçiş **Cloud Messaging** en üstteki sekmedeki. 

1. Kopyalayıp kaydedin **eski sunucu anahtarı** daha sonra kullanmak için. Bildirim hub'ınızı yapılandırmak için bu değeri kullanın.
