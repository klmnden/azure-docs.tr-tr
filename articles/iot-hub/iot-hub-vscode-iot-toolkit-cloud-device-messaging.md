---
title: Visual Studio Code için Azure IOT Toolkit uzantısını ile Mesajlaşma Azure IOT hub'ı bulut cihaz yönetme | Microsoft Docs
description: Cihaz bulut iletilerini izlemek ve bulut, Azure IOT hub'daki cihaz iletilerini göndermek için Visual Studio Code için Azure IOT Toolkit uzantısını kullanmayı öğrenin.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 7/20/2018
ms.author: junhan
ms.openlocfilehash: 7bcb6eebdb6ceba6b07aeb19c1a74309fd4227d0
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206129"
---
# <a name="use-azure-iot-toolkit-extension-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Visual Studio Code için Azure IOT Toolkit uzantısını IOT Hub ve cihaz arasında ileti göndermek ve almak için kullanın

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IOT Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) , IOT hub'ı yönetimini kolaylaştırır kullanışlı bir Visual Studio Code uzantısı. Bu makalede, Visual Studio Code için Azure IOT Toolkit uzantısını arasındaki Cihazınızı IOT hub'ınıza ileti göndermek ve almak için nasıl kullanılacağını odaklanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

CİHAZDAN buluta iletileri izlemeye yönelik ve bulut-cihaz iletilerini göndermek için Visual Studio Code için Azure IOT Toolkit uzantısını kullanmayı öğrenin. CİHAZDAN buluta iletileri Cihazınızı toplar ve ardından IOT hub'ınıza gönderir sensör verilerini olabilir. Bulut-cihaz iletilerini, IOT hub'ınıza cihazınıza gönderen cihazınıza bağlı bir LED yanıp sönme komutları olabilir.

## <a name="what-you-will-do"></a>Neler yapabileceği

- CİHAZDAN buluta iletileri izlemeye yönelik Visual Studio Code için Azure IOT Toolkit uzantısını kullanın.
- Bulut-cihaz iletilerini göndermek için Visual Studio Code için Azure IOT Toolkit uzantısını kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- Etkin bir Azure aboneliği.
- Azure IOT hub, aboneliğiniz altında.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

1. İçinde **Gezgini** görüntülemek VS Code, genişletme **Azure IOT Hub cihazları** sol alt köşedeki bölümünde.
1. Tıklayın **IOT Hub'ı seçin** bağlam menüsünde.
1. Bir açılır pencere, Azure'da ilk kez oturum açarken izin vermek için sağ alt köşedeki gösterilir.
1. Oturum açtıktan sonra Azure abonelik listesi gösterilir ve ardından Azure aboneliği ve IOT hub'ı seçin.
1. Cihaz listesinde gösterilecek **Azure IOT Hub cihazları** birkaç saniye içinde sekmesi.

   > [!Note]
   > Ayrıca, ayarlamayı tamamlamak için **IoT Hub Bağlantı Dizesini Ayarla**'yı seçebilirsiniz. Açılır pencerede bağlanır IOT Cihazınızı IOT hub'ının bağlantı dizesini girin.
   
## <a name="monitor-device-to-cloud-messages"></a>CİHAZDAN buluta iletileri izlemeye

Cihazınızın IOT hub'ına gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. Cihazınızı sağ tıklayıp **D2C iletisini İzlemeyi Başlat**.
1. İzlenen iletilerin gösterilecek **çıkış** > **Azure IOT Toolkit** görünümü.
1. İzlemeyi durdurmak için sağ **çıkış** görüntüleyebilir ve seçebilir **D2C iletisini İzlemeyi Durdur**.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

Cihazınız için IOT hub'ınızdan ileti göndermek için bu adımları izleyin:

1. Cihazınızı sağ tıklayıp **cihaza C2D iletisi gönder**. 
1. İleti giriş kutusuna girin.
1. Sonuçları gösterilecek **çıkış** > **Azure IOT Toolkit** görünümü.

## <a name="next-steps"></a>Sonraki adımlar

CİHAZDAN buluta iletileri izlemeye ve Azure IOT Hub ve IOT cihaz arasında bulut-cihaz iletilerini göndermek öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
