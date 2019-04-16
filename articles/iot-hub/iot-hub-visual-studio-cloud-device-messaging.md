---
title: Cloud Explorer ile Visual Studio için Mesajlaşma Azure IOT hub'ı bulut cihaz yönetme | Microsoft Docs
description: Cihaz bulut iletilerini izlemek ve bulut, Azure IOT hub'daki cihaz iletilerini göndermek için Visual Studio için cloud Explorer'ı kullanmayı öğrenin.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: xshi
ms.openlocfilehash: ab3c02d7207bca70a90df8aa08c73c1484cd635d
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571320"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>IOT Hub ve cihaz arasında ileti göndermek ve almak için Visual Studio için cloud Explorer'ı kullanın

![Uçtan uca diyagramı](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) Azure kaynaklarınızı görüntüleyin, özelliklerini inceleme ve Visual Studio temel Geliştirici eylemler gerçekleştirmek olanak tanıyan kullanışlı bir Visual Studio uzantısıdır. Bu makalede, Cloud Explorer arasındaki Cihazınızı IOT Hub'ınıza ileti göndermek ve almak için nasıl kullanılacağını odaklanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

Visual Studio için Cloud Explorer CİHAZDAN buluta iletileri izlemeye yönelik ve bulut-cihaz iletilerini göndermek için nasıl kullanılacağını öğreneceksiniz. CİHAZDAN buluta iletileri Cihazınızı toplar ve ardından IOT Hub'ınıza gönderir sensör verilerini olabilir. Bulut-cihaz iletilerini cihazınız için IOT Hub'ınıza gönderdiği komutlar olabilir. Örneğin, cihazınıza bağlı bir LED yanıp.

## <a name="what-you-will-do"></a>Neler yapabileceği

- CİHAZDAN buluta iletileri izlemeye yönelik Visual Studio için cloud Explorer'ı kullanın.
- Bulut-cihaz iletilerini göndermek için Visual Studio için cloud Explorer'ı kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- Etkin bir Azure aboneliği.
- Azure IOT Hub, aboneliğiniz altında.
- Microsoft Visual Studio 2017 güncelleştirme 8 veya üzeri
- Cloud Explorer bileşeni (Azure iş yükü ile varsayılan olarak seçilidir) Visual Studio Yükleyicisi'nden

## <a name="update-cloud-explorer-to-latest-version"></a>Cloud Explorer'ı en son sürüme güncelleştirme

Visual Studio Yükleyicisi'nden Cloud Explorer bileşen yalnızca cihaz-Bulut ve bulut-cihaz iletilerini izlenmesini de destekler. Cihaz veya Bulut için iletileri göndermek için indirme ve en son yükleme [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-iot-hub"></a>IOT Hub'ınıza erişmek için oturum açın

1. Visual Studio'da **Cloud Explorer** penceresinde hesap yönetimi simgesine tıklayın. Cloud Explorer penceresini açabilir **görünümü** > **Cloud Explorer** menüsü.

    ![Hesap Yönetimi](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)


2. Tıklayın **hesaplarını yönetme** bulut Gezgini'nde.

3. Tıklayın **Hesap Ekle...**  Azure'a ilk kez oturum açmak için yeni pencerede.

4. Oturum açtıktan sonra Azure abonelik listesi gösterilir. Tıklayın ve görüntülemek istediğiniz Azure aboneliklerini seçin **Uygula**.

5. Genişletin **aboneliğinizi** > **IOT hub'ları** > **uygulamanızın IOT hub'ı**, cihaz listesi, IOT hub'ı düğümünde gösterilir.

    ![Cihaz Listesi](media/iot-hub-visual-studio-cloud-device-messaging/device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>CİHAZDAN buluta iletileri izlemeye

Cihazınızın IOT Hub'ına gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. IOT hub'ı veya cihaza sağ tıklayıp **D2C iletisini İzlemeyi Başlat**.

    ![D2C iletisini izlemeye başlama](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message.png)

2. İzlenen iletilerin gösterilecek **IOT hub'ı** çıkış bölmesi.

    ![İzleme D2C ileti sonucu](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result.png)

3. İzlemeyi durdurmak için herhangi bir IOT hub'ı veya cihaz üzerinde sağ tıklayın ve seçmek için **D2C iletisini İzlemeyi Durdur**.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

Cihazınız için IOT hub'ınızdan ileti göndermek için bu adımları izleyin:

1. Cihazınızı sağ tıklayıp **C2D iletisi gönder**.

    ![C2D ileti gönder](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message.png)

2. İleti giriş kutusuna girin.

3. Sonuçları gösterilecek **IOT hub'ı** çıkış bölmesi.

    ![C2D ileti sonucu Gönder](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result.png)

## <a name="next-steps"></a>Sonraki adımlar

CİHAZDAN buluta iletileri izlemeye ve Azure IOT Hub ve IOT cihaz arasında bulut-cihaz iletilerini göndermek öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]