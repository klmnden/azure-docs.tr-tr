---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 983c65ba6e8b87f1dd66fcfdb50eac088ffab5d0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49960230"
---
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. IoT Edge modülü, kapsayıcı olarak uygulanan yürütülebilir bir pakettir. Bu bölümde, simülasyon cihazınız için telemetri oluşturacak bir modülün dağıtımı yapacaksınız.

1. Azure portalında IoT Hub'ınıza gidin.
1. **Otomatik Cihaz Yönetimi** altında **IoT Edge**’e gidin ve IoT Edge cihazınızı seçin.
1. **Modülleri Ayarlama**'yı seçin.
1. **Modül Ekle** adımının **Dağıtım Modülleri** bölümünde **Ekle**'ye tıklayın ve ardından **IoT Edge Modülü**'nü seçin.
1. **Ad** alanına `tempSensor` girin.
1. **Görüntü URI'si** alanına `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` girin.
1. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.

   ![Adı ve görüntü URI'sini girdikten sonra IoT Edge modülünü kaydedin](./media/iot-edge-deploy-module/name-image.png)

1. **Modül Ekle** adımına dönüp **İleri**’yi seçin.
1. **Rotaları belirtin** adımında tüm modüllerden gelen tüm iletileri IoT Hub'a gönderen varsayılan rotaya sahip olmanız gerekir. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

1. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.
1. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Hizmeti ilk kez başlattığınızda oluşturulan edgeAgent modülüne ek olarak **edgeHub** ve **tempSensor** adlı başka bir çalışma zamanını modülünü görmeniz gerekir.

   ![Dağıtılan modüller listesinde tempSensor modülünü görüntüleme](./media/iot-edge-deploy-module/deployed-modules.png)
