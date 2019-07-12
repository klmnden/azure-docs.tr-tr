---
title: Azure IOT araçları ile birlikte Visual Studio Code için Mesajlaşma Azure IOT hub'ı bulut cihaz yönetme | Microsoft Docs
description: Cihaz bulut iletilerini izlemek ve bulut, Azure IOT hub'daki cihaz iletilerini göndermek için Visual Studio Code için Azure IOT araçları kullanmayı öğrenin.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/18/2019
ms.author: junhan
ms.openlocfilehash: 1289e9c8f8cfc9360c9b2325507b43bab3a69028
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838500"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>IOT Hub ve cihaz arasında ileti göndermek ve almak için Visual Studio Code için Azure IOT araçları kullanın

![Uçtan uca diyagramı](./media/iot-hub-vscode-iot-toolkit-cloud-device-messaging/e-to-e-diagram.png)

[Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) , IOT hub'ı Yönetim ve IOT uygulama geliştirmeyi kolaylaştırır kullanışlı bir Visual Studio Code uzantısı. Bu makalede Visual Studio Code için Azure IOT araçları arasındaki Cihazınızı IOT hub'ınıza ileti göndermek ve almak için nasıl kullanılacağı ele alınmaktadır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

CİHAZDAN buluta iletileri izlemeye yönelik ve bulut-cihaz iletilerini göndermek için Visual Studio Code için Azure IOT araçları kullanmayı öğrenin. CİHAZDAN buluta iletileri Cihazınızı toplar ve ardından IOT hub'ınıza gönderir sensör verilerini olabilir. Bulut-cihaz iletilerini, IOT hub'ınıza cihazınıza gönderen cihazınıza bağlı bir LED yanıp sönme komutları olabilir.

## <a name="what-you-will-do"></a>Neler yapabileceği

* CİHAZDAN buluta iletileri izlemeye yönelik Visual Studio Code için Azure IOT araçları kullanın.

* Bulut-cihaz iletilerini göndermek için Visual Studio Code için Azure IOT araçları kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

* Etkin bir Azure aboneliği.

* Azure IOT hub, aboneliğiniz altında.

* [Visual Studio Code](https://code.visualstudio.com/)

* [VS Code için Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) veya [Visual Studio Code'da bu bağlantının açılabilmesi](vscode:extension/vsciot-vscode.azure-iot-tools).

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

1. İçinde **Gezgini** görüntülemek VS Code, genişletme **Azure IOT Hub cihazları** sol alt köşedeki bölümünde.

2. Tıklayın **IOT Hub'ı seçin** bağlam menüsünde.

3. Bir açılır pencere, Azure'da ilk kez oturum açarken izin vermek için sağ alt köşedeki gösterilir.

4. Oturum açtıktan sonra Azure abonelik listesi gösterilir ve ardından Azure aboneliği ve IOT hub'ı seçin.

5. Cihaz listesinde gösterilecek **Azure IOT Hub cihazları** birkaç saniye içinde sekmesi.

   > [!Note]
   > Ayrıca, ayarlamayı tamamlamak için **IoT Hub Bağlantı Dizesini Ayarla**'yı seçebilirsiniz. Girin **iothubowner** İlkesi açılır pencerede bağlanır IOT Cihazınızı IOT hub'ının bağlantı dizesi.

## <a name="monitor-device-to-cloud-messages"></a>CİHAZDAN buluta iletileri izlemeye

Cihazınızın IOT hub'ına gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. Cihazınızı sağ tıklayıp **Başlat yerleşik olay uç nokta izleme**.

2. İzlenen iletilerin gösterilecek **çıkış** > **Azure IOT hub'ı Araç Seti** görünümü.

3. İzlemeyi durdurmak için sağ **çıkış** görüntüleyebilir ve seçebilir **Durdur yerleşik olay uç nokta izleme**.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

Cihazınız için IOT hub'ınızdan ileti göndermek için bu adımları izleyin:

1. Cihazınızı sağ tıklayıp **cihaza C2D iletisi gönder**.

2. İleti giriş kutusuna girin.

3. Sonuçları gösterilecek **çıkış** > **Azure IOT hub'ı Araç Seti** görünümü.

## <a name="next-steps"></a>Sonraki adımlar

CİHAZDAN buluta iletileri izlemeye ve Azure IOT Hub ve IOT cihaz arasında bulut-cihaz iletilerini göndermek öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]