---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/7/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 1a750a97cdc940c0f0a3d7e33d6be0d33f811425
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53108031"
---
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. IoT Edge modülü, kapsayıcı olarak uygulanan yürütülebilir bir pakettir. Bu bölümde, biz önceden derlenmiş bir modülden dağıtma [Azure Marketi bölümünde IOT Edge modülleri](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Bu modül, sanal cihazınız için telemetri oluşturur.

1. Azure portalında girin `Simulated Temperature Sensor` arama ve açık Market sonucu.

   ![Azure portal Search'te benzetimli sıcaklık algılayıcısı](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. İçinde **abonelik** alanında, zaten değilse, kullanmakta olduğunuz, IOT Hub ile aboneliği seçin.

3. İçinde **IOT hub'ı** alanında, zaten değilse, kullanmakta olduğunuz, IOT Hub adını seçin.

4. Tıklayarak **cihaz bulma**, IOT Edge Cihazınızı seçin (adlı `myEdgeDevice`), ardından **Oluştur**.

5. İçinde **Modül Ekle** adım Sihirbazı'nın tıklayın **SimulatedTemperatureSensor** yapılandırma ayarlarını doğrulamak için modül **Kaydet** seçip**Sonraki**.

6. İçinde **yolları belirtin** adım Sihirbazı'nın yolları düzgün şekilde ayarlandığı tüm iletileri tüm modüllerden IOT Hub'ına gönderir. varsayılan rota ile doğrulama (`$upstream`). Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

7. İçinde **gözden geçirme dağıtım** seçin, adım **Gönder**.

8. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Hizmet başlatıldığında, oluşturulan edgeAgent modülünün yanı sıra adlı başka bir çalışma zamanı modülü görmeniz gerekir **edgeHub** ve **SimulatedTemperatureSensor** listelenen modülü.

   Bu, gösterilecek yeni modüller için birkaç dakika sürebilir. IOT Edge cihazı buluttan yeni dağıtım bilgilerini almak için kapsayıcıları başlatın ve ardından yeni durumuna IOT Hub'ına rapor gerekir. 

   ![Dağıtılan modüller listesinde SimulatedTemperatureSensor görüntüle](./media/iot-edge-deploy-module/deployed-modules-marketplace-temp.png)
