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
ms.openlocfilehash: 90ffebf5f3375e94545f82a95f5c240ad845bd94
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60423426"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Üzerinde **Firebase'i Android uygulamanıza ekleyin** sayfasında, aşağıdaki adımları uygulayın: 
    1. İçin **Android paketi adı**, paketiniz için bir ad girin. Örneğin: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Paket adı belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Seçin **Register app**. 
4. Seçin **indirme google-services.json**, dosyaya Kaydet **uygulama** klasörü seçin ve proje **sonraki**. 

    ![Google-services.json indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
6. Seçin **sonraki** sayfasında. 
7. Seçin **bu adımı atlayın** sayfasında. 

    ![Son adımı atlayın](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. İndirmediyseniz, **google-services.json** yapabileceğiniz dosyası, bu sayfada benzeri. 
5. Geçiş **Cloud Messaging** en üstteki sekmedeki. 
6. Kopyalayıp kaydedin **sunucu anahtarı** daha sonra kullanmak için. Bildirim hub'ınızı yapılandırmak için bu değeri kullanın.
